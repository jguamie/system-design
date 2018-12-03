# Transactions
A transaction is where several reads and writes are grouped together into a single logical unit so that they can be executed as one operation. The entire transaction will either succeed (commit) or fail (abort, rollback). Transactions allow for applications to not have to worry about partial failures and safely retry operations as databases provide these safety guarantees.

At times, it is better to weaken transaction guarantees to increase performance or availability. Almost all relational databases and some nonrelational databases support transactions. In a relational database, everything between a `BEGIN TRANSACTION` and `COMMIT` statement is part of the same transaction. Many nonrelational databases do not provide a way to group operations together.
## ACID
Transactions provide safety guarantees, also known as ACID: *Atomicity, Consistency, Isolation,* and *Durability*. 
Databases that are not ACID-compliant are called BASE: *Basically Available, Soft State,* and *Eventual Consistency*.
### Atomicity
Atomicity is the ability to abort a transaction on error and undo any writes that have been made so far. *Abortability* would have been better to describe the *A* in ACID.

This is not to be confused with atomic operation in multi-threaded programming. An atomic operation is where if one thread executes an atomic operation, another thread could not see half-finished results from that operation.

Atomicity can be implemented using a log such as B-trees for crash recovery.
### Consistency
Consistency is the application's perspective that the database is in a "good state." Certain statements about the data (invariants) must always be true. For example, in an accounting system, credits and debits across accounts must be balanced.

The *C* doesn't really belong in ACID. Consistency is a property of the application, whereas atomicity, isolation, and durability are properties of the database.

This is not to be confused with replica consistency and eventual consistency (replication), consistent hashing (partitioning), and consistency in the CAP theorem.
### Isolation
Isolation is where each transaction running concurrently with other transactions cannot interfere with each other. When transactions have been committed, the result should be identical to if they were run one after another (serially) even if they were run concurrently.

Isolation can be implemented using a lock on each object so only one thread can access an object at a time.
### Durability
Durability is the guarantee that once a transaction has been committed, the data will never be lost. It would safeguard against any hardware faults, database crashes, etc.
## Isolation Levels
Read committed isolation and snapshot isolation are weak (nonserializable) isolation levels. Serializable isolation guarantees that transactions have the same effect as if they were run serially (one at a time, without any concurrency). In practice, serializable isolation is not simple and incurs a performance cost. Many databases choose not to pay this cost. A popular comment is, "Use an ACID database if you're handling financial data!" Even many relational databases that are considered ACID use weak isolation.
### Read Committed Isolation
Read committed isolation is the most basic level of transaction isolation. It guarantees:
* When writing to the database, only data that has been committed will be overwritten (no dirty writes).
* When reading from the database, only data that has been committed will be returned (no dirty reads).
#### Dirty Writes
Dirty writes are where a transaction overwrites uncommitted data of an already-ongoing transaction. The database needs to delay the second transaction's write until the first transaction's write completes.

Most databases prevent dirty writes by using row-level locks. When a transaction wants to modify an object (row or document), it must first acquire a lock on that object. The lock is held until the transaction is committed or aborted. Other transactions that want to modify the object must wait until the lock is released.
#### Dirty Reads
Dirty reads are where one transaction reads another transaction's writes before they have been committed. As the data is in a partially-updated state, it will cause other transactions to make incorrect decisions. Also, if a transaction aborts, other transactions may have read data that will now be rolled back.

Most databases prevent dirty reads by remembering both the old committed value and the new value set by the transaction that currently holds the write lock. While a transaction is ongoing, other transactions that read the object are given the old value. Once a new value is committed, transactions switch over to reading the new value.

Read locks are rarely used as it harms the response time for read-only transactions. One long write transaction can force many read-only transactions to wait until the write transaction is completed.
#### Read Skew (Nonrepeatable Reads)
Read committed isolation doesn't prevent read skew (nonrepeatable reads). Read skew happens when a transaction is writing to multiple objects across the database. Then, a second transaction reads new data in one part of the database and old data in other parts. This causes the second transaction to see inconsistencies in the database. This is solved by snapshot isolation.
### Snapshot Isolation
Snapshot isolation is where a transaction takes a consistent snapshot of the database as its first operation and only reads data this snapshot. Even if the data is changed later by another transaction, each transaction will read old data from its snapshot.

