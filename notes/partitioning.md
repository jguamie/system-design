# Partitioning
As systems scale, a single machine is no longer feasible to process large datasets or high query throughput. Partitioning (sharding) breaks up the dataset into smaller subsets. Each parition becomes its own small database. Partitioning spreads the data and query load across multiple machines. There are two main approaches to partitioning: key range paritioning and hash partitioning.
## Partitioning Key-Value Data
### Key Range Paritioning
Key range partitioning is where keys are sorted and partitioned by a range of keys. Each parition would own all keys from a minimum key to a maximum key. This allows for efficient key range queries. 

With this approach, a partition can have disproportionately high load (hot spot). In some cases, all load could end up on one partition. A specific range of keys that are close together can become very popular. If the database is partitioned by timestamp, all writes today would go to the same partition. To mitigate this risk, partitions can be rebalanced by splitting a range into two subranges (1 partition divided into 2 partitions). Another option is to prefix a key with a different name. For example, a timestamp key can be prefixed with a component name.
### Hash Partitioning
Hash partitioning is where a hash function is applied to each key and partitioned by a range of hashes (instead of range of keys). This approach came about to uniformly distribute data to remove hot spots. Key range queries become inefficient as there is no ordering of keys. Examples of hash partitioning techniques are consistent hashing and rendezvous hashing.

Typically, the database will assign multiple partitions to a node. This improves dynamic load balancing across the nodes. Even with hash partitioning, a single key can become very hot. Applications can use a simple technique to mitigate this: adding a random number at the beginning or end of the key. A two-digit decimal random number will split a key evenly across 100 different keys. This will distribute writes to different parititions. Reads will have to do additional work as they will have to read data from 100 keys and merge the data.
## Partitioning Data with Secondary Indexes
### Document-Based Partitioning

### Term-Based Partitioning

# References
1. [Chapter 6, Partitioning - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
