# Cache System
## Data Capacity
Estimate daily active users. Memory utilized per user. Queries per second (QPS).
Use the Pareto 80/20 Principle. 20% of daily active traffic will account for 80% of usage patterns.
## Design Goals
### Latency
### Consistency
[Read-After-Write Consistency in Amazon S3](https://shlomoswidler.com/2009/12/read-after-write-consistency-in-amazon.html)
#### Read-after-write Consistency
With read-after-write consistency, a newly-created object, file, or table row will immediately be visible to all clients, without any delays. Note: read-after-write is not complete consistency. There’s also read-after-update and read-after-delete.
#### Read-after-update Consistency
Read-after-update consistency would allow edits to an existing file, changes to an already-existing object, or updates to an existing table row to be immediately visible to all clients. That’s not related to read-after-write, which is only for new data. 
#### Read-after-delete Consistency
Read-after-delete would guarantee that reading a deleted object, file, or table row will fail for all clients.
### Availability
## Cache Eviction Policies
[Cache Replacement Policies - Wikipedia](https://en.wikipedia.org/wiki/Cache_replacement_policies)
### Least Recently Used (LRU)
Discards the least recently used items first. This algorithm requires keeping track of what was used when, which is expensive if one wants to make sure the algorithm always discards the least recently used item. General implementations of this technique require keeping "age bits" for cache-lines and track the "Least Recently Used" cache-line based on age-bits. This policy is the most popular due to its simplicity, good runtime performance, and a decent hit rate in common workloads.
### Random Replacement (RR)
Randomly selects a candidate item and discards it to make space when necessary. This algorithm does not require keeping any information about the access history. 
### Least Frequently Used (LFU)
Counts how often an item is needed. Those that are used least often are discarded first. This works very similar to LRU except that instead of storing the value of how recently a block was accessed, we store the value of how many times it was accessed.
## Cache Invalidation
### Write-through Cache
This is a caching system where writes go through the cache and write is confirmed as success to the client only if writes to DB and cache BOTH succeed. This is really useful for applications which write and re-read the information quickly. We will have complete data consistency between cache and storage. Also, this ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. However, writes to two separate systems will cause higher write latency.
### Write-around Cache
This is a caching system where writes go directly to the DB and confirmed as success to the client. This can reduce flooding the cache with write operations that will not subsequently be re-read. However, this has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from DB causing higher read latency.
### Write-back Cache
This is a caching system where writes go to cache alone and immediately confirmed as success to the client. The cache then asynchronously syncs this write to the DB. The write-back to permanent storage is done asynchonously or at specified intervals. This results in low latency and high write throughput for write-intensive applications. However, this speed comes with the risk of data loss during system disruptions. We can add replication to the cache and require replicas to confirm writes as well.
## Time to Live (TTL)
[TTL - Wikipedia](https://en.wikipedia.org/wiki/Time_to_live)
### Higher TTL
Higher TTL leads to longer caching and faster reads for clients. It also reduces traffic to DB. However, consistency worsens.
### Shorter TTL
Shorter TTL leads to improved consistency. However, this limits caching and add read latency to clients. 
## Distributed Cache
## Example: DNS
[In the World of DNS, Cache is King](http://blog.catchpoint.com/2014/07/15/world-dns-cache-king/)
## Example: HTTP Cache Headers
[Increasing Application Performance with HTTP Cache Headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)
# References
* [Design a Cache System - Gainlo](http://blog.gainlo.co/index.php/2016/05/17/design-a-cache-system/)
* [Design Cache - InterviewBit](https://www.interviewbit.com/problems/design-cache/)
* [Memcached - Wikipedia](https://en.wikipedia.org/wiki/Memcached)
