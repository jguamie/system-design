# Replication
Replication is where we maintain copies of the same data on multiple machines (replicas). Replication allows a system to:
* **Reduce latency.** Keep data geographically close to users.
* **Increase availability.** Allow the system to continue to function when some parts have failed.
* **Increase read throughput.** Scale the number of machines that can serve read queries.

The challenge of replication is how to handle changes to the data. There are three main approaches to replication: single-leader replication, multi-leader replication, and leaderless replication.
## Single-Leader Replication
In single-leader replication, clients send all writes to a single leader node, which communicates changes to all replica follower nodes. Clients can read from any replica. Examples of systems that use this mode of replication include relational databases (MySQL, PostgreSQL, Oracle, SQL Server), some nonrelational databases (MongoDB), and message brokers (Kafka, RabbitMQ).
### New Followers
Systems will periodically take a snapshot of the leader's database. This snapshot is used to create the new follower node. The leader will maintain a position (PostgreSQL: log sequence number, MySQL: binlog coordinates) in its replication log. The follower uses this position to ask the leader for all data changes that have happened since the last snapshot.

When a node goes down, this same method is used to restore a node.
### Leader Failover
Refer to the notes on [Raft Distributed Consensus](https://github.com/jguamie/system-design/blob/master/notes/raft-distributed-consensus.md).
### Synchronous vs. Asynchronous Replication
In synchronous replication, the leader waits until the follower has confirmed that it received the write before reporting success to the client and making the write visible to other clients. In asynchronous replication, the leader sends the write to the follower but doesn't wait for a response from the follower.

It is impractical for all followers to be synchronous as one node outage would cause the whole system to stall. In practice, only one of the followers is synchronous while the others are asynchronous. If the synchronous follower becomes slow or unresponsive, one of the asynchronous followers is changed to be synchronous. In this scenario, at least two nodes are guaranteed to have an up-to-date copy of the data (semi-synchronous). Fully asynchronous systems are also used in practice.
### Eventual Consistency
The delay between a write on the leader and replicated onto a follower is known as replication lag. In most cases, this lag is only a fraction of a second. This temporary inconsistency is known as eventual consistency.
### Read-After-Write Consistency
Read-after-write consistency (or read-your-writes consistency) is a guarantee that clients will always see any updates they've made. This is implemented as follows:
1. When a client sends read requests for data it previously modified, the leader will process the request.
1. If the time between the read request and the last update is within one minute, the leader must process the request.
1. For more fine-grained control, monitor the replication lag on followers. If the time between the read request and the last update is within the replication lag window, the leader must process the request.
### Monotonic Reads Consistency
When reading from multiple asynchronous followers, a client can see data moving backwards in time. Monotonic reads consistency prevents this by ensuring each client processes its reads from the same replica. This can be accomplished by routing clients to replicas based on a hash of the client/user ID. if a replica fails, the clients queries would be rerouted to a different replica.
## Multi-Leader Replication
In multi-leader replication, clients can send writes to one of several leader nodes, which communicate changes to the other leader nodes and all replica follower nodes. This mode is for expanding beyond a single datacenter. Multi-leader replication does not make sense within a single datacenter due to the added complexity.
### Conflict Resolution
With multi-leader replication, write conflicts become a problem. The system must have a means to detect conflicts and resolve them.
## Leaderless Replication
In leaderless replication, clients send each write to multiple nodes. Clients read from multiple nodes in parallel and if a discrepancy is detected, it will correct the nodes with stale data. This mode was popularized by Amazon Dynamo--Dynamo inspired Riak and Cassandra, which are Dynamo-style database systems. 
# References
1. [Chapter 5, Replication - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
