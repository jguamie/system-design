# Transactions
A transaction is where several reads and writes are grouped together into a single logical unit so that they can be executed as one operation. The entire transaction will either succeed (commit) or fail (abort, rollback). Transactions allow for applications to not have to worry about partial failures and safely retry operations as databases provide these safety guarantees.

At times, it is better to weaken/abandon transaction guarantees to increase performance or availability. Almost all relational databases and some nonrelational databases support transactions. In a relational database, everything between a `BEGIN TRANSACTION` and `COMMIT` statement is part of the same transaction. Many nonrelational databases do not provide a way to group operations together.
## ACID
Transactions provide safety guarantees, also known as ACID: *Atomicity, Consistency, Isolation,* and *Durability*. 
Databases that are not ACID-compliant are called BASE: *Basically Available, Soft State,* and *Eventual Consistency*.
### Atomicity
Atomicity is the ability to abort a transaction on error and undo any writes that have been made so far. *Abortability* would have been better to describe the *A* in ACID.

This is not to be confused with atomic operation in multi-theaded programming. An atomic operation is where if one thread executes an atomic operation, another thread could not see half-finished results from that operation.

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
## Weak Isolation Levels
Weak isolation levels are nonserializable. In practice, serializable isolation is not simple and incurs a performance cost. Many databases choose not to pay this cost. 
A popular comment is, "Use an ACID database if you're handling financial data!" Even many relational databases that are considered ACID use weak isolation.
### Read Committed Isolation
Read committed is the most basic level of transaction isolation. It guarantees:
* When writing to the database, only data that has been committed will be overwritten (no dirty writes).
* When reading from the database, only data that has been committed will be returned (no dirty reads).
#### Dirty Writes
Dirty writes can cause a transaction to overwrite uncommitted data of an already-ongoing transaction. The database needs to delay the second transaction's write until the first transaction's write completes.

Most databases prevent dirty writes by using row-level locks. When a transaction wants to modify an object (row or document), it must first acquire a lock on that object. The lock is held until the transaction is committed or aborted. Other transactions that want to modify the object must wait until the lock is released.
#### Dirty Reads
Dirty reads cause users to see data in a partially-updated state. This can cause other transactions to make incorrect decisions. Also, if a transaction aborts, other transactions may read data that is later rolled back.

Most databases prevent dirty reads by remembering both the old committed value and the new value set by the transaction that currently holds the write lock. While a transaction is ongoing, other transactions that read the object are given the old value. Once a new value is committed, transactions switch over to reading the new value.

Read locks are rarely used as it harms the response time for read-only transactions. One long write transaction can force many read-only transactions to wait until the write transaction is completed.
### Snapshot Isolation
A concern with read committed isolation is that it doesn't prevent read skew (nonrepeatable reads). Read skew happens when a transaction is writing to multiple objects across the database. A second transaction will read new data in one part of the database and old data in other parts.

This is solved by the next level of transaction isolation. Snapshot isolation is where a transaction takes a consistent snapshot of the database as its first operation and only reads data this snapshot. Even if the data is changed later by another transaction, each transaction will read old data from its snapshot.

Some other databases refer to snapshot isolation with a different name. In Oracle, this is implemented as serializable isolation. In PostgreSQL and MySQL, this is implemented as repeatable read isolation.
# References
1. [Chapter 7, Transactions - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321) Chapters 1-3, 5-9 only
