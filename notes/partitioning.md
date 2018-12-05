# Partitioning
As systems scale, a single machine is no longer feasible to process large datasets or high query throughput. Partitioning (sharding) breaks up the dataset into smaller subsets. Each partition becomes its own small database. Partitioning spreads the data and query load across multiple machines. There are two main approaches to partitioning: key range partitioning and hash partitioning.

Typically, the database will assign multiple partitions to a node. This improves dynamic load balancing across the nodes. If a leader-follower replication model is used, each partition's leader is assigned to one node and its followers are assigned to other nodes. Each node could be a leader to some partitions and a follower for other partitions.
## Key-Value Data Model
### Key Range Partitioning
Key range partitioning is where keys are sorted and partitioned by a range of keys. Each partition would own all keys from a minimum key to a maximum key. This allows for efficient key range queries. 

With this approach, a partition can have disproportionately high load (hot spot). In some cases, all load could end up on one partition. A specific range of keys that are close together can become very popular. If the database is partitioned by timestamp, all writes today would go to the same partition. To mitigate this risk, partitions can be rebalanced by splitting a range into two subranges (1 partition divided into 2 partitions). Another option is to prefix a key with a different name. For example, a timestamp key can be prefixed with a component name.
### Hash Partitioning
Hash partitioning is where a hash function is applied to each key and partitioned by a range of hashes (instead of range of keys). This approach came about to uniformly distribute data to remove hot spots. Key range queries become inefficient as there is no ordering of keys. Hash partitioning is implemented as consistent hashing or rendezvous hashing.

Even with hash partitioning, a single key can become very hot. Applications can use a simple technique to mitigate this: adding a random number at the beginning or end of the key. A two-digit decimal random number will split a key evenly across 100 different keys. This will distribute writes to different partitions. Reads will have to do additional work as they will have to read data from 100 keys and merge the data.
## Data Model with Secondary Indexes
Partitioning becomes more complicated when secondary indexes are involved. The secondary indexes need to have their own partitioning approach: document-based partitioning (local indexes) or term-based partitioning (global indexes).
### Document-Based Partitioning
Document-based partitioning (local indexes) are where the secondary indexes are stored in the same partition as the primary key and value. Each partition only maintains the secondary indexes of the datasets it controls. On write, only a single partition needs to be updated. On read, secondary indexes must be gathered across ALL partitions (scatter/gather). This makes read queries on secondary indexes very expensive.
### Term-Based Partitioning
Term-based partitioning (global indexes) are where secondary indexes are partitioned separately. A global index is partitioned, but different from the partitioning approach used for the primary index. On write, several partitions of the secondary index must be updated. On read, data can be served from a single partition. This makes read queries on secondary indexes efficient. At the same time, writes to secondary indexes become slower and more complicated.

DynamoDB uses global indexes. Updates to secondary indexes are asynchronous and therefore, eventually consistent.
# References
1. [Chapter 6, Partitioning - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
