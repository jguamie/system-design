# Replication
As systems scale, a single machine is no longer feasible to process large datasets or high query throughput. Partitioning (sharding) breaks up the dataset into smaller subsets. Each parition becomes its own small database. Partitioning spreads the data and query load across multiple machines. There are two main approaches to partitioning: key range paritioning and hash partitioning.
## Key Range Paritioning
Key range partitioning is where keys are sorted and partitioned by a range of keys. Each parition would own all keys from a minimum key to a maximum key. This allows for efficient key range queries. 

This approach has a risk of hot spots. Applications tend to access the same range of keys that are close together causing query load to be focused on a couple partitions. To mitigate this risk, partitions are rebalanced by splitting a range into two subranges (1 partition divided into 2 partitions).
## Hash Partitioning
Hash partitioning is where a hash function is applied to each key and partitioned by a range of hashes. Key range queries become inefficient as there is no ordering of keys. Instead, load is distributed more evenly in order to minimize hot spots.

Typically, the database will assign multiple partitions to a node. This improves dynamic load balancing across the nodes.
# References
1. [Chapter 6, Replication - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
