# SQL and MySQL

## what is relational database?

A relational database organizes data into tables with predefined relationships, making it easy to access and manage related information. It uses rows to represent records and columns to represent attributes, with unique identifiers (keys) to link tables together logically. This structure is widely used, flexible, and efficient for storing, retrieving, and managing data, often using a language like SQL.

---
## What is ACID? How do we achieve it in SQL?
**ACID** stands for **Atomicity, Consistency, Isolation, Durability.** These properties gurantee *reliable transactions* in relational databases like MySQL, PostgreSQL, Orcle, etc.

>"ACID properties guarantee reliable transaction processing.
Atomicity ensures all-or-nothing execution,
Consistency ensures rule compliance,
Isolation ensures correct behavior under concurrency,
and Durability ensures committed data persists even after failures."

### A → Atomicity
***A transaction must completed fully or not at all.***  
>*Atomicity ensures that a transaction is treated as a single, indivisible unit of work, such that either all of its operations are successfully completed, or none of them are applied. If any part of the transaction fails, the entire transaction must be rolled back to preserve system correctness.*

using transactions we can achive this. Use **COMMIT** and **ROLLBACk**.

```SQL
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- commit only if both statment succeed. 
```
### C → Consistency

***Transaction must move database from one valid state to another valid state.***
>*Consistency ensures that a transaction takes the database from one valid state to another valid state by preserving all predefined rules, constraints, and data integrity conditions. A completed transaction must not violate any database invariants*

That means constraints must be satisfied and No invalid data allowed.

Using following we can achive this: 
* Primary key
* Foreign key
* Unique constraint
* Check constraint
* Data types

```SQL

ALTER TABLE orders
ADD CONSTRAINT check_amount CHECK (amount > 0);

```
If someone inserts amount = -50, the transaction fails → keeps DB consistent.

### I → Isolation

***Transactions should run as if they are the only one running.***  

>*Isolation ensures that concurrent execution of transactions results in a system state that would be obtained if the transactions were executed serially, one after the other, without interference. Intermediate states of a transaction must be invisible to other concurrent transactions.*

This Prevents dirty reads, phantom reads, dirty writes, lost updates.

This can be achived through following isolation levels:
* READ UNCOMMITTED
* READ COMMITTED
* REPEATABLE READ (MySQL default)
* SERIALIZABLE

```SQL

SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

```
### D → Durability

***Once a transaction is committed, the data is never lost, even on crash.***

>*Durability ensures that once a transaction has been committed, its effects become permanent and must persist even in the case of system failures, crashes, or power loss. The database guarantees the permanence of committed data through logging, recovery mechanisms, and stable storage.*

Through following we can achieves it:
* WAL (Write-Ahead Log) / Redo logs
* fsync
* Binary logs
* Crash recovery mechanisms

Example:
* In low level, MySQL InnoDB engine stores changes in redo logs → after crash, it replays logs automatically. 
* MySQL Replication in higher level

---
what is relationship? How many types of relationships present in RDBMS? 

A database relationship is a logical connection between two or more database tables, based on a shared data field, that links related information.


