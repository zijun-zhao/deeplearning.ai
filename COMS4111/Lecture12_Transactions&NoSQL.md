

### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

### Table of Contents

1. [Lecture1&2-24Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture1&2_Intro&Overview.md)
2. [Lecture3-31Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture3.md)
3. [Lecture4-7Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture4_ERModel_SQL.md)
4. [Lecture5-14Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture5_ERModel_SQL.md)
5. [Lecture6-21Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture6_RelationalAlgebra.md)
6. [Lecture7-28Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture7_Wrap_up.md)
7. [Lecture8-6Mar](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture8_EndModule_I.md)
8. [Lecture9-13Mar](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture9_Disks&IO&Index.md)
9. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)
10. [Lecture11-10Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture11_QueryProcessing&Transactions&Recovery.md)
11. [Lecture12-17Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture12_Transactions.md)

* Course Modules 
	* Transaction Concept
	* Transaction State
	* Concurrent Executions
	* Serializability
	* Recoverability
	* Implementation of Isolation
	* * Transaction Definition in SQL
	* Testing for Serializability.

  
## 17 Apr 2020


1. Transaction Concept
	* A transaction is a **unit of program execution** that accesses and  possibly updates various data items.
	* E.g., transaction to transfer $50 from account A to account B:
	
		![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_1.jpg)
* Two main issues to deal with:
	* **Failures** of various kinds, such as hardware failures and system crashes
	* **Concurrent** execution of multiple transactions
* **Atomicity requirement**: If the transaction fails after step 3 and before step 6, money will be “lost” leading to an inconsistent database state
	* Failure could be due to software or hardware
	* The system should ensure that updates of a partially executed transaction are not reflected in the database
* **Durability requirement**: once the user has been notified that the transaction has completed (i.e., the transfer of the $50 has taken place), the updates to the database by the transaction must persist even if there are software or hardware failures.
* **Consistency requirement in above example**: The sum of A and B is unchanged by the execution of the transaction
* **Isolation requirement**: if between steps 3 and 6, another transaction T2 is allowed to access the partially updated database, it will see an inconsistent database (the sum  A + B will be less than it should be).


2. In general, consistency requirements include 
	* **Explicitly** specified integrity constraints such as primary keys and foreign keys
	* **Implicit** integrity constraints
		* e.g., sum of balances of all accounts, minus sum of loan amounts must equal value of cash-in-hand
	* A transaction must see a **consistent database**.
	* During transaction execution the database may be **temporarily** inconsistent.
	* When the transaction completes successfully the database must be consistent
		* Erroneous transaction logic can lead to inconsistency

3. An example of fund transfer
```sql
start transaction;

SELECT balance into @s_balance FROM w4111final.banking_account where id=3;
set @s_balance = @s_balance-50;
update banking_account set balance=@s_balance where id=3;

SELECT balance into @t_balance FROM w4111final.banking_account where id=4;
set @s_balance = @t_balance+50;
update banking_account set balance=@t_balance where id=4;

rollback;
```
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_2.jpg)
* In this case, when we write(A) and we change the value. We have exclusively lock A. Therefore transaction T2 will start but it will be blocked. T2 will be block when it performs **read(A).

![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_3.jpg)
* In this case, read(B) of T2 will work. read(B) in T1 will also work.
	* The operation are interweaved. 

* The edges are the flow of data. The data flowing is a relation.
    * Base table.
    * Derived table.

* Isolation can be ensured trivially by running transactions serially
	* That is, one after the other.   
* However, executing multiple transactions concurrently has significant benefits, as we will see later. 



4. Transaction are in a state, many of thoses states are internel which we cannot see.

![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_4.jpg)
> partially committed and failed are internal states.

* **Active**: the initial state; the transaction stays in this state while it is executing
* **Partially committed**: after the final statement has been executed.
* **Failed**: after the discovery that normal execution can no longer proceed.
	* Not necessary need to do the abort.
* **Aborted**: after the transaction has been rolled back and the database restored to its state prior to the start of the transaction. Two options after it has been aborted:
	* Restart the transaction
		* Can be done only if no internal logical error
	* Kill the transaction
* Committed – after successful completion.

5. Concurrent Executions
	* Multiple transactions are allowed to run **concurrently** in the system.  Advantages are:
		* Increased processor and disk utilization, leading to better transaction throughput
			* E.g., one transaction can be using the CPU while another is reading from or writing to the disk
		* **Reduced average response time for transactions**: short transactions need not wait behind long ones.
		
* **Concurrency control schemes** – mechanisms  to achieve isolation
	* That is, to control the interaction among the concurrent transactions in order to prevent them from destroying the consistency of the database
		* Will study in Chapter 15, after studying notion of correctness of concurrent executions.


6. Schedule is the order in which things happen. The overall schedule

