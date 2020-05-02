

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
  
## 10 Apr 2020


1. Recall
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_1.png)
    * Execution Engine: Runs query quickly
    * Index/file/recored manager: locate tuples and rows quickly
    * Buffer&Buffer manager: optimize transfer between memory and disk, also use caching to guarantee block access in memory.
    * Control access assures the multiple users/programs do not accidently overwrite/corrupt data.
    * Durability ensures that when a transaction commits, update is not lost.
2. Recall **Basic Steps in Processing an SQL Query**

![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_2.jpg)
    * After the SQL query comes in text, it get parsed and translated to some executable languages(similar to the Compilation). The internal expression is transfered in the relational algebra expression. The evaluation tree we have seen before(the operators are nodes, edges are lines that flow through). The Optimizer takes the statistics and determine the optimal way to evaluate the query, then generate the execution. Evaluation Engine like java's VM/ Python's interpreter.

3. Query execution: 
    > the algorithms that manipulate the data of the database
4. Preview of Query Compilation from [Database Systems](http://infolab.stanford.edu/~ullman/dscb.html)
    * Query compilation is divided into the three major steps
        * a) Parsing. A parse tree for the query is constructed. 
        * b) Query Rewrite. The parse tree is converted to an initial query plan, which is usually an algebraic representation of the query. This initial plan is then transformed into an **equivalent** plan that is expected to require less time to execute. 
        * c) Physical Plan Generation. The abstract query plan from (b), often called a logical query plan, is turned into a physical query plan by selecting algorithms to implement each of the operators of the logical plan, and by selecting an order of execution for these operators. The physical plan, like the result of parsing and the logical plan, is represented by an expression tree. The physical plan also includes details such as how the queried relations are accessed, and when and if a relation should be sorted.
            * Parts (b) and (c) are often called the query optimizer, and these are the hard parts of query compilation. 

4. Parsing  
* The output is a **Parse Tree**
```sql
SELECT movieTitle 
FROM Starsln 
WHERE starName IN ( 
    SELECT name 
    FROM MovieStar 
    WHERE birthdate LIKE 17.1960’
);

```
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_3.jpg)
    
    
5. Simple Execution/Plan Tree
* The nodes are SQL operators. The operator parameters are:
    * Relations.
    * Parameters to operators.
* The edges are the flow of data. The data flowing is a relation.
    * Base table.
    * Derived table.
6.Logical Plan
> Here is an example from [this website]()

