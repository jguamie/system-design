TBD
# Replication
Replication is where we maintain copies of the same data on multiple machines. Replication allows a system to:
* **Reduce latency.** Keep data geographically close to users.
* **Increase availability.** Allow the system to continue to function when some parts have failed.
* **Increase read throughput.** Scale the number of machines that can serve read queries.

The challenge of replication is how to handle changes to the data. There are three main approaches to replication: single-leader replication, multi-leader replication, and leaderless replication.
## Single-Leader Replication
In single-leader replication, clients send all writes to a single leader node, which communicates changes to all replica follower nodes. Clients can read from any replica. Examples of systems that use this mode of replication include relational databases (MySQL, PostgreSQL, Oracle, SQL Server), some nonrelational databases (MongoDB), and message brokers (Kafka, RabbitMQ).
### Synchronous vs. Asynchronous Replication
In synchronous replication, the leader waits until the follower has confirmed that it received the write before reporting success to the client and making the write visible to other clients. In asynchronous replication, the leader sends the write to the follower but doesn't wait for a response from the follower.

It is impractical for all followers to be synchronous as one node outage would cause the whole system to stall. In practice, only one of the followers is synchronous while the others are asynchronous. If the synchronous follower becomes slow or unresponsive, one of the asynchronous followers is changed to be synchronous. In this scenario, at least two nodes are guaranteed to have an up-to-date copy of the data (semi-synchronous). Fully asynchronous systems are also used in practice.
### New Followers
Systems will periodically take a snapshot of the leader's database. This snapshot is used to create the new follower node. The leader will maintain a position (PostgreSQL: log sequence number, MySQL: binlog coordinates) in its replication log. The follower uses this position to ask the leader for all data changes that have happened since the last snapshot.

When a node goes down, this same method is used to restore a node.
### Leader Failover
Refer to the notes on [Raft Distributed Consensus](https://github.com/jguamie/system-design/blob/master/notes/raft-distributed-consensus.md).
## Multi-Leader Replication
In multi-leader replication, clients can send writes to one of several leader nodes, which communicate changes to the other leader nodes and all replica follower nodes.
## Leaderless Replication
In leaderless replication, clients send each write to multiple nodes. Clients read from multiple nodes in parallel and if a discrepency is detected, it will correct the nodes with stale data. 
# References
1. [Chapter 5, Replication - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
