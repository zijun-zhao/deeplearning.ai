

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
8. [Lecture10-29Mar](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)



* Course Modules 
  * Reminder of DBMS implementation and Module II. 
  * DisksandData Input/Output 
  
## 29 Mar 2020
1. Data Management
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_1.jpg)
* Storage manager: loads and stores blocks(external storage). Key idea is to minimize the number of block transfers between the disk and memory
    * Block is what has been transfered between the external storage devices and the computer memory.

* Buffer manager: manages how records lay out in blocks. A RAM buffer.
    * The database manager requests and pins physical memoery. 
* Index/file/record manager: **important**

2. Row versus Columnar Storage

> from [link](https://softwareengineeringdaily.com/2017/01/13/columnar-data-apache-arrow-and-parquet-with-julien-le-dem-and-jacques-nadeau/) ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_2.jpg)
* Row-oriented Database: A block would have a lot of rows. Rows are contiguous in a block. Read and write all the rows when read and write the block
     * When tending to be ineractive and transactional, like someone update their profile, will be better.
* Column Oriented Database. When want to read  a row, possibly we need to read five blocks. There are tradeoff between these two database.
    * also known as **columnar representation**
    * Store each attribute of a relation separately 
    * Column Oriented data storage allows us to **access all the entries in a database column quickly and efficiently**. Columnar storage formats are mostly relevant today for performing large analytics jobs
        * When tending to do group by, it is more efficnet
        * As big data comes, column-oriented database will be more common.
    * Pandas is column oriented
3. Columnar Representation
* Benefits: 
    * Reduced IO if only some attributes are accessed 
    * Improved CPU cache performance 
    * Improved compression 
    * **Vector processing** on modern CPU architectures 
* Drawbacks
    * Cost of **tuple reconstruction** from columnar representation 
    * Cost of tuple deletion and update
    * Cost of decompression
* Columnar representation found to be more efficient for **decision support** than row-oriented representation
* Traditional row-oriented representation preferable for transaction processing 
* Some databases support both representations 
    * Called hybrid row/column store
4. Storage Access
   
* Blocks are units of both **storage allocation** and **data transfer**.
* Database system seeks to minimize the number of block transfers between the disk and memory.
    * when the block is in the memoery, we call it a page; when it is on a disk we call it a blcok.
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_3.jpg)
* We can reduce the number of *disk accesses* by keeping as many blocks as possible in main memory.
* **Buffer**: portion of main memory available to store copies of disk blocks. 
* **Buffer manager**–subsystem responsible for allocating buffer space in main memory.
    * CPU accesses the memory, not the disk. So buffer manager will minimize the number of disk I/O
    
5. The logical Concept
        ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_4.jpg)
* The DBMS and queries can only **manipulate in-memory blocks and records**. 
* A very, very, very small fraction of all blocks fit in memory.
* The block spaces of the disk memory. Same size slots are in memory that holding the data. Query manages page in the buffer pool. The way to classfy the block in buffer pool is 
    * whether it is free or now, does it has block in it?
    * has that page been updated? Is the block dirty?
        * If did, we need to write the block back in the disk to make the page consistent.
        * If the block has not been updated, we can decide whether to release it or not.
* Once the buffer pool is full and new blocks are required, we need to perform **block replacement**, we need to refer to the **replacement policy**
6. Buffer Manager
* Programs call on the buffer manager when they **need a block** from ***disk***. 
* If the block is already in the buffer, buffer manager returns the address of the block in main memory
* If the block is not in the buffer, the buffer manager
    * Allocates space in the buffer for the block
        * Replacing (throwing out) some other block, if required, to make space for the new block.
        * Replaced block written back to disk only **if it was modified since the most recent time** that it was written to/fetched from the disk.
    * Reads the block from the disk to the buffer, and returns the address of the block in main memory to requester.
* **Pinned block**: memory block that is **not allowed to be written back to disk**
    * **Pin** done before reading/writing data from a block
    * **Unpin** done when read /write is *complete*
    * Multiple concurrent pin/unpin operations possible 
        * Keep a pin count, buffer block can be evicted only if pin count = 0
* **Shared and exclusive locks on buffer**
    * Needed to prevent concurrent operations from reading page contents as they are moved/reorganized, and to ensure only one move/reorganize at a time
    * Readers get shared lock, updates to a block require exclusive lock
    * **Locking rules**: 
        * Only one process can get exclusive lock at a time 
        * Shared lock cannot be concurrently with exclusive lock
        * Multiple processes may be given shared lock concurrently
        
7. Buffer-Replacement Policies
* Most operating systems replace the block **least recently used**(LRU strategy)
    * Idea behind LRU –use past pattern of block references as a predictor of future references 
    * LRU can be bad for some queries
* Queries have well-defined access patterns (such as sequential scans), and a database system can use the information in a user’s query to predict future references
* Mixed strategy with hints on replacement strategy provided by the query optimizer is preferable
* Example of bad access pattern for LRU: when computing the join of 2 relations r and s by a nested loops 
```
for each tuple tr of r do
  for each tuple ts of s do
    if the tuples trand ts match …
```
* The most efficient caching algorithm would be to always discard the information that will not be needed for the longest time in the future. This optimal result is referred to as **Bélády's optimal algorithm**/**simply optimal replacement policy** or the **clairvoyant algorithm** 


8. If the buffer pool is large and we want a sorted time stamp, everytime we touch a page we need to update the time. Instead of thinking the memory as a list of pages, we think it as a circle of pages: ***The “Clock Algorithm” ***

* LRU is (perceived to be) expensive 
    * Maintain timestamp for each block. 
    * Update and resort blocks on access.
* The “Clock Algorithm” is a **less expensive approximation**. 
    * Arrange the frames (places blocks can go) into a logical circle like the seconds on a clock face. 
    * Each frame is marked 0 or 1. 
        * Set to 1 when block added to frame.
        * Or when application accesses a block in frame.
    * Replacement choice 
        * Sweep second hand clockwise one frame at a time.
        * If bit is 0, choose for replacement. 
        * If bit is 1, set bit to zero and go to next frame. 
            * It means we swept by it at least once and no one asks for it.
    * The basic idea is. On a clock face 
        * If the second hand is currently at 27 seconds.
        * The 28 second tick mark is “the least recently touched mark.”
9. Replacement Algotirhm
* The algorithms are more sophisticated in the real world, e.g.
    * “Scans” are common, e.g. go through a large query result in order (will be more clear when discussing cursors). 
        * The engine knows the current position in the result set.
        * Uses the sort order to determine which records will be accessed soon. 
        * Tags those blocks as not replaceable. 
        * (A form of clairvoyance).
    * Not all users/applications are equally “important.”
        * Classify users/applications into priority 1, 2 and 3.
        * Sub-allocate the buffer pool into pools P1, P2 and P3.
        * Apply LRU within pools and adjust pool sizes based on relative importance.
        * This prevents
            * A high access rate, low-priority application from taking up a lot of frames 
            * Result in low access, high priority applications not getting buffer hits
        > A lot can be done. Why does a database engine manage its storage differently than the operating system? 
10. We will cover Optimization of Disk Block Access when talking about transaction. 
* Note here when we need to replace a page, we need to write the old page to a disk before we load the new page from the disk to the memory. This process takes time. To save more time, instead, people choose to write and change to a fast disk: read the data in and move it over in the background. Suppose we have a non volatile RAM(like a semiconductor), we write the block in the RAM and move it back to the disk later. In that case, we do not risk losing the thing did. This can get rid of two slow I/O and need a fast write and a slow read.

 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_4.jpg)
 
11. 
