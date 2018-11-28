# MapReduce
MapReduce is a programming model and system for processing large data sets.
## Overview
* The model is easy to use as it hides the details of parallelization, fault-tolerance, data distribution, locality optimization, and load balancing. Programmers do not need experience with parallel or distributed systems to use MapReduce.
* A typical MapReduce computation processes many terabytes of data. Computations are often performed with 200,000 map tasks and 5,000 reduce tasks using 2,000 worker machines. Each task is about 16 MB to 64 MB of data.
* Machines are typically dual-processor x86 processors running Linux. Each machine has about 2 GB to 4 GB of memory.
## Programming Model
* **Map.** Takes an input and produces a set of intermediate key/value pairs.
* **Reduce.** Accepts intermediate pairs and combines shared keys together to produce an output.
## Execution Order
<img src="https://github.com/jguamie/system-design/blob/master/images/map-reduce-execution-order.png" align="middle" width="90%">
1. MapReduce library in the user program splits input files into *M* tasks. The program copies are started on a cluster of machines.
1. One of the program copies becomes the master. The rest become workers. The master assigns *M* map tasks and *R* reduce tasks to the workers.
1. Map workers read the contents split up from the input. It parses the input to generate input key/value pairs. Next, it passes each input pair into the user-defined Map function. This will output intermediate key/value pairs that are buffered into memory.
1. Periodically, a map worker writes buffered intermediate pairs to local disk across *R* partitions. The map worker passes the pair disk locations to the master. The master will forward these locations to the reduce workers.
1. Once a reduce worker is notified by the master of the buffered intermediate pair locations, the reduce worker will use an RPC to read the data from the map workers' local disk. Once all intermediate data has been read, it sorts the data by intermediate keys. This will group all identical keys together.
1. The reduce worker passes each unique intermediate key and the corresponding set of values to the user-defined Reduce function. The output is appended to a final output file for the given reduce partition.
1. When all map and reduce tasks have completed, the master returns the result to the user program. The output is available in 'R' output files, one per reduce task. Each of the file names were specified by the user prior to running the program.
