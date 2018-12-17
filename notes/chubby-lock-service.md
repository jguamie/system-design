# The Chubby Lock Service
The Chubby Lock Service is a distributed course-grained locking service. Chubby also provides low-volume storage to be used as a repository for distributed systems' configuration changes. Chubby's most popular use has been as a name service.
## Overview
Chubby's goal is to allow clients to synchronize their activities and agree to basic information about their systems. Chubby uses the Paxos distributed consensus protocol to solve for asynchronous consensus. Each Chubby instance (Chubby cell) are dedicated to a single data center and typically serve about 10,000 machines.
## Design
Chubby wasn't designed to be a library that simply provides Paxos distributed consensus, but instead as a centralized lock service. This made it easier for developers to maintain existing program structure. Minimal effort is required to elect a master and write to an existing file server.

The storage feature is important as services need to advertise Chubby's results with others e.g., after a primary is elected or when data is repartitioned. Not having a separate service for sharing the results reduces the number of servers that clients depend on. Another benefit is that the consistency features of the protocol become shared.

Each Chubby cell typically has 5 replicas. This is to ensure high availability. A service advertising its primary through a Chubby file can have thousands of clients that need to observe this file.

Chubby uses event notification to notify clients of any changes to Chubby files. For example, clients and replicas of a service will need to be notified if the master/primary changes.
## Name Service
Chubby's success as a name service is due to its use of consistent client caching over time-based caching. Developers appreciate not having to manage a cache timeout such as DNS's time-to-live (TTL) value.

Caching within the standard internet naming system, DNS, is based on time. Each DNS entry has a TTL. A TTL that is too large will lead to long client fail-over times. A TTL that is too short will overload the DNS servers.

Chubby's caching uses explicit invalidations--a constant rate of session KeepAlive requests can maintain a large number of cache entries indefinitely.

A single Chubby name service cell is able to handle 90,000 clients communicating directly with it.
## Examples
* **Google File System**. Chubby is used to appoint a GFS master server. It is also used to store metadata. Chubby is the root of its data structures.
* **Bigtable**. Chubby is used to elect a master, allow the master to discover the servers it controls, and allow clients to find the master. It is also used to store metadata. Chubby is the root of its distributed data structures.
* **Access Control Lists (ACLs).** Chubby is used as a repository for ACL files.
# References
1. Burrows, Mike, "[The Chubby lock service for loosely-coupled distributed systems](https://ai.google/research/pubs/pub27897)," 7th USENIX Symposium on Operating Systems Design and Implementation (OSDI), {USENIX} (2006)
