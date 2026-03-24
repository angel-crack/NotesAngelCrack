## Full Setup, Concepts, and Re‑Provisioning Guide (Red Hat)

This document explains how to deploy and operate a Linux-based pipeline that ingests Cisco CUCM CDR/CMR files and stores them in MongoDB.

---
# 1. Overview

CUCM exports files that are uploaded to:

```bash
/home/ruadmin/CDR
```

The system must:

1. Detect new files
2. Parse records
3. Insert records into MongoDB
4. Move files to processed folders

---

# 2. Architecture

```bash
CUCM
 |
 | SFTP
 v
/home/ruadmin/CDR
 |
 | systemd path watcher
 v
cdr-watcher.path
 |
 v
cdr-watcher.service
 |
 v
Python ingestion script
 |
 +--> MongoDB
 +--> CDR_Processed
 +--> CDR_Failed
```

---
# 3. Linux Concepts

## systemd

systemd is Linux’s service manager.

Useful commands:

```bash
systemctl start SERVICE
systemctl stop SERVICE
systemctl restart SERVICE
systemctl status SERVICE
```

---

## systemd service

A service runs a command.

Example:

```bash
run python script
run web server
```

Service files are located in:

```bash
/etc/systemd/system
```

Example:

```bash
cdr-watcher.service
```

---

## systemd path unit

A path unit watches a directory and triggers a service.

Example:

```bash
watch /home/ruadmin/CDR
```

When files appear → run service.

---

# 4. Install Requirements

```bash
sudo dnf update -y
sudo dnf install -y python3 python3-pip mongodb-mongosh
```

---

# 5. Create Directories

```bash
mkdir -p /opt/cdr_watcher
mkdir -p ~/CDR
mkdir -p ~/CDR_Processed
mkdir -p ~/CDR_Failed
mkdir -p /var/lib/cdr-watcher
```

---

# 6. Create Python Virtual Environment

```bash
cd /opt/cdr_watcher
python3 -m venv venv
source venv/bin/activate
pip install pymongo
```

---

# 7. Environment File

Create:
```bash
/etc/cdr-watcher.env
```

Example:

```bash
MONGO_URI=mongodb://admin:secret@127.0.0.1:27017/?authSource=admin
MONGO_DB=cucm
MONGO_CDR_COLLECTION=cdr_records
MONGO_CMR_COLLECTION=cmr_records
```

Secure it:

```bash
chmod 600 /etc/cdr-watcher.env
```

---

# 8. Python Script

Save as:

```bash
/opt/cdr_watcher/handle_new_cdr.py
```



