# General

Run the following command on the publisher to check the replication:

```powershell
admin:utils dbreplication runtimestate

Server Time: Tue Apr  2 15:16:04 CDT 2024

Cluster Replication State: BROADCAST SYNC ended at: 2021-12-07-17-47
     Sync Result: SYNC COMPLETED on 749 tables out of 749
     Sync Status: All Tables are in sync
     Use CLI to see detail: 'file view activelog cm/trace/dbl/20211207_174435_dbl_repl_output_Broadcast.log'

DB Version: ccm14_0_1_11900_132

Repltimeout set to: 300s
PROCESS option set to: 1


Cluster Detailed View from cucm1.dcloud.cisco.com (2 Servers):

                                      PING      DB/RPC/   REPL.    Replication    REPLICATION SETUP
SERVER-NAME         IP ADDRESS        (msec)    DbMon?    QUEUE    Group ID       (RTMT) & Details
-----------         ----------        ------    -------   -----    -----------    ------------------
cucm1               198.18.133.3      0.020     Y/Y/Y     0        (g_2)          (2) Setup Completed
cucm2               198.18.133.33     0.256     Y/Y/Y     579      (g_5)          (2) Setup Completed

```

[+] If broadcast sync is not recent

```powershell
Cluster Replication State: BROADCAST SYNC ended at: 2021-12-07-17-47
```

We will run:

```powershell
admin:utils dbreplication status


Replication status check is now running in background.
Use command 'utils dbreplication runtimestate' to check its progress

The final output will be in file cm/trace/dbl/sdi/ReplicationStatus.2024_04_02_15_19_23.out

Please use "file view activelog cm/trace/dbl/sdi/ReplicationStatus.2024_04_02_15_19_23.out " command to see the output
```

Then, with this one we check, note that it is 680 from 749 tables checked

``` c
admin:utils dbreplication runtimestate

Server Time: Tue Apr  2 15:20:34 CDT 2024

Cluster Replication State: Replication status command started at: 2024-04-02-15-19
     Replication status command in PROGRESS. Checked 680 tables out of 749
     Last Completed Table: applicationuserdevicemap
     No Errors or Mismatches found.

     Use 'file view activelog cm/trace/dbl/sdi/ReplicationStatus.2024_04_02_15_19_23.out' to see the details


DB Version: ccm14_0_1_11900_132

Repltimeout set to: 300s
PROCESS option set to: 1
```