Serial Schedule|Serial Schedule|Non serial Schedule
---|---|---
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_5.jpg)|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_6.jpg)|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_7.jpg)

* A serial schedule in which T1 is followed by T2. 
* A interleaved schedule, the outcome is the same as a serial schedule. It may be faster.

7.Serializability
	* A (possibly concurrent) schedule is serializable if it is **equivalent to a serial schedule**. Different forms of schedule equivalence give rise to the notions of:
		* Conflict serializability
		* View serializability

8. Conflicting Instructions 
* Two instructions conflict when they access the same piece of data and one of them do **write**

9. Conflict Serializability
> A schedule is not necessaryly serialized, but we swap non conflicting operations.
	* If a schedule S can be transformed into a schedule S’ by a series of swaps of non-conflicting instructions, we say that S and S’ are conflict equivalent.
	* We say that a schedule S is conflict serializable if it is conflict equivalent to a serial schedule


Schedule 3|Schedule 6
---|---
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_8.jpg)|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_9.jpg)

* Schedule 3 can be transformed into Schedule 6, a serial schedule where T2 follows T1, by series of swaps of non-conflicting instructions. 
	* Therefore Schedule 3 is conflict serializable.

T3|T4
---|---
read(Q)|-
-|write(Q)
write(Q)|-
* In this example, We are unable to swap instructions in the to obtain either the serial schedule < T3, T4 >, or the serial schedule < T4, T3 >.


10. Precedence graph: a direct graph where the vertices are the transactions (names).

> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_10.jpg)

* A schedule is conflict serializable if there is no circle in the precedence graph. 
* A schedule is conflict serializable if and only if its precedence graph is acyclic.
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_11.jpg)
* However, the problem of checking if a schedule is view serializable falls in the class of NP-complete problems. 


11. Cascading Rollbacks
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_12.jpg)

* Cascading rollback: a single transaction failure leads to a series of transaction rollbacks.  Consider the following schedule where none of the transactions has yet committed (so the schedule is recoverable)
	* In the example above, the operation in T<sub>11</sub> read(A), but the write(A) in T<sub>10</sub> never happens due to abort, therefore read(A) reads a value that never happens. The abort in T<sub>10</sub> will cause the operation in T<sub>11</sub> and T<sub>12</sub> also abort, which will be a cascading abort.
	
	
12. Weak Levels of Consistency
	* Some applications are willing to live with weak levels of consistency, allowing schedules that are not serializable
		* E.g., a **read-only** transaction that wants to get an approximate total balance of all accounts 
		* E.g., database statistics computed for query optimization can be approximate (why?)
		* Such transactions need not be serializable with respect to other transactions
	* Tradeoff accuracy for performance
> According to Prof. Ferguson, When we start a transaction, we will specify an isolation level. Everything up upon now is searializable. There are also levels that are less strict, which will allow non-serializable behaviors occur because it does not matter. Serializable is kind of restrictive.

13. Several levels of consistency in SQL-92
	* Serializable: default
	* Repeatable read: only committed records to be read. 
		* Repeated reads of same record must return same value.
			* When doing join, go through the inner loop once for every row and the outer loop. For the right table, we may read each record thousands of time.
		* However, a transaction may not be serializable – it may find some records inserted by a transaction but not find others.
			* When looping through a table, we will never see a different value for a record that we read, record that we read will never disappear. Things may get insert
	* Read committed: only committed records can be read.
		* Successive reads of record may return different (but committed) values.
	* Read uncommitted: even uncommitted records may be read. 
> When transfering monet, serializable is important. But if we are just trying some statistics, we do not care whether the data has been changed.
	
```SQL	
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

start transaction;

SELECT balance into @s_balance FROM w4111final.banking_account where id=3;
set @s_balance = @s_balance-50;
update banking_account set balance=@s_balance where id=3;

SELECT balance into @t_balance FROM w4111final.banking_account where id=4;
set @s_balance = @t_balance+50;
update banking_account set balance=@t_balance where id=4;

rollback;
```
	
	
	
14. Concurrency Control 
* Principle way that the database system guarantees isolation is by **lock**, Data items can be locked in two modes :
	* **exclusive** (X) mode. Data item can be both read as well as  written. X-lock is requested using  lock-X instruction.
	* **shared**(S) mode. Data item can only be read. S-lock is requested using  lock-S instruction.
* We can have multiple shared lock at the same time, but we can only have one exclusive lock. 

15. Lock-compatibility matrix

-|s|X
---|---|---
S|true|false
T|false|false

![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_13.jpg)
	* grant table shows the lock that has been granted: T1 will X-lock on A. In sql we cannot specify the lock we want. When doing a select, it will do a share lock. Unlock will release the grant. In theory, when we request a lock(like lock X(A) in T1), the mangager will look at the grant table to see if it conflits others. lock X(A) in T1 will suspend since it conflicts with grant-S(A,T2).