```python
#!/usr/bin/env python3
#!/usr/bin/env python3

import os
import csv
import time
import shutil
import traceback
from pathlib import Path
from pymongo import MongoClient

IN_DIR = Path("/home/ruadmin/CDR")
PROCESSED_DIR = Path("/home/ruadmin/CDR_Processed")
FAILED_DIR = Path("/home/ruadmin/CDR_Failed")
STATE_FILE = Path("/var/lib/cdr-watcher/seen.txt")

MONGO_URI = os.getenv("MONGO_URI")
MONGO_DB = os.getenv("MONGO_DB","cucm")
COLL_CDR = os.getenv("MONGO_CDR_COLLECTION","cdr_records")
COLL_CMR = os.getenv("MONGO_CMR_COLLECTION","cmr_records")

client = MongoClient(MONGO_URI)
db = client[MONGO_DB]
cdr_coll = db[COLL_CDR]
cmr_coll = db[COLL_CMR]

MAX_PROCESSED_FILES = 20

def load_seen():
    if not STATE_FILE.exists():
        return set()
    return set(STATE_FILE.read_text().splitlines())

def save_seen(seen):
    STATE_FILE.parent.mkdir(parents=True, exist_ok=True)
    STATE_FILE.write_text("\n".join(sorted(seen)))


def cleanup_processed_dir():
    files = [f for f in PROCESSED_DIR.iterdir() if f.is_file()]
    if len(files) <= MAX_PROCESSED_FILES:
        return

    files_sorted = sorted(files, key=lambda p: p.stat().st_mtime)
    files_to_delete = files_sorted[: len(files_sorted) - MAX_PROCESSED_FILES]

    for old_file in files_to_delete:
        try:
            old_file.unlink()
        except Exception as err:
            print("CLEANUP_FAILED", old_file, err)

def process_file(path):

    try:
        with open(path) as f:
            reader = csv.reader(f)
            headers = next(reader)
            next(reader)

            docs = []
            for row in reader:
                docs.append(dict(zip(headers,row)))

        if not docs:
            raise ValueError("no data rows found")

        if path.name.startswith("cdr"):
            cdr_coll.insert_many(docs, ordered=False)
        else:
            cmr_coll.insert_many(docs, ordered=False)

        shutil.move(str(path), PROCESSED_DIR/path.name)
        cleanup_processed_dir()
        return True

    except StopIteration:
        print(f"FAILED file={path.name} reason=empty_or_invalid_csv")
        shutil.move(str(path), FAILED_DIR/path.name)
        return False
    except ValueError as e:
        print(f"FAILED file={path.name} reason={e}")
        shutil.move(str(path), FAILED_DIR/path.name)
        return False
    except csv.Error as e:
        print(f"FAILED file={path.name} reason=csv_parse_error detail={e}")
        shutil.move(str(path), FAILED_DIR/path.name)
        return False
    except Exception as e:
        print(f"FAILED file={path.name} error={type(e).__name__}: {e}")
        print(traceback.format_exc())
        shutil.move(str(path), FAILED_DIR/path.name)
        return False

def main():

    seen = load_seen()

    for f in IN_DIR.iterdir():
        if not f.is_file():
            continue

        if f.name in seen:
            continue

        if process_file(f):
            seen.add(f.name)

    save_seen(seen)

if __name__ == "__main__":
    main()
```

Make executable:

```bash
chmod +x /opt/cdr_watcher/handle_new_cdr.py
```

---

# 9.  systemd Service

```bash
/etc/systemd/system/cdr-watcher.service
```

```
[Unit]
Description=Run CDR/CMR handler script
StartLimitIntervalSec=30
StartLimitBurst=20

[Service]
Type=oneshot
EnvironmentFile=/etc/cdr-watcher.env
Environment=PYTHONUNBUFFERED=1
ExecStart=/usr/bin/flock /run/cdr-watcher.lock /opt/cdr_watcher/venv/bin/python /opt/cdr_watcher/handle_new_cdr.py
User=root
Group=root

```

---

# 10. systemd Path

```bash
/etc/systemd/system/cdr-watcher.path
```

```
[Unit]
Description=Watch CDR directory for new files

[Path]
PathModified=/home/ruadmin/CDR
Unit=cdr-watcher.service

TriggerLimitIntervalSec=30
TriggerLimitBurst=1

[Install]
WantedBy=multi-user.target
```

---

# 11. Reload systemd

```bash
sudo systemctl daemon-reload
```

---

# 12. Enable Watcher

```bash
sudo systemctl enable cdr-watcher.path
sudo systemctl start cdr-watcher.path
```

---

# 13. Check Status

```bash
systemctl status cdr-watcher.path
```

---

# 14. Test

```bash
touch /home/ruadmin/CDR/test
```

Check logs:

```bash
journalctl -u cdr-watcher.service
```

---

# 15. Recovery

If service stops due to start limit:

```bash
systemctl reset-failed cdr-watcher.service cdr-watcher.path
systemctl restart cdr-watcher.path
```

---

# 16. Logs

```bash
journalctl -fu cdr-watcher.service
```

---

# 17. Reprovision Checklist

```bash
1 Install python
2 Install mongo
3 Create directories
4 Create venv
5 Install pymongo
6 Copy python script
7 Create env file
8 Create service
9 Create path
10 reload systemd
11 enable watcher
```

System ready.
