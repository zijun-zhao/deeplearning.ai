

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
8. [Lecture10-3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)



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
    > A solid-state drive (SSD) uses ﬂash memory internally to store data. It is called solid state because there is no moving parts 
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
        
        
9. Storage Hierarchy
* primary storage: Fastest media but volatile (cache, main memory). 
* secondary storage:next level in hierarchy, non-volatile, moderately fast access time
    * Also called **on-line storage**
    * E.g., flash memory, magnetic disks 
* tertiary storage:lowest level in hierarchy, non-volatile, slow access time
    * also called **off-line storage** and used for **archival storage**
    * e.g., magnetic tape, optical storage
    * Magnetic tape
        * Sequential access, 1 to 12 TB capacity
        * A few drives with many tapes
        * Juke boxes with petabytes (1000’s of TB) of storage

10. Storage Interfaces
* Disk interface standards families
    * SATA(Serial ATA)
            * SATA 3 supports data transfer speeds of up to 6 gigabits/sec
* SAS (Serial Attached SCSI)
    * SAS Version 3 supports 12 gigabits/sec
* NVMe(Non-Volatile Memory Express) interface
    * Works with PCIe connectors to support lower latency and higher transfer rates
    * Supports data transfer rates of up to 24 gigabits/sec 
* Disks usually connected directly to computer system

> Those standards families are the protocols to and from a disk that is connected to a computer

* In **Storage Area Networks (SAN)**, a large number of disks are connected by a high-speed network to a number of servers
* In **Network Attached Storage (NAS)** networked storage provides a file system interface using networked **file system protocol**, instead of providing a disk system interface

> SAN and NAS are sort of distributed disk systems

11. Disk Controler is a separate processor. 

* Network Attached Storage: Computers are connected through the Internet. Dedicated devices act as the disk. By talking to the controller, accessing data over network. We can think the *NAS Device* as highly specialized computer. The protocal used is TCP/IP
* Storag Area Network: different from NAS, SAN sWITCH is a specialized protocol.(Don't need to know the differences)

12. Magnetic Disks
> From the book![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_6.jpg)

* Each track is divided into sectors.
    * A sector is the smallest unit of data that can be read or written. (sector is a unit of transfer)
    * Sector size is normally bigge than 512 bytes
* To read/write a sector
    * disk arm swings to position head on right track
    * platter spins continually; data is read/written as sector passes under head 
* Disk controller–interfaces between the computer system and the disk drive hardware(basicaly a computer)
    * accepts high-level commands to read or write a sector
    * initiates actions such as moving the disk arm to the right track and actually reading or writing the data
    * Computes and attaches **checksums** to each sector to *verify that data is read back correctly*
        * If data is corrupted, with very high probability stored checksum won’t match recomputed checksum 
    * Ensures **successful writing** by reading back sector after writing it
    * Performs remapping of bad sectors(For example, if sector 23 broken, the controller could still access sector 23 but actually it remaps it to another sector)
    
13. Performance Measures of Disks
* Access time–the time it takes from when a read or write request is issued to when data transfer begins.  Consists of: 
     * **Seek** time–time it takes to reposition the arm over the correct track. 
         * Average seek time is 1/2 the worst case seek time.
             * Would be 1/3 if all tracks had the same number of sectors, and we ignore the time to start and stop arm movement § 4 to 10 milliseconds on typical disks
     * **Rotational latency**–time it takes for the sector to be accessed to appear under the head.
         * 4 to 11 milliseconds on typical disks (5400 to 15000 r.p.m.)
         * Average latency is 1/2 of the above latency.
     * Overall latency is 5 to 20 msecdepending on disk model 
> If we have a single bit of data, we still need to seek and rotate. The process are the same. The cost of getting 64 bytes are the same as the cost of getting one byte.
* Data-transfer rate –the rate at which data can be retrieved from or stored to the disk.
     * 25 to 200 MB per second max rate, lower for inner tracks
* Disk block is a logical unit for storage allocation and retrieval
    * 4 to 16 kilobytes typically
        * Smaller blocks: more transfers from disk
        * Larger blocks:  more space wasted due to partially filled blocks
* **Sequential access pattern**->*By blcok access, the next block will be i+1*
    * Successive requests are for successive disk blocks 
    * Disk seek required only for first block 
> By read and write sequentially, the blocks are kept in order. We do not need to seek the cylinder, just need to rotate to get the data

> Complex queries like group by tend to be sequential.
* **Random access pattern**
    * Successive requests are for blocks that can be anywhere on disk
    * Each access requires a seek 
    * Transfer rates are low since a lot of time is wasted in seeks
* I/O operations per second (IOPS)
* Number of random block reads that a disk can support per second 
* 50 to 200 IOPS on current generation magnetic disks     
* Mean time to failure (MTTF)–the average time the disk is expected to run continuously without any failure.
    * Typically 3 to 5 years
    * Probability of failure of new disks is quite low, corresponding to a “theoretical MTTF”of 500,000 to 1,200,000 hours for a new disk
        * E.g., an MTTF of 1,200,000 hours for a new disk means that given 1000 relatively new disks, on an average one will fail every 1200 hours
    * MTTF decreases as disk ages
    
14. Flash Storage
* NOR flash vs NAND flash
    * NAND flash
        * used widely for storage, cheaper than NOR flash
        * requires page-at-a-time read (page: 512 bytes to 4 KB) § 20 to 100 microseconds for a page read
        * **Not much difference between sequential and random read** This it is made of semiconductor
        * Page can only be written once 
            * Must be erased to allow rewrite(CANNOT UPDATE, ONLY CAN DELETE AND REWRITE: Set it all back to zero)
* Solid state disks
    * Use **standard block-oriented disk interfaces, but store data on multiple flash storage devices internally**
    * Transfer rate of up to 500 MB/sec using SATA, and up to 3 GB/sec using NVMePCIe
* **Remapping** of logical page addresses to physical page addresses avoids waiting for erase
* **Flash translation table** tracks mapping
    * also stored in a label field of flash page
    * remapping carried out by flash translation layer
> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_7.jpg)
> Just remember: if read something from block 6, you cannot rewrite block 6. Instead, you need to find an empty block, write on the empty block and then map the new block to the actual physical block. Then erase the old block and makes it on the free list. In the physical Page address spaces(where is the page physically in the system), but page actually moves.

