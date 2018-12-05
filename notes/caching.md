# Caching
## Data Capacity
Estimate daily active users. Memory utilized per user. Queries per second (QPS).
Use the Pareto 80/20 Principle. 20% of daily active traffic will account for 80% of usage patterns.
## Cache Eviction Policies
[Cache Replacement Policies - Wikipedia](https://en.wikipedia.org/wiki/Cache_replacement_policies)
### Least Recently Used (LRU)
[Implement LRU Cache - LeetCode](https://leetcode.com/problems/lru-cache/description/)

Discards the least recently used items first. This algorithm requires keeping track of what was used when, which is expensive if one wants to make sure the algorithm always discards the least recently used item. General implementations of this technique require keeping "age bits" for cache-lines and track the "Least Recently Used" cache-line based on age-bits. This policy is the most popular due to its simplicity, good runtime performance, and a decent hit rate in common workloads.
### Random Replacement (RR)
Randomly selects a candidate item and discards it to make space when necessary. This algorithm does not require keeping any information about the access history. 
### Least Frequently Used (LFU)
Counts how often an item is needed. Those that are used least often are discarded first. This works very similar to LRU except that instead of storing the value of how recently a block was accessed, we store the value of how many times it was accessed.
### LFU with Dynamic Aging (LFUDA)
Same as LFU but adds an age factor to the reference count. In LFU, items frequently accessed in the past but unpopular today will remain in the cache for a long time. Dynamic Aging reduces the count of such items thereby making them eligible for replacement.
## Cache Invalidation
### Write-through Cache
This is a caching system where writes go through the cache and write is confirmed as success to the client only if writes to DB and cache BOTH succeed. This is really useful for applications which write and re-read the information quickly. We will have complete data consistency between cache and storage. Also, this ensures that nothing will get lost in case of a crash, power failure, or other system disruptions. However, writes to two separate systems will cause higher write latency.
### Write-around Cache
This is a caching system where writes go directly to the DB and confirmed as success to the client. This can reduce flooding the cache with write operations that will not subsequently be re-read. However, this has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from DB causing higher read latency.
### Write-back Cache
This is a caching system where writes go to cache alone and immediately confirmed as success to the client. The cache then asynchronously syncs this write to the DB. The write-back to permanent storage is done asynchronously or at specified intervals. This results in low latency and high write throughput for write-intensive applications. However, this speed comes with the risk of data loss during system disruptions. We can add replication to the cache and require replicas to confirm writes as well.
## Time to Live (TTL)
[TTL - Wikipedia](https://en.wikipedia.org/wiki/Time_to_live)
### Higher TTL
Higher TTL leads to longer caching and faster reads for clients. It also reduces traffic to DB. However, consistency worsens.
### Shorter TTL
Shorter TTL leads to improved consistency. However, this limits caching and add read latency to clients. 
## Example: DNS
[In the World of DNS, Cache is King](http://blog.catchpoint.com/2014/07/15/world-dns-cache-king/)
## Example: HTTP Cache Headers
[Increasing Application Performance with HTTP Cache Headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)
# References
1. [Caching - Grokking the System Design Interview](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5643440998055936)
1. [Design Cache - InterviewBit](https://www.interviewbit.com/problems/design-cache/)
1. [Memcached - Wikipedia](https://en.wikipedia.org/wiki/Memcached)
