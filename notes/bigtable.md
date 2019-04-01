# Bigtable
Bigtable is a distributed storage system for managing structured data.
## Data Model
Bigtable is indexed by row key, column key, and timestamp. Row keys and column keys are arbitrary strings. Timestamps are 64-bit integers in microseconds. 
`(row:string, column:string, time:int64) -> string`
### Row Keys
Row keys are typically 10-100 bytes. The max size is 64 kilobytes. Bigtable stores data by the row key in lexicographic order. Each table is dynamically partitioned by a range of rows. These partitions of row ranges are called tablets. In Bigtable, this partitioning approach allows for efficient reads to row ranges.
### Column Keys
Column keys are grouped into sets of column families. As column keys within a column family have minimal variability, it allows for optimal data compression within a column family.
### Timestamps
Bigtable allows for multiple versions of the same data. These versions are indexed by timestamp. Different versions are stored in decreasing order so the most recent version is read first. Bigtable can be configured to garbage collect older versions automatically. With garbage collection turned on, only the most recent three versions are stored.
## Design
Bigtable has three major components: a client library, one master server, and many tablet servers.
### Master
The Bigtable master is primarily responsible for assigning tablets to tablet servers and balancing tablet server load. It manages garbage collection of GFS files. It handles any schema changes e.g, table and column family updates.
### Tablet Servers
Each tablet server will manage between 10 to 1,000 tablets. The tablet server handles all read and write requests to these tablets. The tablet server will also split tablets when it has become too large. Each tablet is about 100 to 200 MB in size.
### Clients
Clients communicate directly with tablet servers through the client library for all their read and write requests. Clients do not rely on the Bigtable master for tablet location information. As such, the Bigtable master is always lightly loaded.
### Tablet Location
Bigtable uses a three-level hierarchy to store tablet location information. This hierarchy can store up to 2<sup>34</sup>tablets.
<img src="https://github.com/jguamie/system-design/blob/master/images/bigtable-tablet-location.png" align="middle" width="60%">

1. The first level is the Root tablet. The Root tablet location is stored in a Chubby file. The Root tablet contains mappings of all the User tablets to their respective METADATA tablet. The Root tablet is never split to maintain the three level hierarchy.
2. The second level consists of the METADATA tablets. Each METADATA tablet contains the location of a set of User tablets. Each User tablet location is stored under a row key and contains the tablet's table identifier and its end row. Each METADATA row stores about 1 KB in memory. Each METADATA tablet has a max size of 128 MB.
3. The third level consists of all the User tablets. To locate tablets, clients require three network round-trips including one read from Chubby. The client library caches tablet locations.
## Dependencies
BigTable uses the Sorted String Table (SSTable) file format for its indexes. Refer to Chapter 3: Storage and Retrieval in Designing Data-Intensive Applications for more information on SSTables.

Bigtable uses the [Google File System](https://github.com/jguamie/system-design/blob/master/notes/google-file-system.md) to store log and data files.

Bigtable uses the [Chubby Lock Service](https://github.com/jguamie/system-design/blob/master/notes/chubby-lock-service.md) to ensure a single active master, discover tablet servers, store Bigtable schema information, and store access control lists.

Bigtable depends on Borg, Google's cluster management system, for scheduling jobs, managing resources on shared machines, dealing with machine failures, and monitoring machine health.
## Transactions
Bigtable will only support single-row [transactions](https://github.com/jguamie/system-design/blob/master/notes/transactions.md). Bigtable does not support general transactions across a range of row keys.
### Tablets

## Applications
### MapReduce
Bigtable can be used as an input source and/or output target for [MapReduce](https://github.com/jguamie/system-design/blob/master/notes/map-reduce.md) jobs.

# Reference
1. Chang, Fay, Dean, Jeffrey, Ghemawat, Sanjay, Hsieh, Wilson, Wallach, Deborah, Burrows, Mike, Chandra, Tushar, Fikes, Andrew, and Gruber, Robert, "[Bigtable: A Distributed Storage System for Structured Data](https://ai.google/research/pubs/pub27898)," 7th USENIX Symposium on Operating Systems Design and Implementation (OSDI), {USENIX} (2006), pp. 205-218
1. Kleppmann, Martin. “Chapter 3: Storage and Retrieval.” [*Designing Data-Intensive Applications: The Big Ideas behind Reliable, Scalable, and Maintainable Systems*](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321). Sebastopol, CA: O'Reilly Media, 2017.