Some other databases refer to snapshot isolation with a different name. In Oracle, this is implemented as serializable isolation. In PostgreSQL and MySQL, this is implemented as repeatable read isolation.
#### Multi-Version Concurrency Control (MVCC)
Snapshot isolation is usually implemented with multi-version concurrency control (MVCC). The database will keep several different committed versions of an object as various in-progress transactions need to read database state at different points in time. Each transaction is always given a unique, always-incrementing transaction ID. 

Whenever a write occurs, the data is tagged with the writer's transaction ID. Whenever a transaction reads from the database, any writes made by transactions with a later transaction ID are ignored. Whenever a transaction deletes data, the row isn't actually deleted. Instead, it is marked for deletion by setting the `deleted_by` field to the transaction ID.

A garbage collection process will periodically remove old object versions and delete objects marked for deletion when they are no longer visible to any transactions.
#### Lost Updates
Lost updates are when two transactions concurrently perform a read-modify-write cycle. Then, one transaction overwrites the other's write without incorporating its changes causing data loss. **TODO. TBD.**
#### Write Skew
Snapshot isolation doesn't prevent write skew. Write skew happens when a transaction reads an object, makes a decision based on the data, and writes its decision to the database. However, by the time the decision is committed, the initially-read object has changed causing the premise of the decision to be false. Only serializable isolation prevents write skew.
#### Phantom Reads
Phantom reads happen when a transaction reads objects that match a search condition. Then, another transaction commits a write that changes the results of that search. Snapshot serialization prevents standard phantom reads but 2PL's index-ranged locks are required to prevent phantoms and write skew.
### Serializable Isolation
Serializable isolation is the strongest isolation level. It guarantees that even if transactions are run in parallel (concurrently), the end result is the same as if they were run one at a time (serially). This isolation level prevents all possible race conditions.
#### Actual Serial Execution
The simplest way to implement serializable isolation is to remove concurrency entirely. That is, to execute each transaction one at a time, in serial order, on a single thread. Multi-threaded concurrency was considered essential for good performance but cheaper, large-sized RAM has made single-threaded execution possible. Today, it is feasible to keep an entire active dataset in memory. Transactions in memory execute much faster than from disk. Redis uses this approach. As throughput is limited to a single CPU core, transactions need to be structured differently to make the most of that single thread.

Partitioning data across multiple nodes can allow scaling to multiple CPU cores but require either:
1. Transactions to only read and write data from a single partition so that cross-parition coordination will not be necessary.
1. The database to coordinate transactions across all partitions that the transaction touches.

Key-value data can be partitioned easily. Data with multiple secondary indexes will require a lot of cross-partition coordination.
#### Two-Phase Locking (2PL)
In two-phase locking (2PL), several transactions are allowed to concurrently read the same object as long as no transaction is writing to it. If a transaction wants to read from an object, it must wait for any transaction writing to the object to commit or abort. Also, vice versa is true: if a transaction wants to write to an object, it must wait for all transactions reading from the object to commit or abort. MySQL (InnoDB) and SQL Server use 2PL.

2PL is implemented using shared locks and exclusive locks. The locks are implemented as follows:
* When a transaction wants to read an object, it must first acquire a shared lock. It must wait on any existing exclusive lock on the object. Other transactions reading from the same object can also acquire the shared lock simultaneously.
* When a transaction wants to write to an object, it must first acquire an exclusive lock. It must wait on any existing lock (shared or exclusive) on the object.
* When a transaction first reads and then writes to an object, it can upgrade its shared lock to an exclusive lock. It must wait on any transactions that have the shared lock on the same object.
* After a transaction acquires a lock, it must hold onto the lock until it commits or aborts.

Deadlocks easily occur when a transaction is stuck waiting for another transaction to release its lock, and vice versa. The database detects deadlocks between transactions and aborts one so that the other can make progress. The application will retry the aborted transaction.

To protect against phantoms and write skew, databases with 2PL use index-ranged locks (next-key locks). These work just like shared or exclusive locks but applied across a range of objects versus only one object.

2PL has significantly worse performance with transaction throughput and response times versus weak isolation. This is due to the overhead of acquiring/releasing locks and reduced concurrency.
# References
1. [Chapter 7, Transactions - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321) Chapters 1-3, 5-9 only
