# CDR / CMR Ingestion System – Full Documentation

This document explains **end‑to‑end** how the CDR/CMR ingestion system works on this Linux host.
It covers **architecture, concepts, setup, operations, failure handling, performance tuning,
and what should / should not be modified**.

---

## 1. High‑level architecture

```
CUCM / Export Source
        |
        v
SFTP upload → /home/sftp-box/CDR
        |
        v
systemd PATH unit (cdr-watcher.path)
        |
        v
systemd SERVICE unit (cdr-watcher.service)
        |
        v
Python ingestion script
        |
        +--> MongoDB (records inserted)
        |
        +--> CDR_Processed / CDR_Failed
```

Key design principles:
- **Event‑driven** (no polling loops)
- **Stateless per run**
- **Idempotent** (files processed once)
- **Safe under high volume**
- **Recoverable after failures**

---

## 2. Core systemd concepts (VERY IMPORTANT)

### 2.1 systemd `.path` unit (Watcher)

A `.path` unit tells systemd:

> *“Watch a filesystem path and trigger another unit when it changes.”*

In this system:
- `cdr-watcher.path` watches `/home/sftp-box/CDR`
- When files are created or modified, it triggers `cdr-watcher.service`

**Important characteristics**
- It does NOT run continuously
- It does NOT know which file changed
- It only triggers the service
- Multiple file events can happen before a single trigger

That is why the Python script **scans the directory**, instead of relying on a single filename.

---

### 2.2 systemd `.service` unit (Executor)

A `.service` unit defines **what command to run**.

In this system:
- `cdr-watcher.service` runs the Python ingestion script
- `Type=oneshot` means:
  - run once
  - exit
  - systemd considers it successful if exit code = 0

A oneshot service being `inactive (dead)` is **NORMAL and correct**.

---

### 2.3 Why systemd rate‑limits services

systemd protects the host by rate‑limiting services that start too often.

If a service is started many times in a short window:
- systemd triggers `start-limit-hit`
- the unit becomes **blocked**
- manual reset is required

This is NOT a bug – it is protection.

---

## 3. Debouncing and trigger storms

When many files arrive quickly:
- `PathModified` can fire multiple times
- systemd may attempt to start the service repeatedly
- start-limit-hit occurs

### The fix (implemented)
```ini
TriggerLimitIntervalSec=30
TriggerLimitBurst=1
```

Meaning:
- at most **1 trigger every 30 seconds**
- the script processes ALL pending files per run

This is the correct pattern for high‑volume ingestion.

---

## 4. Concurrency protection (flock)

To prevent overlapping executions:

```ini
ExecStart=/usr/bin/flock /run/cdr-watcher.lock   /opt/cdr_watcher/venv/bin/python /opt/cdr_watcher/handle_new_cdr.py
```

This guarantees:
- only **one ingestion run at a time**
- safe coexistence of:
  - live ingestion
  - backfill ingestion

---

## 5. Python ingestion logic (overview)

### 5.1 File detection
- Scans input directory
- Identifies:
  - `cdr_*` files
  - `cmr_*` files
- Ignores unknown files

### 5.2 File format
Each file:
- Line 1 → column headers
- Line 2 → data types
- Line 3+ → records

### 5.3 Mongo insertion
- Uses `pymongo`
- Inserts in batches
- Uses `ordered=False` for performance
- Reuses Mongo client per run

### 5.4 File lifecycle
| Outcome | Destination |
|-------|-------------|
| Success | `/home/sftp-box/CDR_Processed` |
| Failure | `/home/sftp-box/CDR_Failed` |

### 5.5 Idempotency
A state file tracks processed files:
```
/var/lib/cdr-watcher/seen.txt
```

Prevents re‑processing after restarts.

---

## 6. Virtual environment (PEP 668 compliant)

System Python is protected.
Dependencies are installed in a venv:

```
/opt/cdr_watcher/venv
```

Used by systemd directly.

**Never install Python packages system‑wide with `pip` on Ubuntu 22.04+**

---

## 7. Environment configuration

All sensitive values live in:
```
/etc/cdr-watcher.env
```

Example:
```ini
MONGO_URI=mongodb://admin:secret@127.0.0.1:27017/?authSource=admin
MONGO_DB=cucm
MONGO_CDR_COLLECTION=cdr_records
MONGO_CMR_COLLECTION=cmr_records
```

Permissions:
```bash
chmod 600 /etc/cdr-watcher.env
```

---

## 8. Operational commands (DAY‑TO‑DAY)

### 8.1 Check status
```bash
systemctl status cdr-watcher.path
systemctl status cdr-watcher.service
```

### 8.2 Start live ingestion
```bash
systemctl start cdr-watcher.path
```

### 8.3 Stop live ingestion
```bash
systemctl stop cdr-watcher.path
```

### 8.4 View logs
```bash
journalctl -u cdr-watcher.service
journalctl -fu cdr-watcher.service
```

---

## 9. Recovery from failures (CRITICAL)

### 9.1 If service stops due to start-limit-hit
```bash
systemctl reset-failed cdr-watcher.service cdr-watcher.path
systemctl start cdr-watcher.path
```

This is REQUIRED. Restart alone is NOT enough.

---

## 10. Backfill processing (historical data)

Backfill uses:
- same Python script
- different directories
- separate state file
- same MongoDB

Backfill runs **without stopping live ingestion**.

Locking guarantees safety.

---

## 11. Performance tuning

### 11.1 Disable file stability wait for backfill
```bash
STABLE_SECONDS=0
```

### 11.2 Keep stability wait for live ingestion
Default:
```bash
STABLE_SECONDS=2
```

Prevents partial file ingestion.

---

## 12. What is SAFE to change

✅ Safe:
- Batch size
- Trigger debounce interval
- Mongo indexes
- Logging verbosity
- Backfill directories

---

## 13. What is NOT recommended

❌ Not recommended:
- Removing debounce limits
- Removing flock lock
- Using `--break-system-packages`
- Installing pip packages system‑wide
- Running multiple ingestion scripts concurrently
- Using polling loops instead of systemd path units

---

## 14. Why this design is correct

This design:
- scales to tens of thousands of files
- survives restarts
- avoids data duplication
- avoids race conditions
- respects OS‑level safeguards
- is fully observable and auditable

---

## 15. Final notes

This system behaves more like a **log ingestion pipeline** than a simple script.
systemd is doing exactly what it should do — protecting the system.

Once configured correctly, it will:
- run for months without intervention
- recover cleanly after crashes
- handle both live and historical data safely

---

**End of document**
