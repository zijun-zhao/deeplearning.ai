

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
8. [Lecture9-13Mar](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture9_Module_II.md)



* Course Modules 
  * Reminder of DBMS implementation and Module II. 
  * DisksandData Input/Output 
  
## 13 Mar 2020
1. Four distinct module: 
    * Foundation Module: relational data model, data modeling and SQL.
    * Implementation, what goes on inside a DBMS.
    * NoSQL databases, like graph database. They also rely heavily on implementations covered module II.
    * Data analytics
2. 

> What is a database? In essence a database is nothing more than a collection of information that exists over a long period of time, often many years. In common parlance, the term database refers to a collection of data that is managed by a DBMS. The DBMS is expected to:

A different view that makes something a database(quoted from book *Database Systems: The Complete Book(2nd Edition)*

* 1. Allow users to create new databases and specify their schemas (logicalstructure of the data), using a specialized data-definition language
    * Similar to creating classes in java. 
* 2. Give users the ability to query the data (a “query” is database lingo for a question about the data) and modify the data, using an appropriate language, often called a query language or data-manipulation language.
    * We spent the first module covering this topic. Sometimes called the functional requirements: functions that what it does.
* 3. Support the storage of very large amounts of data — many terabytes or more — over a long period of time, allowing efficient access to the data for queries and database modifications. 
    * Not what it does. But how it does.
* 4. Enable durability, the recovery of the database in the face of failures, errors of many kinds, or intentional misuse.
    * Not what it does. But how it does.
* 5. Control access to data from many users at once, without allowing unexpected interactions among users (called isolation) and without actions on the data to be performed partially but not completely (called atomicity).
    * Not what it does. But how it does.


A file is a set of blocks. A block contains many records but is usually not "full." There is space in a block to hold newly inserted records.

 

The DB needs to know which block to use for a newly created record/row. It can use the free space map.
