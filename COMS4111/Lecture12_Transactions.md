

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
* In this case, when we write(A) and we change the value. We have exclusively lock A. Therefore transaction T2 will start but it will be blocked. T2 will be block when it performs **read(A)**.

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

	-|S|X
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

* System is deadlocked if there is a set of transactions such that every transaction in the set is **waiting for another transaction in the set**.![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_19.jpg)
	* Locking is normally explicit. When doing select, update, etc the lock is done automatically. In this example, T4 tries to share lock on B, but it cannot since T3 has already lock on B. T4 will stop and wait for that lock, while T3 also cannot lock A. 
* To handle deadlock
	* Require that each transaction locks all its data items before it begins execution (pre-declaration).
	* Impose partial ordering of all data items and require that a transaction can lock data items only in the order specified by the partial order (graph-based protocol).
		* cannot lock something in front of you
* Deadlock prevention strategies
* **wait-die** scheme — non-preemptive
	* Older transaction may wait for younger one to release data item.
	* Younger transactions never wait for older ones; they are rolled back instead.
	* A transaction may die several times before acquiring a lock
* **wound-wait** scheme — preemptive
	* Older transaction wounds (forces rollback) of younger transaction instead of waiting for it. 
	* Younger transactions may wait for older ones.
	* Fewer rollbacks than wait-die scheme.
* In both schemes, a rolled back transactions is restarted with its original timestamp. 
	* Ensures that older transactions have precedence over newer ones, and starvation is thus avoided.
	> The above are more theoretical. Timeout is more common.
	* Timeout-Based Schemes:
		* A transaction waits for a lock only for a specified amount of time. After that, the wait times out and the transaction is rolled back.
		* Ensures that deadlocks get resolved by timeout if they occur
		* Simple to implement
		* But may roll back transaction unnecessarily in absence of deadlock
			* Difficult to determine good value of the timeout interval.
		* Starvation is also possible
* Deadlock Detection using wait-for graph
	> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_20.jpg)
* When deadlock is  detected :
	* Some transaction will have to rolled back (made a victim) to break deadlock cycle.  
		* Select that transaction as victim that will incur minimum cost
* Rollback -- determine how far to roll back transaction
	* Total rollback: Abort the transaction and then restart it.
	> Total rollback is more common. 
	* Partial rollback: Roll back victim transaction only as far as necessary to release locks that another transaction in cycle is waiting for
* Starvation can happen (why?): 
	* One solution: **oldest transaction** in the deadlock set is never chosen as victim
* Another way of locking: Multiple Granularity
	> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_21.jpg)
* Intention Lock Modes
	* In addition to S and X lock modes, there are three additional lock modes with multiple granularity:
		* intention-shared (IS): indicates explicit locking at a lower level of the tree but only with shared locks.
		* intention-exclusive (IX): indicates explicit locking at a lower level with exclusive or shared locks
		* shared and intention-exclusive (SIX): the subtree rooted by that node is locked explicitly in shared mode and explicit locking is being done at a lower level with exclusive-mode locks.
	* Intention locks allow a higher level node to be locked in S or X mode without having to check all descendent nodes.
	> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_22.jpg)

17. Locking rules for insert/delete operations
	* An exclusive lock must be obtained on an item **before it is deleted**
	* A transaction that **inserts** a new tuple into the database I **automatically** given an X-mode **lock** on the tuple
* Ensures that 
	* reads/writes conflict with deletes
	* Inserted tuple is not accessible by other transactions until the transaction that inserts the tuple commits


18. Phantom Phenomenon
* Example of phantom phenomenon.
* A transaction T1 that performs predicate read  (or scan) of a relation 
	```sql
	select count(*)
	from instructor
	where dept_name = 'Physics'
	```
* and a transaction T2 that inserts a tuple while T1 is active but **after predicate read**
```sql
insert into instructor values ('11111', 'Feynman', 'Physics', 94000)
```
* (conceptually) conflict in spite of not accessing any tuple in common.
* If only tuple locks are used, non-serializable schedules can result
	* E.g. the scan transaction does not see the new instructor, but may read some other tuple written by the update transaction
* Can also occur with updates
	* E.g. update Wu’s department from Finance to Physics
> Here, When doing the select statement, in transaction T1 I read the predicate, and after insert, if we run T1 again, the count will be different. We will relock that we have touched, but we cannot lock the new-insert one. 


19. How to handle phantoms
* Index locking protocol
> This means we cannot do an insert since we are locking the index notes, then remember the B+ tree, we can also not update the index.

20. The Two-Phase Locking Protocol 
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

21. An example of how database lock: When execute the followin code in workbench without commit, we will not able to run the same code 
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
	
22. Timestamp Based Concurrency Control-can prevent phantoms
	* Each transaction Ti  is issued a **timestamp TS(Ti)** when it enters the system.
		* Each transaction has a **unique** timestamp
		* Newer transactions have timestamps strictly greater than earlier ones
		* Timestamp could be based on a logical counter
			* Real time may not be unique
			* Can use (wall-clock time, logical counter) to ensure 
	* Timestamp-based protocols manage concurrent execution such that **time-stamp order = serializability order**
	* Several alternative protocols based on timestamps

23. Validation-Based Protocol
> Check whether any changes after the read. 

24. Multiversion concurrency control
* Multiversion schemes keep old versions of data item to increase concurrency.  
	* Cases exist that some database system support increment lock. The idea behind is that increment lock does not flip. At the end, if three transaction increment


25. Weak Levels of Consistency
* Cursor stability: 
	* For reads, each tuple is locked, read, and lock is immediately released
	* X-locks are held till end of transaction
	* Special case of degree-two consistency
	> does not guarantee serializability26. 
26. SQL allows non-serializable executions
	* Serializable: is the default
	* Repeatable read: allows only committed records to be read, and repeating a read should return the same value (so read locks should be  retained)
	* However, the phantom phenomenon need not be prevented
		* T1 may see some records inserted by T2, but may not see others inserted by T2
	* Read committed:  same as degree two consistency, but most systems implement it as cursor-stability
	* Read uncommitted: allows even uncommitted data to be read
* In most database systems, read committed is the default consistency level
	* Can be changed as database configuration parameter, or per transaction
		* set isolation level serializable


Finish of 17-Apr-2020 Lecture  32.27





