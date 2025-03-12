| Unified Communications Manager (DB) | Unified Communications Manager (DB)   | 1500, 1501 / TCP | Database connection (1501 / TCP is the secondary connection)                                    |
| ----------------------------------- | ------------------------------------- | ---------------- | ----------------------------------------------------------------------------------------------- |
| Unified Communications Manager (DB) | Unified Communications Manager (DB)   | 1510 / TCP       | CAR IDS DB. CAR IDS engine listens on waiting for connection requests from the clients.         |
| Unified Communications Manager (DB) | Unified Communications Manager (DB)   | 1511 / TCP       | CAR IDS DB. An alternate port used to bring up a second instance of CAR IDS during upgrade.     |
| Unified Communications Manager (DB) | Unified Communications Manager (DB)   | 1515 / TCP       | Database replication between nodes during installation                                          |
| Cisco Extended Functions (QRT)      | Unified Communications Manager (DB)   | 2552 / TCP       | Allows subscribers to receive Cisco Unified Communications Manager database change notification |
| Unified Communications Manager (DB) | Unified Communications Manager (CDLM) | 8001 / TCP       | Client database change notification                                                             |


This means checking the **/etc/hosts** file, **.rhosts** and **sqlhosts** files, through CUCM reporting.

|                            |                                                                                                                                                                                                                  |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /etc/hosts \| grep hos     | This file is used to locally resolve hostnames to IP addresses.  It should include the hostname and IP address of all nodes in the cluster including CUPS nodes.                                                 |
| cat /home/informix/.rhosts | A list of hostnames which are trusted to make database connections                                                                                                                                               |
| $INFORMIXDIR/etc/sqlhosts  | Full list of CCM servers for replication.  Servers here should have the correct hostname and node id (populated from the process node table).  This is used to determine to which servers replicates are pushed. |
