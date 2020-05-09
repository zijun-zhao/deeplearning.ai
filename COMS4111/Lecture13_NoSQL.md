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
12. [Lecture12-17Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture13_NoSQL.md)



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

1. NoSQL Databases
> Database did not support sql, or completely not relational. Original, all databases are noSQL. Then relational database come along, everything become relational database. People begin to realize relational has limitations later.

> Now NoSQL tends to mean "not only SQL", it will suppoty a sql-like query, and it will not be strictly relational, it will relax integrity and consistency constraint.
	* However, by forcing integrity and consistency, relational database limits the scalibility and availability.

* The primary difference between NoSQL and SQL database: Many NoSQL stores compromise consistency. 


> Instead, most NoSQL databases offer a concept of "**eventual consistency**" in which database changes are propagated to all nodes "eventually" (typically within milliseconds) so queries for data might not return updated data immediately or might result in reading data that is not accurate, a problem known as stale reads


2. Types of NoSQL database:
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_15.jpg)
* What is the underline base data model?
	* relational database: Tables, tabula data 
	* document database: json is hierarchical->mongoDB
	* graph databases: data model is graph.
	* wide column stores
	* key-value database
	
3. CAP Theorem: foundamental theory
	* Consistency: Every read receives the most recent write or an error.
	* Availability: Every request receives a (non-error) response, without guarantee that it contains the most recent write.
	* Partition Tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
		* The system continues to operate even though there are failures.
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_16.jpg)

* There are no database that can satisfy these three, only two of them can be satisfied.

4. Consistency Models
* **STRONG CONSISTENCY**: Strong consistency is a consistency model where all subsequent accesses to a distributed system will always return the updated value after the update.
* **WEAK CONSISTENCY**: It is a consistency model used in distributed computing where subsequent accesses might not always be returning the updated value. There might be inconsistent responses.
	* processer cache,
* **EVENTUAL CONSISTENCY**: Eventual consistency is a special type of weak consistency method which informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value.
	* if you keep reading, you will eventually see the recent value
	* Due to availablity, as the figure below, you will finally get the value you write if you keep reading. However, problem of overwrite may happen.  
Availability and scalability via| Several algorithms for conflict resolution|Illustration
---|---|---
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_18.jpg)|Detect and handle in application&Clock/change vectors/version numbers|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_17.jpg)

* Another tradeoff for eventual consistency: Partition Tolerance
	* Database get logging, before get the put, we can lock all the copys, then we can do the *put* then release all the copy. But we cannot lock all the copy if there are partitions. In a replicate database, we can get consistency through locking, but we will not able to lock a lot of copies. For availablity, we need multiple copies. For consistency, we need lock all the copies since we do not tolerate partitions. If we have partition tolerance, we cannot lock all the copies, then we do not have consistency.

> How many replicate are there? It depends on how many replicate you need. Replicate is for performance and avialbility. If you care for availability, then you will only need two.


