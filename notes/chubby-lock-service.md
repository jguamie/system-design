# The Chubby Lock Service
The Chubby Lock Service is a course-grained locking service and reliable. As a secondary, Chubby provides low-volume storage for distributed systems. Chubby's most popular use has been as a name server.
## Overview
Chubby's goal is to allow clients to synchronize their activities and agree to basic information about their systems. Chubby uses the Paxos distributed consensus protocol to solve for asynchronous consensus. Each Chubby instance (Chubby cell) are dedicated to a single data center and typically serve about 10,000 machines.
## Design
## Name Service
Chubby's success as a name server is due to its use of consistent client caching over time-based caching.
## Examples
* **Google File System**. Chubby is used to appoint a GFS master server. It is also used to store metadata. Chubby is the root of its data structures.
* **Bigtable**. Chubby is used to elect a master, allow the master to discover the servers it controls, and allow clients to find the master. It is also used to store metadata. Chubby is the root of its distributed data structures.
# References
1. Burrows, Mike, "[The Chubby lock service for loosely-coupled distributed systems](https://ai.google/research/pubs/pub27897)," 7th USENIX Symposium on Operating Systems Design and Implementation (OSDI), {USENIX} (2006)
