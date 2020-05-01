

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
8. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)
8. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)


* Course Modules 
  * 
  * 
  
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
* Column Oriented Database. When want to read  a row, possibly we need to read five blocks. There are tradeoff between these two kinds of database.
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
        * Replaced block **written back** to disk **only if** it was modified since the most recent time that it was written to/fetched from the disk.
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
        * Worst possible algorithm for join    
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
        * If bit is 1, **set bit to zero** and go to next frame. 
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
        > A lot can be done. The above answers the question **Why does a database engine mantain on buffer pool, manage its storage differently than the operating system?**
10. We will cover Optimization of Disk Block Access when talking about transaction. 
* Note here when we need to replace a page, we need to write the old page to a disk before we load the new page from the disk to the memory. This process takes time. To save more time, instead, people choose to write the change to a fast disk: read the data in and move it over in the background. Suppose we have a non-volatile RAM(like a semiconductor), we write the block in the RAM and move it back to the disk later. In that case, we do not risk losing the update. This can get rid of two slow I/O and need a fast write and a slow read.

 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_4.jpg)
 
11. Basic Concepts of **Indexes**
* Indexing mechanisms used to speed up access to desired data.
    * E.g., author catalog in library 

   
* **Search Key**-attribute to set of attributes used to look up records in a file.
* An index file consists of records (called index entries) of the form
    search-key | pointer
    --- | --- 
    * Just like the card catelog in big bang theory, search-key is the author's name, often a combination of columns. pointer will be the location ![Image of Yaktocat](https://i.pinimg.com/originals/d9/8a/4a/d98a4abc61c8d3dead42031c4625c6ec.jpg)
* Index files are typically **much smaller** than the original file 
    * A table is logically a file, and indexes is another file tells you how to find the record fast
* Two basic kinds of indices: 
    * Ordered indices:  search keys are stored in sorted order
         * The order of entry in the index is the same of the ordering of the entries of the files.
    * Hash indices: search keys are distributed uniformly across “buckets” using a “hash function”.

12. Index Evaluation Metrics
* Access types supported efficiently.  E.g.,
    * Records with a specified value in the attribute 
    * Records with an attribute value falling in a specified range of values. 
* Access time
* Insertion time
* Deletion time 
* Space overhead


13. Ordered Indices
* In an ordered index, index entries are stored sorted **on the search key** value.  
    * sorted using a search key
* **Clustering index**: in a sequentially ordered file, the index whose search key **specifies the sequential order** of the file. 
    * Also called primary index
        * The sorted order in the index determines the sorted order in the file.
    * The search key of a primary index is usually but not necessarily the primary key.
* **Secondary index**: an index whose search key specifies an order **different from** the sequential order of the file.  
    * Also called nonclusteringindex.
    * For example, rows in the table can only be sort in one way. By scanning the table, it is either sorted by the s_ID(then it is a clustering index) or by the i_ID. According to [this page](https://www.guru99.com/indexing-in-database.html):
        > In a bank account database, data is stored sequentially by acc_no; you may want to find all accounts in of a specific branch of ABC bank. Here, you can have a secondary index for every search-key. Index record is a record point to a bucket that contains pointers to all the records with their specific search-key value.

index information | columns in the table
--- | --- 
 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_5.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_6.jpg)
* **Index-sequential** file: sequential file ordered on a search key, with a clustering index on the search key.
14. When classifying indices
    * Is the index sorted on/not on the key
        * Is it clustered or not
    * Is it sparse?
        * If it is dense, then **Index record appears for every search-key value in the file**
15. Several examples

ordered, dense, clustered | ordered, but not clustered
--- | --- 
 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_7.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_8.jpg)
 
16.  **Sparse Index Files**
> does not have entry for every distinct key value
* Sparse Index:  contains index records for only some search-key values.
    * Applicable when records are sequentially ordered on search-key
* To locate a record with search-key value Kwe: 
    * Find index record with largest search-key value **< K**
    * Search file sequentially starting at the record to which the index record points 
> In general, there can be only one sparse index


> Secondary indices **have to be** ***dense***
        ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_9.jpg)
        
17. Clustering vs Nonclustering Indices
* Indices offer substantial benefits when searching for records.
* BUT: indices imposes overhead on database modification
    * when a record is inserted or deleted, every index on the relation must be updated
    * When a record is updated, any index on an updated attribute must be updated
