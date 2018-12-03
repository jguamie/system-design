# Transactions
A transaction is where several reads and writes are grouped together into a single logical unit so that they can be executed as one operation. The entire transaction will either succeed (commit) or fail (abort, rollback). Transactions allow for applications to not have to worry about partial failures and safely retry operations as databases provide these safety guarantees.

At times, it is better to weaken/abandon transaction guarantees to increase performance or availability. Almost all relational databases and some nonrelational databases support transactions.
## ACID
Transactions provide safety guarantees, also known as ACID: *Atomicity, Consistency, Isolation,* and *Durability*. 
Databases that are not ACID-compliant are called BASE: *Basically Available, Soft State,* and *Eventual Consistency*.
### Atomicity
Atomicity is the ability to abort a transaction on error and undo any writes that have been made so far. *Abortability* would have been better to describe the *A* in ACID.

This is not to be confused with atomic operation in multi-theaded programming. An atomic operation is where if one thread executes an atomic operation, another thread could not see half-finished results from that operation.
### Consistency
Consistency is the application's perspective that the database is in a "good state." Certain statements about the data (invariants) must always be true. For example, in an accounting system, credits and debits across accounts must be balanced.

The *C* doesn't really belong in ACID. Consistency is a property of the application, whereas atomicity, isolation, and durability are properties of the database.

This is not to be confused with replica consistency and eventual consistency (replication), consistent hashing (partitioning), and consistency in the CAP theorem.
### Isolation
Isolation is where each transaction running concurrently with other transactions cannot interfere with each other. When transactions have been committed, the result should be identical to if they were run one after another (serially) even if they were run concurrently.
### Durability
Durability is the guarantee that once a transaction has been committed, the data will never be lost. It would safeguard against any hardware faults, database crashes, etc.
# References
1. [Chapter 7, Transactions - Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321) Chapters 1-3, 5-9 only
