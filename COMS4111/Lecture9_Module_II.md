

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
2. (quoted from book *Database Systems: The Complete Book(2nd Edition)*

> What is a database? In essence a database is nothing more than a collection of information that exists over a long period of time, often many years. In common parlance, the term database refers to a collection of data that is managed by a DBMS. The DBMS is expected to:

A different view that makes something a database
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

3. For database applications built directly on top of file systems, problems happen；
> Data redundancy and inconsistency: data is stored  in multiple file formats resulting induplication of information in different files
> Difficulty in accessing data

    > Need to write a new program to carry out each new task

> Data isolation

    > Multiple files and formats

> Integrity problems

    > Integrity constraints  (e.g., account balance > 0) become “buried” in program code rather than being stated explicitly
    > Hard to add new constraints or change existing ones
    
4. In Module I, we explored how users interactwiththe(some) of thefunctions through DDL and DML. In module II, we will explore how DBMS implement the capabilities “under the covers.” 
> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_1.jpg)

* To some extent, mysql workben is a query tool. It is also an administration tool. In all cases, everything flow to DML, while DDL implements it. 
* query evaluation engine runs the query, almost like the interpreter, but it is highly specialized.
* buffer manager controls storage(what data is kept in memory)
* file manager determines how to open and loads data

> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_2.jpg)

* DML for users, DDL for administrators.


5. Classification of Physical Storage Media
* Can differentiate storage into:
    * volatile storage: loses contents when power is switched off
    * non-volatile storage:
        * Contents persist even **when power is switched off**.
        * Includes secondary(disk) and tertiary storage, as well as batterbacked up main-memory.
6. Factors affecting choice of storage media include
    * Speed with which data can be accessed
    * Cost per unit of data
    * Reliability

7. Storage  Hierarchy
> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_3.jpg)
In general, as goes up, the speed becomes faster, but more expensive. Everytime you go up, the cost becomes a factor of ten, also the performance.
* optical disk like physical DVDs
* magnetic disk: like an external storage for a computer. **SURVIVES power failures and system crashes**, data maybe destried due to disk failure, but that failures are rare.
    *  also referred to as the hard disk drive (HDD).
* main memory: **VOLATILE**:the contents of main memory are lost in the event of a power failure or system crash. Is word/byte addresable
* flash memory is **not volatile**. When turning off the battery the data is stil there.
    > A solid-state drive (SSD) uses ﬂash memory internally to store data
* cache mainly stores on the same chip as the processor. **the fastest and most costly form of storage**, managed by the computer system hardware. Also byte addresable because it is semiconductor.
* flash memory, magetic disk, optical disk and magnetic tapes are externary.

> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_4.jpg)


* Unit of transfer from disk to main memory is **block**. The unit of access to main memory is byte.
8. Storage technology
> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_5.jpg)
* Latency is how long it takes to read something.
* I/P Ps is how many read/writes per second.
> Although here the exact value is ancient, it does give an idea of Price and Performance

> The general observation is that 

    • Performance goes up 10X/level. 
        • Price goes up 10x per level. 
        • Note: One major change is improved price performance of SDD relative to HDD for large data.
        
        
        
A file is a set of blocks. A block contains many records but is usually not "full". There is space in a block to hold newly inserted records.

 

The DB needs to know which block to use for a newly created record/row. It can use the free space map.
