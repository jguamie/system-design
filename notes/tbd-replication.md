TBD
# Replication
There are three main approaches to replication: single-leader replication, multi-leader replication, and leaderless replication.
## Single-Leader Replication
In single-leader replication, clients send all writes to a single leader node, which communicates changes to all replica follower nodes. Clients can read from any replica.
## Multi-Leader Replication
In multi-leader replication, clients can send writes to one of several leader nodes, which communicate changes to the other leader nodes and all replica follower nodes.
## Leaderless Replication
In leaderless replication, clients send each write to multiple nodes. Clients read from multiple nodes in parallel and if a discrepency is detected, it will correct the nodes with stale data. 
# References
1. [Chapter 5, Replication - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
