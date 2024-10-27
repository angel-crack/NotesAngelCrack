# Basics

## Topology

Fully Meshed topology, all servers are logically connected. So, every server can update the others if it is a User Facing Features (CFA). for NonUser Facing Features (Route Pattern) still requires publisher to be accessible.

Every node needs to send this changes to all nodes.

![[Topology.png]]
## Check status

There are three different ways to check the current replication status:
### RTMT

Voice/Video > Servie > Database Summary

![[Pasted image 20240402135701.png]]

### Command Line

```powershell
admin:show perf query class "Number of Replicates Created and State of Replication"
==>query class :

 - Perf class (Number of Replicates Created and State of Replication) has instances and values:
    ReplicateCount  -> Number of Replicates Created   = 749
    ReplicateCount  -> Replicate_State                = 2
```

### CUCM GUI

Cisco Unified Reporting > Unified CM Database Status > Generate Report

This is the complete one when we can see a lot of things. 

![[Pasted image 20240402140604.png]]

![[Pasted image 20240402140627.png]]

![[Pasted image 20240402140653.png]]

![[Pasted image 20240402140710.png]]

![[Pasted image 20240402140725.png]]




## Status based on Number

| **Value** |              **Meaning**              |                                                                                                                                                                                                  **Description**                                                                                                                                                                                                   |
| :-------: | :-----------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     0     |         Initialization State          |                                                                                                                                           Replication is in the process of setting up. A  setup failure can occur if replication is in this state for more than an hour.                                                                                                                                           |
|     1     | The Number of replicates is incorrect |                                                                                                                                 Set up is still in progress. This state is rarely seen in versions 6.x and 7.x; in version 5.x, it indicates that the setup is still in progress.                                                                                                                                  |
|     2     |          Replication is good          |                                                                                                                                                       Logical connections are established and the tables are matched with the other servers on the cluster.                                                                                                                                                        |
|     3     |           Mismatched tables           | Logical connections are established but there is an uncertainty whether the tables match.<br><br>In versions 6.x and 7.x, all servers could show state 3 even if one server is down in  the cluster.<br><br>This issue can occur because the other servers are unsure whether there is an update to the User Facing Feature (UFF) that has not been passed from the subscriber to the other device in the cluster. |
|     4     |         Setup Failed/Dropped          |                                                                                                                                 Server no longer has an active logical connection in order to receive any database table across the network. No replication occurs in this state.                                                                                                                                  |

Refer to: [[Troubleshooting]]