* Sequential scan using clustering index is efficient, but a sequential scan *using a secondary (nonclustering) index is expensive on magnetic disk*
    * Each record access may fetch a new block from disk 
    * Each block fetch on magnetic disk requires about 5 to 10 milliseconds
    
    
18. Multilevel Index
> Although Index files are typically **much smaller** than the original file, they can still be very large

> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_10.jpg)
* If index does not fit in memory, access becomes expensive.
* Solution: treat index kept on disk as a sequential file and construct a sparse index on it. 
    * outer index: a sparse index of the basic index
    * inner index: the basic index file
* If even outer index is too large to fit in main memory, yet another level of index can be created, and so on. 
* Indices at all levels must be *updated on insertion or deletion from the file*.

19. B+-Tree Index Files
> If the indexes entries are stored in order in an index, then it is a sequential index. This will be a binary search tree called B+ Tree. By large most indexes are B+ Tree
* Disadvantage of indexed-sequential files
    * Performance degrades as file grows, since many overflow blocks get created.  
    * Periodic reorganization of entire file is required.
* Advantage of B+-tree index files:  
    * Automatically reorganizes itself with small, local, changes, in the face of insertions and deletions.  
    * Reorganization of entire file is not required to maintain performance.
* (Minor) disadvantage of B+-trees: 
    * Extra insertion and deletion overhead, space overhead.
* Advantages of B+-trees outweigh disadvantages
    * B+-trees are used extensively
    
    
* In the following example, in the table *course*, there are two B+ tree but only one of them is clustered since the file can only have one sort order

clustered B+ Tree | B+ Tree
--- | --- 
 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_11.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_12.jpg)
 
20. Example of B+-Tree
 > ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_13.jpg)
* Every node in the tree has **multiple** entries, it is not a binary tree. 
* The degree out can be larger than the B+ node. Since the index nodes are data on disks, if we have to read a block to get the data, why do not we make the internal nodes the same size as the block size. Therefore, the leaves are always **sorted in order**. The order may or may not be the same as the record order. Multilevel index's entries tells us where to go to get the record.
     * The degree is the a little ambiguous. But it will always keep the tree at the same depth by **rebanlancing**.
          * If the maximum degree is 7, then based on the size of the disk block and size of key, we can fix 6 keys in a block.
21. How many keys can be stored in a block?
 > Depends on the key size and the block size. For example, if we alter a column type from varchar(128) to varchar(16), the database will know it can compress more key inside a block and it will rebuild the index, packing more keys to the index.
 
22. B+-Tree Node Structure
 
P<sub>1</sub> | K<sub>1</sub>|P<sub>2</sub>|...|P<sub>n-1</sub>| K <sub>n-1</sub>|K <sub>n</sub>
--- | --- | --- | --- | --- | --- | --- 
* Typical node
    * Ki are the search-key values
    * Pi are pointers to children (for non-leaf nodes) or pointers to records or buckets of records (for leaf nodes).
* The search-keys in a node are ordered: K<sub>1</sub><K<sub>2</sub> <K<sub>3</sub> <. . .<K<sub>n-1</sub> (Initially assume no duplicate keys, address duplicates later) 
    * Scan until find the key bigger
    
    

23. Leaf Nodes in B+-Trees
 > ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_14.jpg)
*  For i= 1, 2, . . ., n–1, pointer Pipoints to a file record with search-key value Ki
* If L<sub>i</sub>, L<sub>j</sub> are leaf nodes and i< j, L<sub>i</sub>'s search-key values are less than or equal to L<sub>j</sub>'s search-key values
* P<sub>n</sub> points to next leaf node in search-key order 



24. Other Issues in Indexing
* Record relocation and secondary indices
* If a record moves, all secondary indices that store record pointers have to be **updated**
* Node splits in B+-tree file organizations become very expensive 
* Solution: use search key of B+-tree file organization instead of record pointer in secondary index
    * Add record-id if B+-tree file organization search key is non-unique
    * Extra traversal of file organization to locate record 
        * Higher cost for queries, but node splits are cheap
* In practice, we will only index what we really need.