* Locking protocols enforce serializability by restricting the set of possible schedules.

16. Example of Deadlock
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_13.jpg)
* In the above example, neither T3 nor T4 can make progress: executing  lock-S(B) causes T4 to wait for T3 to release its lock on B, while executing  lock-X(A) causes T3  to wait for T4 to release its lock on A.
* Such a situation is called a deadlock. 
	* To handle a deadlock one of T3 or T4 must be rolled back and its locks released.

> According to Prof. Ferguson, when doing locking, we need to detect and break deadlock, otherwise the system will freaze

17. The Two-Phase Locking Protocol 
* A protocol which ensures conflict-serializable schedules.
* Locking by itself does not assure serializability. However, two-phase locking, in which all transactions first enter a phase where they only **acquire locks**, and then enter a phase where they only **release locks**, will guarantee serializability.
* Phase 1: Growing Phase
	* Transaction may obtain locks 
	* Transaction may not release locks
* Phase 2: Shrinking Phase
	* Transaction may release locks
	* Transaction may not obtain locks
* The protocol assures **serializability**. It can be proved that the transactions can be serialized *in the order of their lock points*  (i.e., the point where a transaction acquired its final lock). 


* Extensions to basic two-phase locking needed to ensure recoverability of freedom from cascading roll-back
	* **Strict two-phase locking**: a transaction must hold all its exclusive locks till it commits/aborts.
		* Ensures recoverability and avoids cascading roll-backs
	* **Rigorous two-phase locking**: a transaction must hold all locks till commit/abort. 
		* Transactions can be serialized in the order in which they commit.
* Most databases implement *rigorous two-phase locking*, but refer to it as simply two-phase locking



> Two-phase locking is sufficient, but not necessary. However, there are schedules that are serializable but are precluded by strict two-phase locking. There are conflict serializable schedules that cannot be obtained if the two-phase locking protocol is used, despite of the fact that it would produce the expected answer.  

18. An example of how database lock: When execute the followin code in workbench without commit, we will not able to run the same code 
in another tab

	```sql
	SET SESSION TRANSACTION ISOLATION LEVEL serializable;

	select * from banking_account;
	update banking_accounts set balance=151 where id=1;
	select * from banking_account;
	```	

* Similarly, for the code below, only when the first execution is aborted will we finish running the second execution

	```sql
	SET SESSION TRANSACTION ISOLATION LEVEL repeatable read;

	select * from banking_account;
	update banking_accounts set balance=157 where id=1;
	select * from banking_account;
	```	
	
* Similarly, for the code below, only when the first execution is aborted will we finish running the second execution
	```sql
	SET SESSION TRANSACTION ISOLATION LEVEL read uncommitted;

	select * from banking_account;
	update banking_accounts set balance=157 where id=1;
	select * from banking_account;
	```		
> Two things we care: atomicity and isolation. Isolation is done by locking, there are various level for isolation.
	


19. NoSQL Databases
> Database did not support sql, or completely not relational. Original, all databases are noSQL. Then relational database come along, everything become relational database. People begin to realize relational has limitations later.

> Now NoSQL tends to mean "not only SQL", it will suppoty a sql-like query, and it will not be strictly relational, it will relax integrity and consistency constraint.
	* However, by forcing integrity and consistency, relational database limits the scalibility and availability.

* The primary difference between NoSQL and SQL database: Many NoSQL stores compromise consistency. 


> Instead, most NoSQL databases offer a concept of "**eventual consistency**" in which database changes are propagated to all nodes "eventually" (typically within milliseconds) so queries for data might not return updated data immediately or might result in reading data that is not accurate, a problem known as stale reads


20. Types of NoSQL database:
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_15.jpg)
* What is the underline base data model?
	* relational database: Tables, tabula data 
	* document database: json is hierarchical->mongoDB
	* graph databases: data model is graph.
	* wide column stores
	* key-value database
	
21. CAP Theorem: foundamental theory
	* Consistency: Every read receives the most recent write or an error.
	* Availability: Every request receives a (non-error) response, without guarantee that it contains the most recent write.
	* Partition Tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
		* The system continues to operate even though there are failures.
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_16.jpg)

* There are no database that can satisfy these three, only two of them can be satisfied.

22. Consistency Models
* **STRONG CONSISTENCY**: Strong consistency is a consistency model where all subsequent accesses to a distributed system will always return the updated value after the update.
* **WEAK CONSISTENCY**: It is a consistency model used in distributed computing where subsequent accesses might not always be returning the updated value. There might be inconsistent responses.
	* processer cache,
* **EVENTUAL CONSISTENCY**: Eventual consistency is a special type of weak consistency method which informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value.
	* if you keep reading, you will eventually see the recent value




