

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

* “RAID(redundant array of independent disks) is a datastorage virtualizationtechnology that combines multiple physicaldisk drivecomponents into a single logical unit for the purposes ofdata redundancy, performance improvement, or both. (…) 
    * RAID0 consists ofstriping, withoutmirroringorparity.(…) 
    * RAID1 consists of data mirroring, without parity or striping. (…) 
    * RAID2 consists of bit-level striping with dedicatedHamming-codeparity. (…) 

* The two basic level are RAID-0 and RAID-1![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_9.jpg)
    * Due to the redudency of RAID1, we need to cut the storage capacity in half since we will store each piece of data twice.
* RAID-5 and RAID-6 are other types of combinations![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/13Mar_10.jpg)
    * RAID-5 means there are 5 small "logical" disks under the *big logical disk*. It spreads the data over multiple disks. It only write the data once. If we lose element A2, we can recreate the value by computing it from the remaining blocks. Performance improves, redundancy is only 20% instead of 100%
    
    
    
    
A file is a set of blocks. A block contains many records but is usually not "full". There is space in a block to hold newly inserted records.

 

The DB needs to know which block to use for a newly created record/row. It can use the free space map.