25. Prefix compression of index string
> If we have an index that repeat a lot, the prefix of it is a string and it repeats a lot, we do not need to store it
* Key values at internal nodes can be prefixes of full key 
* Keep enough characters to distinguish entries in the subtrees separated by the key value 
    * E.g., “Silas”and “Silberschatz”can be separated by “Silb” 
* Keys in leaf node can be compressed by sharing common prefixes
    > An example from [oracle](https://blogs.oracle.com/dbstorage/compressing-your-indexes:-index-key-compression-part-1)
    
    ![Image of Yaktocat](https://cdn.app.compendium.com/uploads/user/e7c690e8-6ff9-102a-ac6d-e4aebca50425/f4a5b21d-66fa-4885-92bf-c4e81c06d916/Image/c4c492757c4828427162e94306d565f4/indexkeycompressionimg.png)
    
    
26. Hashing is always not sorted
* Buckets->overflow buckets
* A bucketis a unit of storage containing one or more entries (a bucket is typically a disk block).
    * we obtain the bucket of an entry from its search-key value using a hashfunction 
* In a hash index, buckets store entries with pointers to records 
* In a hash file-organization buckets store records
> ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_15.jpg)
* Bucket overflow can occur because of 
    *  **Insufficient buckets**
    * Skew in distribution of records. This can occur due to two reasons: 
        * multiple records have **same search-key value** 
        * chosen hash function produces non-uniform distribution of key values 
    * Although the probability of bucket overflow can be reduced, it cannot be eliminated; it is handled by using **overflow buckets**.
* An example of Hashing Visualization. Here use *Simple Mod Hash* with table size 7


First insert 3 | Then insert 10, 10 mod 7 is 3|same hash position|Insert at position 4
--- | --- | --- | --- 
 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_16.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_17.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_18.jpg)| ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_19.jpg)
    

* **Overflow chaining**: the overflow buckets of a given bucket are chained together in a linked list.    
* Above scheme is called **closed addressing** (also called closed hashing or open hashing depending on the book you use) 
    * An alternative, called **open addressing** (also called open hashing or closed hashing depending on the book you use) which does not use overflow buckets, is not suitable for database applications. 
* Note if we change the number of root buckets, the hash value of everything will change. Basicly every record need to be moved. Reorganization caused a lot.
    
    
    
27. Update anomaly problem, some rows are functions of other rows.


28. Deficiencies of Static Hashing
* In static hashing, function hmaps search-key values to a fixed set of B of bucket addresses. Databases grow or shrink with time.
    * If initial number of buckets is too small, and file grows, performance will degrade due to **too much overflows**. 
    * If space is allocated for anticipated growth, a significant amount of space will be wasted initially (and buckets will be **underfull**). 
    * If database shrinks, again space will be wasted. 
* One solution: periodic re-organization of the file with a new hash function 
    * Expensive, disrupts normal operations 
* Better solution: allow the number of buckets to be **modified dynamically**. 

## 3 Apr 2020

1. Indices on Multiple Keys
* Composite search keysare search keys containing more than one attribute
    * E.g., (dept_name, salary)
        * Then first sort according to dept_name, and within a same dept_name sort according to salary
        * In this example, it does not help with the "or" problem, only help with the "and" problem
        * With the where clause *where dept_name = “Finance” and salary = 80000* the index on (dept_name, salary) can be used to fetch only records that satisfy **both conditions**. 
            * Using separate indices in less efficient: we may fetch many records (or pointers) that satisfy only one of the conditions. 
        * Can also efficiently handle wheredept_name= “Finance”and salary < 80000 
        * But **cannot** efficiently handle *where dept_name < “Finance” and balance = 80000*
            * May fetch many records that satisfy the first but not the second condition

* Lexicographic ordering: (a<sub>1</sub>, a<sub>2</sub>) < (b<sub>1</sub>, b<sub>2</sub>) if either
    * a<sub>1</sub> < b<sub>1</sub>, or
    * a<sub>1</sub>=b<sub>1</sub>and  a<sub>2</sub>< b<sub>2</sub>
    
2. An exmple of multiple keys application
    * If create an index of customer's name, choose last name first, then first name
 ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_20.jpg)
 
```sql
ALTER TABLE `classicmodels`.`customers` 
ADD INDEX `customor_name` (`contactLastName` ASC, `contactFirstName` ASC) VISIBLE;

```

