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
BigTable uses the Sorted String Table (SSTable) file format for its indexes. Refer to Chapter 3: Storage and Retrieval in Designing Data-Intensive Applications for more information on SSTables.

Bigtable uses the [Google File System](https://github.com/jguamie/system-design/blob/master/notes/google-file-system.md) to store log and data files.

Bigtable uses the [Chubby Lock Service](https://github.com/jguamie/system-design/blob/master/notes/chubby-lock-service.md) to ensure a single active master, discover tablet servers, store Bigtable schema information, and store access control lists.

Bigtable depends on Borg, Google's cluster management system, for scheduling jobs, managing resources on shared machines, dealing with machine failures, and monitoring machine health.
### Transactions
Bigtable will only support single-row [transactions](https://github.com/jguamie/system-design/blob/master/notes/transactions.md). Bigtable does not support general transactions across a range of row keys.
## Applications
### MapReduce
Bigtable can be used as an input source and/or output target for [MapReduce](https://github.com/jguamie/system-design/blob/master/notes/map-reduce.md) jobs.

# Reference
1. Chang, Fay, Dean, Jeffrey, Ghemawat, Sanjay, Hsieh, Wilson, Wallach, Deborah, Burrows, Mike, Chandra, Tushar, Fikes, Andrew, and Gruber, Robert, "[Bigtable: A Distributed Storage System for Structured Data](https://ai.google/research/pubs/pub27898)," 7th USENIX Symposium on Operating Systems Design and Implementation (OSDI), {USENIX} (2006), pp. 205-218
1. Kleppmann, Martin. “Chapter 3: Storage and Retrieval.” [*Designing Data-Intensive Applications: The Big Ideas behind Reliable, Scalable, and Maintainable Systems*](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321). Sebastopol, CA: O'Reilly Media, 2017.
