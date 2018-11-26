# The Google File System
The Google File System (GFS) is a scalable, distributed file system.
## Overview
* GFS is designed for files that are 100 MB or larger. Multi-GB files are common. GFS is not optimized for small files.
* Writes are typically large, sequential writes that are appended to files. Once written, files are seldom modified again.
* Most client applications prioritize processing data in bulk at a high rate. Not many have low latency requirements for an individual read/write.
## Design
* Each GFS cluster has a single master and multiple chunk servers (3 replicas at default). 
### Master
* In terms of data, the master only maintains the file system metadata.
* The master sends HeartBeat messages to each chunkserver to provide instructions and collect state.
* As metadata is minimal compared to chunks, the master is able to keep all metadata in memory. This allows master operations to be fast.
### Chunkservers
* Chunkservers do not cache file data. Files are too large to be cached.
### Clients
* Clients initially interact with the master for metadata. After that, clients communicate directly with the chunkservers
* Clients only cache metadata. Clients stream huge files or have working sets too large to be cached.
### Chunk Size
* Files are divided into fixed-size 64 GB chunks. Each chunk is identified by a globally-unique 64 bit chunk handle.
* Large chunk size reduces client interactions with the master. Reads and writes on the same chunk only require one initial request to the master for chunk location info.
### Metadata
* The metadata consists of file and chunk namespaces, access control information, file-to-chunk mappings, and chunk locations (on all replicas).
* All metadata is available in master's memory.
* Namespaces and file-to-chunk mappings are persisted through log mutations on an operation log stored on master's local disk. The operation log is also replicated on remote machines. During master startup, it replays the operation log to recover its file system state.
#### Chunk Locations
* Chunk locations are not persistently stored on master's disk. Chunkservers provide chunk location information to the master. When a master starts up, it polls the chunkservers for this info. When a chunkserver joins a cluster, it provides this info to the master.
* The master stays up-to-date on chunk locations through periodic HeartBeat messages.
#### Operation Log
* When the operation log becomes large, the master will checkpoint its state.
