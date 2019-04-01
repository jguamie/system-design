# Bigtable
Bigtable is a distributed storage system for managing structured data.
## Data Model
Bigtable is indexed by row key, column key, and timestamp. 
`(row:string, column:string, time:int64) -> string`
### Row Keys
Bigtable stores data in lexicographic order by the row key. Each table is dynamically partitioned by a range of rows. These partitions of row ranges are called tablets. In Bigtable, this partitioning approach allows for efficient reads to row ranges.
### Column Keys
Column keys are grouped into sets of column families. As column keys within a column family have minimal variability, it allows for optimal data compression within a column family.
### Timestamps
Bigtable allows for multiple versions of the same data. These versions are indexed by timestamp (64-bit integer in microseconds). Different versions are stored in decreasing order so the most recent version is read first. Bigtable can be configured to garbage collect older versions automatically.
## This is still Work in Progress

# Reference
1. Chang, Fay, Dean, Jeffrey, Ghemawat, Sanjay, Hsieh, Wilson, Wallach, Deborah, Burrows, Mike, Chandra, Tushar, Fikes, Andrew, and Gruber, Robert, "[Bigtable: A Distributed Storage System for Structured Data](https://ai.google/research/pubs/pub27898)," 7th USENIX Symposium on Operating Systems Design and Implementation (OSDI), {USENIX} (2006), pp. 205-218