Logical Query|PlanDistributed Query Plan
---|---
![fig1](https://gerardnico.com/_media/data/type/relation/engine/logical_query_plan.png?w=300&tok=0fc4d4) | ![fig1](https://gerardnico.com/_media/data/type/relation/engine/distributed_query_plan.png?ezimgfmt=rs:607x454/rscb1/ng:webp/ngcb1)
```sql
SELECT
    item.brand,
    SUM(price)
FROM
    sales,
    item
WHERE
    sales.item_key = item.item_key
GROUP BY item.brand
```
* The plan executes in stages (bottom up).
* Some stages can have parallelism, e.g. multiple threads to improve CPU utilization while processing I/O.
    * Here 'Scan on item' is a select, two different threads can run at the same time: 'scan on item' and 'scan on sales'.
    
7. Query Optimization from [Database Systems](http://infolab.stanford.edu/~ullman/dscb.html)
    * Which of the algebraically equivalent forms of a query leads to the **most efficient** algorithm for answering the query? 
    * For each operation of the selected form, what algorithm should we use to implement that operation?
    * How should the operations pass data from one to the other, e.g., in a pipelined fashion, in main-memory buffers, or via the disk?
* Each of these choices depends on the metadata about the database. Typical metadata that is available to the query optimizer includes: the size of each relation; statistics such as the approximate number and frequency of different values for an attribute; the existence of certain indexes; and the layout of data on disk.
    
8. Three Core Optimization Techniques

* Query rewrite: Transform the logical query into an equivalent, more efficient query (lower cost query), e.g.
   * R ⋈ S is the same as S ⋈ R
   * 𝞼(R ⋈ S) is the same at 𝞼(R) ⋈ 𝞼(S)
       * cost of **join** is size of table R times size of table S. 100*100=10 000. R ⋈ S is row(R)*row(S), the select cost is 1.
       * cost of 𝞼(R) is 1 if it is performed on the primary key, therefore cost of 𝞼(R) ⋈ 𝞼(S) is 100
   * 𝛅(R ⋈ S) = 𝛅(R) ⋈ 𝛅(S), where 𝛅 is the distinct operator.
* Access path selection: Which index to choose?
* Operator implementation selection:
    * There are several related implementations for each operator
    * For example,
        * Sort Scan
        * Sort Join
        * Hash Join
        * Hash Distinct
9. Statistics and Catalogs
    * Need information about the relations and indexes involved.  Catalogs typically contain at least:
        * **tuples (NTuples)** and **pages (NPages)** for each relation.
        * **distinct key values (NKeys)** and **NPages** for each index.
        * **Index height, low/high key values (Low/High)** for each tree index.
    * Catalogs updated periodically.
    * Updating whenever data changes is too expensive; lots of approximation anyway, so slight inconsistency ok.
    * More detailed information (e.g., histograms of the values in some field) are sometimes stored
10. The Catalog Contains
    * For each table
        * Table name, storage information.
        * Attribute name and type for each column.
        * Index name, type and indexed attributes
        * Constraints
    * Statistical information
        * Cardinality of each table
        * Size: No. of blocks.
             * If there are lots of rows and rows are big, there will be a lot of blocks
        * Index cardinality: Number of **distinct key values** for each index
            * If the key is primary/unique, then the number of dinstinct key value = number of rows; But if it is a secondary index, the number of index will be less than number of rows.
        * Index size: Number of blocks for each index.
             * An index is a subset of the column.
        * Index tree height
            * how many levels
        * Index ranges
11. Access Path
* Every relational operator accepts one or more tables as input.The operators “accesses the tuples” in the tables. 
* There are two “ways” to retrieve the tuples
* Scan the relation (via blocks)
* Use an index and matching condition.
    * if there is a condition in the where clause. No need to scan the table, but need to go through the index. Therefore the height of index tree matters.
* A selection condition is in **conjuctive normal** form if
    * Each term is column_name op value
         * compare column against the value
    * op is one of <, <=, =, >, >=, <>
    * The terms are combined, e.g. (nameLast = ‘Ferguson’) AND (ab > 500)
        * and
    * CNF allows using a matching index for the access path.
* The engine can select a *matching index*
    * A Hash Index on (c1,c2,c3) if the condition is *c1=x AND c2=y AND c3=z*.
    * A Tree Index if the condition is (can be ordered as)
        * c1 op value
            * 'nameLast = ‘Ferguson’', in the set of people last name is Fergusion, then return whose ab>500. Even every rows are in a different blocks, fewer blocks are needed to read
        * c1 op x AND/OR c2 op y
        * … …
    * Many indexes may match, and the engine chooses the most **selective match**(number of tuples or blocks) that match.
        * the most selective index is the one with the fewest entries. 
    
Remember keys are smaller than the rows. So we can get many key values. Making the index tree two levels deep,  1:09：00
12. Query evalueation

    * Alternative ways of evaluating a given query
        * Equivalent expressions
        * Different algorithms for each operation
    * An example of pushing down the select ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_4.png)
    
13. SELECTion Pushing
* Consider two relations
    * StarsIn(title, year, star_name)
    * Movies(title, year, length, genre, studio_name, producer_no)
* Find everyone who starred in a comedy movie in 1996
    * Query 1: 
        * SELECT star_name,  FROM StarsIn JOIN Movies ON	StarsIn.title=Movies.title AND StarsIn.year=Movies.year	WHERE StarsIn.year=1996 and Movies.genre=‘Comedy’
            * Assume both tables have 1000 rows. Then the JOIN operation cost 1 000 000
    * Query 2:
        * SELECT star_name FROM (SELECT star_name, title, year FROM StarsIn WHERE year=1996 as a) JOIN
     (SELECT title, year, genre FROM Movies WHERE year=1996 AND genre=‘Comedy’)	ON a.year=b.year and a.title=b.title
            * Assume both tables have 1000 rows. Two select may be 30 and 20. Then the final result is 600. Table becomes smaller.
* SELECT on a JOIN of table R and table S compares O(#(R)*#(S)) tuples
    * Have to load relevant blocks
    * Compare the tuples to compute the JOIN
    * Scan the JOIN to apply the SELECT
* Pushing the SELECT(s) through the JOINs
    * May vastly **reduce the size of the table** to JOIN
    * Only JOIN tuples that could be selected in the WHERE clause.
    
You can also push project through JOIN, but this **reduces the size of the data**, not the number of tuples examined.
14. Join: 
> Default choice is Index Nested Loops
```
for each tuple r in R do
	for each tuple s in S where ri == sj  do
		add <r, s> to result
  
```
* For every row in R, and for every record in S, if equal then put in the record. If there is N rows in S, the second operation will run N times for each iteration. 
    * But in fact, when operating 'for each tuple s in S where ri == sj', we know the value for r, we can use that as an index. 
        * The reason to swap the table: we want the index to be in the inside to eliminate the need to scan.
        * If both them have clustering index, we want the clustered index to be inside. If the index is not cluster, the order of index does not guarantee the order of the files. If the index is clustered, when you hit the first record in the block, we can eliminate the block I/O.
    * If there is an index on the join column of one relation (say S), can make it the inner and exploit the index.
        * Cost:  M + ( (M*pR) * cost of finding matching S tuples) 
        * M=#pages of R, pR=# R tuples per page
    * For each R tuple, cost of probing S index is about 1.2 for hash index, 2-4 for B+ tree.  Cost of then finding S tuples (assuming Alt. (2) or (3) for data entries) depends on clustering.
        * Clustered index:  1 I/O (typical), unclustered: upto 1 I/O per matching S tuple.


15. Join: Sort-Merge ((R)⨝i=j(S))
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_5.jpg)
* Sort R and S on the join column, then scan them to do a ``merge’’ (on join col.), and output result tuples.
    * Advance scan of R until current R-tuple >= current S tuple, then advance scan of S until current S-tuple >= current R tuple; do this until current R tuple = current S tuple.
    * At this point, all R tuples with same value in Ri (current R group) and all S tuples with same value in Sj (current S group) match;  output <r, s> for all pairs of such tuples.
    * Then resume scanning R and S.
* R is scanned once; each S group is scanned once per matching R tuple.  (Multiple scans of an S group are likely to find needed pages in buffer.) 1:19:18 fig
    * Sort R based on the join staff. Then when doing join, we only need to go through the other table once. The sort merge is cheaper than the nexted loop.
16. Recall Selection Operation
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