15. Redundant Array of Independent Disk
> **Lots of physical disks look like one single disk**![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_8.jpg)
* Note the whole thing will break if two disks break at the same time
* “RAID(redundant array of independent disks) is a datastorage virtualizationtechnology that combines multiple physicaldisk drivecomponents into a single logical unit for the purposes ofdata redundancy, performance improvement, or both. (…) 
    * RAID0 consists ofstriping, withoutmirroringorparity.(…) 
    * RAID1 consists of data mirroring, without parity or striping. (…) 
    * RAID2 consists of bit-level striping with dedicatedHamming-codeparity. (…) 

* The two basic level are RAID-0 and RAID-1![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_9.jpg)
    * Due to the redudency of RAID1, we need to cut the storage capacity in half since we will store each piece of data twice.
* RAID-5 and RAID-6 are other types of combinations![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_10.jpg)
    * Availability uses parity blocks
        * Suppose I have 4 different data blocks on the logical drive A: A1, A2, A3, A4. 
            * Parity function: Ap= P(A1,A2,A3,A4) 
            * Recovery function: A2=R(Ap,A1,A3,A4
    * During normal operations:
        * Read processing simply retrieves block.
        * Write processing of A2 updates A2 and Ap      
    * If an individual disk fails, the RAID
        * Read 
            * Continues to function for reads on non-missing blocks.
            * Implements read on missing block by recalculating value. 
        * Write
            * Updates block and parity block for non-missing blocks.
            * Computes missing block, and calculates parity based on old and new value.
        * Over time
            * “Hot Swap” the failed disk.
            * Rebuild the missing data from values and parity.
    * RAID-5 means there are 5 small "logical" disks under the *big logical disk*. It spreads the data over multiple disks. It only writes the data once, it need four things to write and the error correcting data. The recovery disks(like Ap) only has the correctrion codes. If we lose element A2, we can recreate the value by computing it from the remaining blocks. Performance improves, redundancy is only 20% instead of 100%
    * has a signiﬁcant overhead for random writes, since a single random block write requires 2 block reads (to get the old values of the block and parity block) and 2 block writes to write these blocks back. 
        * In contrast, the overhead is low for large sequential writes, since the parity block can be computed from the new blocks in most cases, without any reads. RAID level 1 is therefore the RAID level of choice for many applications with moderate storage requirements and high random I/O requirements.
    * Improved performance through parallelism
        * Rotation/seek
        * Transfer
    * RAID is more often used in data center.
16. Hardware Issues
* Latent failures: data successfully written earlier gets damaged 
    * can result in data loss even if only one disk fails
    > Data being written correctly does not mean it stays correct. Electromagnetic files can corrupt data on a disk.
* Data scrubbing:
    * continually scan for latent failures, and recover from copy/parity 
    > It check on the backgroud whether the data is corrupted.
* Hot swapping: replacement of disk while system is running, without power down 
    * Supported by some hardware RAID systems
    * reduces time to recovery, and improves availability greatly
    > In a RAID-5 level, it may be a 6th disk waiting there.
* Many systems maintain spare disks which are kept online, and used as replacements for failed disks immediately on detection of failure
    * Reduces time to recovery greatly
* Many hardware RAID systems ensure that a single point of failure will not stop the functioning of the system by using
    * Redundant power supplies with battery backup
    * Multiple controllers and multiple interconnections to guard against controller/interconnection failures
17. Optimization of Disk-Block Access
* Buffering: in-memory buffer to cache disk blocks
* Read-ahead: Read extra blocks from a track in anticipation that they will be requested soon 
    * Knowing that blocks will be accessed sequentially, we will keep reading without waiting to be asked.
* Disk-arm-schedulingalgorithms re-order block requests so that disk arm movement is minimized
    * elevator algorithm: read as the elevator goes.
18. Data Access Patterns
> Data access is predicatable in a database. There are lots of queries that happen sequencially. If it is very common, we will store that table. Therefore we only need to seek once.
* Optimizing I/O performance
    * Group records into blocks if accessed together.
    * Group blocks onto the same sector/cylinder if accessed together/sequentially è Eliminates seek time. 
    * Schedule access: 
        * If I have toread cylinders 1, 9, 2, 8, 3 
        * A better ordering is 1,2,3,9,8. 
    * Stripe across disks:  Read one block from 10 disks instead one 10 from 1. 
    * Pre-fetch and preload.
    
19. Convert from cylinder head to logical address: **LBA = (( C x HPC ) + H ) x SPT + S –1 **

20. File Organization
* The database is stored as a collection of **files**.  Each file is a sequence of records. A record is a sequence of fields. 
    > Most standard mapping will be: file->table, record->row of the table, field->column
* One approach
    * Assume record size is fixed
    * Each file has records of one **particular** type only
    Different files are used for different relations This case is easiest to implement; will consider variable length records later * * We assume that records are smaller than a disk block 
> Row in relation maps to a record; A table maps to a file
* A tuple in a relation maps to a record. Records may be
    * Fixed length
        * (all the rows are the same size)
    * Variable length
        * (within a table, the row has different sizes, this may happen when the column is a combination of varchar, known and unknown)
    * Variable format (which we will see in Neo4J, DynamoDB, etc).
        * Not only the sizes are different, the columns are also differnt. You cannot do that in a relational database because they have the same columns. But there are database that rows having different columns
* A block
    * Is the unit of **transfer between disks and memory** (buffer pools). 
    * Contains multiple records, usually but not always from the same relation. 
* The database address space contains 
    * All ofthe blocks and records that the database manages 
    * Including blocks/records containing data 
    * And blocks/records containing free space.
21. ***Fixed length Record***
> The most common type of records
```sql
CREATE TABLE `products` ( 
`product_id` char(8) NOT NULL, 
`product_name` varchar(16) NOT NULL, 
`product_description` char(8) NOT NULL,
`product_brand` enum('IBM','HP','Acer','Lenovo','Some really long brand name') NOT NULL, PRIMARY KEY (`product_id`) ) ENGINE=InnoDBDEFAULT CHARSET=utf8;
````

Although the ENUM's length is not differnet, and although it is varchar. Why
* Is this fixed length? product_brandand product_nameare clearly variable length. 
* Isn’t char(8) to short for a product description?

To answer that
> “In computing,internationalization and localizationare means of adapting computer software to different languages, regional differences and technical requirements of a target locale. Internationalization is the process of designing a software application so that it can be adapted to various languages and regions without engineering changes. Localization is the process of adapting internationalized software for a specific region or language by adding locale-specific components and translating text. Localization (which is potentially performed multiple times, for different locales) uses the infrastructure or flexibility provided by internationalization (which is ideally performed only once, or as an integral part of ongoing development).” According to wiki[https://en.wikipedia.org/wiki/Internationalization_and_localization]
* We often hold the long string somewhere else, which is called **internationalization and localization**. Instead of storing long description, we will store a foreign key that stores it. Typically, we will store it as language code+where it is.
    * Retrieving a product description may require two elements 
        * Language code, e.g. (EN, DE, ES, …) 
        * Description ID, e.g. 01105432 
        * (EN, 01105432) and (DE, 01105432) are the same description in different languages (English and German) 
        * The description ID is an attribute of the product.
        * The language is an attribute or property of the user, install location, etc.
    * When going to create a schema in mysql workbench
        * It will ask which charactersets to choose.
    ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_11.jpg)

* `product_name` varchar(16) NOT NULL It will to statistical analysis. Although product_name is varchar(16) and the length can be 0 .. 15 characters. If the value is almost always between 12 and 16 characters, then the database engine may decide to store in a fixed length area. 
    * Because the relatively small savings available from treating like a variable length record
    * Does not justify the added complexity involved in variable size record management.
*  `product_brand` enum('IBM','HP','Acer','Lenovo','Some really long brand name') 
    * Those values for the enum are not stored in a table as strings
    * The DB engine would/could represent with a 1 bytecode in a **lookup table**.
    * This enables simplified, fixed length record management and saves space because
        * 1,000 products in the catalog from ‘Some really long brand’
        * Requires 1,000 bytes instead of 27,000 bytes. 
    * Repeated values: 
        * The same value occurring repeatedly in multiple distinct tuples is common, even when not an enum, e.g. 
            * Country names, City names, street names, last names, first names, 
            * Foreign keys in many-to-many associative entities. 
    * DBMS storage management engine may detect the situation, andreplace values with symbol lookups.
* We can think of the file being a stream of bytes. While the first record starts at 0. Then the second one will be n*(2-1). It will be easy to figure out where is the record is.
    * Store record istarting from byte **n * (i–1)**, where n is the size of each record. 
    * Record access is simplebut records may cross blocks 
        * Modification: do not allow records to cross block boundaries
    * On the disk, if we delete the record3, there will be an empty space. There are two basic strateges: shift stuff down or do not move the record but keep a free list.
        * move records i+ 1, . . ., n to i, . . . , n –1 
        * move record n to i 
        * do not move records, but link all free records on a free list 
        
move records i+1,..., n to i, ... , n–1       | move record n to i|  do not move records, but link all free records on a free list 
:-------------------------:|:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_12.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_13.jpg) | ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_14.jpg)

* The header of the field will have a list of free list. The related problem is that there will be more storage. It is faster(no need to move records around) but it takes more space. This happends on the block level(file is a serires of blocks). 
    * When having a data to insert, first need to find out a block from the header of the file that has space in it, and then go to that block header to find a free record. 
    * If delete a record, it also delete the index entry. It completely goes away.
    
22. ***Variable length Record***
* Variable-length records arise in database systems in several ways:
    * Storage of multiple record types in a file
    * Record types that allow variable lengths for one or more fields such as strings (**varchar**) 
    * Record types that allow repeating fields (used in some older data models).
* Attributes are **stored in order**
* Variable length attributes represented by fixed size (offset, length), with actual data stored after all fixed length attributes
* **Null values represented by null-value bitmap**
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_15.jpg) 
In the block there is a header. Inside a header there is an array of pointers to where the record are on the block.

* On a fixed-length record, you know exactly where is the record. But for the variable length record, you need to go and get the pointer to the block. 
    * Slotted pageheader contains: 
        * number of record entries
        * end of free space in the block
        * location and size of each record 
    * Records can be moved around **within a page to keep them contiguous with no empty space between them**; entry in the header must be updated. 
    * Pointers should not point directly to record —insteadthey should point to the entry for the record in header.

23. Organization of Records in Files(different approacheds to store record)
* **Heap**–record can be placed anywhere in the file where there is space
* **Sequential**–(for that file/table)store records in sequential order, based on the value of the search key of each record. Normally will sort based on the primary key.
* In a  **multitable clustering file organization** records of several different relations can be stored in the same file
    * Motivation: store related records on the same block to minimize I/O
* **B+-tree file organization**
    * Ordered storage even with inserts/deletes 
    * More on this in Chapter 14 
* **Hashing** – a hash function computed on search key; the result specifies in which block of the file the record should be placed
    * More on this in Chapter 1

24. **Heap File Organization**
* Records can be placed **anywhere** in the file where there is free space 
* Records usually do not move **once allocated**
    * Even they are are sorted, they will not move around
* Important to be able to efficiently find free space within file
* **Free-space map**
    * Array with 1 entry per block.  Each entry is a few bits to a byte, and records fraction of block that is free
    * In example below, 3 bits per block, value divided by 8 indicates fraction of block that is free

      4     | 2|  1|4|7|3|6|5|1|2|0|1|1|0|5|6
      :-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
    * Can have second-level free-space map
    * In example below, each entry stores maximum from 4 entries of first level free-space map
      4     | 7|  2|6
      :-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
* Free space map written to disk periodically, OK to have wrong (old) values for some entries (will be detected and fixed)

25. Sequential File Organization
* Suitable for applications that require **sequential processing** of the entire file
* The records in the file are ordered by a search-key 
    * It may have kind of exceptions. At the end of each record, there is a pointer to the next record. Instead of moving records around, it will be basically a linked list. Things that are in a same block are sort of close to each other in the order-> clustering
       ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_16.jpg)
* Deletion –use pointer chains
* Insertion 
    * locate the position where the record is to be inserted
    * if there is free space insert there 
    * if no free space, insert the record in an overflow block 
    * In either case, pointer chain must be updated
* Need to **reorganize the file** from time to time to restore sequential order
     * After a lot of delete and insert, we need to reorganize it and make it physically sorted, making it more efficinet. Reorder is expensive since we need to move the record and index entries(block and offset). The reorder tends to happen in the background. By far the most common scenario in at least relational database, reads dominates write, and more over, the changes are inserts. Delete tends to be rare.
26.Multitable Clustering File Organization
* Store several relations in one file using a multitableclustering file organization 
    ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_17.jpg)
* multitable clustering of department and instructor
    ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_18.jpg)
* By keeping them together in the same join.
* good for queries involving department ⨝instructor, and for queries involving one single department and its instructors
* bad for queries involving only department
* results in variable size records 
    * Records may not have the same size
* Can add pointer chains to link records of a particular relation
    * But since the records are fixed. We can manage it and do optimization due to the source are two fixed records.
> Note that most of the time one table one file. But it is allowed to have multiple files in one table. The record size will be the biggest recrod size. In this example we will pick the length of instructor. All those records will have header to indicate how to interprete the sequences. From a data structure point of view, it is similar to keep two linked lists that interwove into the same array. This multitable clustering is not common, but if you will need to join two tables frequently, this will be an efficient way.

27. Partitioning
* **Table partitioning**: Records in a relation can be partitioned into smaller relations that are stored separately 
    * E.g., transaction relation may be partitioned into transaction_2018, transaction_2019, etc.
* Queries written on transactionmust access records in all partitions
    * Unless query has a selection such as year=2019, in which case only one partition in needed
* Partitioning 
    * Reduces costs of some operations such as free space management
    * Allows different partitions to be stored on different storage devices
        * E.g., transaction partition for current year on SSD, for older years on magnetic disk

28.Data Dictionary Storage
The **Data dictionary**(also called **system catalog**) stores metadata; that is, data about data, such as
* Information about relations
    * names of relations
    * names, types and lengths of attributes of each relation 
    * names and definitions of views 
    * integrity constraints 
* User and accounting information, including passwords 
* Statistical and descriptive data
    * number of tuples in each relation
* Physical file organization information
    * How relation is stored (sequential/hash/…)
    * Physical location of relation
* Information about indices (Chapter 14) 

show tables from information_schema    | select * from information_schema.view_table_usage| select * from information_schema.statistics where table_schema='newbook'; 
:-------------------------:|:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_19.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_20.jpg) | ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_21.jpg)


29. Relational Represemtation of system metadata
   ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_22.jpg)
Specialized data structures designed for efficient access, in memory
> By codd's rule: all the metadata is itself relational data. Most databases keep the metadata stored in a way different from the data. But in relational database, metadata are stored as the same as the data.

```sql
select * from information_schema.tables;
```

* In the table below, there is a row describing that table. In the metadata for tables, there is a row that describes the table that holds the metadata for tables.
```sql
select * from information_schema.tables where table_name='table';
```
* All tables except the previous table
```sql
select * from information_schema.tables -- where table_name='table';
```

30. A file is a set of blocks. A block contains many records but is usually not "full". There is space in a block to hold newly inserted records.

 

The DB needs to know which block to use for a newly created record/row. It can use the free space map.