* On the contrary, if do the contrary
   * This will not help if you look somebody up only knowing the last name
   ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_21.jpg)
```sql
ALTER TABLE `classicmodels`.`customers` 
ADD INDEX `customor_name` (`contactFirstName` ASC, `contactLastName` ASC) VISIBLE;

```

* What we start first will help us to find what we want, if we want to find both from first name and last name, we need to add another index

   
3. Covering indices 
    * Add extra attributes to index so (some) queries can avoid fetching the actual records 
        * It is key that columns in it that I do not need to make it a key. But by puting the column in the covering key, despite the fact that the columns are redundant, we will be able to retrieve data from the index, **not from the data file**: no need to read and retrieve the data file
        
        
4. Query Processing

         ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/29Mar_22.jpg)
* After the SQL query comes in texxt, it get parsed and translated to some executable languages. CPU will understand how to deal with it using Optimizer. The optimizer pools in statistics of information to decide how to execue it.
    * For example, there are two ways to implement σsalary<75000(instructor)
        * Scan the instructor table and then find everything that matches
        * Use index, use the refinement first and do the scan next
5. Is index a pointer?
* As answered by Prof. Ferguson:
    > If given this tree  ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/Bplustree.png)
* Then although data are in the same file, the block may be different, the index file may look like. It points to where the record is

Key Value | Record Location
--- | --- 
a2|(/user/local/myswl,data/s1/f1, 21,7)
a4|(/user/local/myswl,data/s1/f1, 32,101)

* We can sort the index file according to record, in this case it is the actual data

Key Value | Record 
--- | --- asdfadg
a4|sfgshj



6. Measures of Query Cost
* Many factors contribute to time cost 
    * disk access, CPU, and network communication 
    * An image of storage network![img](https://cdn.ttgtmedia.com/rms/onlineImages/network_attached_storage.jpg)
* Cost can be measured based on 
    * **response time**, i.e. total elapsed time for answering query, or
        * Resource consuption+time in lien
    * **total resource consumption**
        * When you get to the head of the line, how long it takes someone to check you in
* We use total resource consumption as cost metric 
    * Response time harder to estimate, and minimizing resource consumption is a good idea in a shared database
* We ignore CPU costs for simplicity 
    * Real systems do take CPU cost into account 
    * Network costs must be considered for parallel systems § We describe how estimate the cost of each operation
* We do not include cost to writing output to disk


7. Selection Operation
* File scan 
    * Algorithm A1(**linear search**).  Scan each file block and test all records to see whether they satisfy the selection condition. 
        * Cost estimate = brblock transfers + 1 seek
            * b<sub>r</sub> denotes number of blocks containing records from relation r
    * If selection is on a key attribute, can stop on finding record
        * cost = (b<sub>r</sub>/2) block transfers + 1 seek
    * Linear search can be applied regardless of 
        * selection condition or
        * ordering of records in the file, or 
        * availability of indices 
    * Note: binary search generally does not make sense since data is not stored consecutively
        * except when there is an index available, 
        * and binary search requires more seeks than index search
* Index scan: search algorithms that use an index
    * selection condition must be on search-key of index.
    * A2 (clustering index, equality on key).  Retrieve a single record that satisfies the corresponding equality condition  
        * Cost= (h<sub>i</sub>+ 1) * (t<sub>T</sub>+ t<sub>S</sub>)
    * A3 (clustering index, equality on nonkey)Retrieve multiple records.
        * Records will be on consecutive blocks 
            * Let b = number of blocks containing matching records 
            * Cost= h<sub>i</sub>* (t<sub>T</sub>+ t<sub>S</sub>)+ t<sub>S</sub>+ t<sub>T</sub>* b
    * A4(secondary index, equality on key/non-key). 
        * Retrieve a single record if the search-key is a candidate key 
            * Cost = (hi+ 1) * (tT+ tS) 
        * Retrieve multiple records if search-key is not a candidate key 
        * each of n matching records may be on a different block 
            * Cost =  (hi+ n) * (tT+ tS)
            * Can be very expensive!
       > Each index entry is going to refer to a block, but we still need to do block I/O in the worst case
8. First look at the select condition, then determine which indices apply. Then thinking about the index selectivity and clustering.



