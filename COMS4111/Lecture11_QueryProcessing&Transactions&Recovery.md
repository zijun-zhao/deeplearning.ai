

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
```sql
SELECT star_name,  FROM StarsIn JOIN Movies ON
StarsIn.title=Movies.title AND StarsIn.year=Movies.year
WHERE StarsIn.year=1996 and Movies.genre='Comedy'
```
* Assume both tables have 1000 rows. Then the JOIN operation cost 1 000 000
    * Query 2:
```sql
SELECT star_name FROM (SELECT star_name, title, year FROM StarsIn WHERE year=1996 as a) 
JOIN
(SELECT title, year, genre FROM Movies WHERE year=1996 AND genre=‘Comedy’)	
ON a.year=b.year and a.title=b.title
```
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
16. Several different algorithms to implement joins
* Nested-loop join
* Block nested-loop join
* Indexed nested-loop join
* Merge-join
* Hash-join
17. Nested-loop join
	* To compute the theta join        r ⨝θs
```sql
for each tuple tr in r do begin
	for each tuple ts  in s do begin
		test pair (tr,ts) to see if they satisfy the join condition θ
			if they do, add tr·ts to the result.
		end
	end
```
* The default join
	* r  is called the **outer relation** and s the **inner relation** of the join.
	* Requires no indices and can be used with any kind of join condition.
	* Expensive since it examines every pair of tuples in the two relations. 
17. Block nested-loop join basicly operates on the block level, the idea is that we go through the block therefore we will not read block over and over again. Each block will only be read once,
 	* Variant of nested-loop join in which every block of inner relation is paired with every block of outer relation.
```sql
for each block Br of r do begin
	for each block Bs of s do begin
		for each tuple tr in Br  do begin
			for each tuple ts in Bs do begin
				Check if (tr,ts) satisfy the join condition
					if they do, add tr 
					ts to the result.
			end
		end
	end
end
```

18. Indexed nested-loop join
	* Index lookups can replace file scans if
		* join is an equi-join or natural join and
		*an index is available on the inner relation’s join attribute
			* Can construct an index just to compute a join.
> Sometimes even there are not index on the table, the database will build an index. If the index is ordered, then it is similar to a sort. However, if the only comparison operator is the equality, we can build a hash table.

19. Hash-Join![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_6.jpg)
	* Applicable for equi-joins and natural joins.
	* A hash function h is used to partition tuples of both relations 
	* When we hash a table, for a given hash value, all the rows and columns hashed to the value are in the same bucket. By only doing **equality**, we can sort a table
		* If the bucket is too large, we can load it in memory.
20. Complex Joins
	* Join with a conjunctive condition: r⨝θ<sub>1</sub>∧θ<sub>2</sub>∧...θ<sub>n</sub>S
		* Either use nested loops/block nested loops, or
		* Compute the result of one of the simpler joins r ⨝θ<sub>i</sub>s
			* final result comprises those tuples in the intermediate result that satisfy the remaining conditions
				* θ<sub>1</sub>∧θ<sub>2</sub>∧...θ<sub>n</sub>

21. Materialization
	* Materialized evaluation: evaluate one operation at a time, starting at the lowest-level.  Use intermediate results materialized into temporary relations to evaluate next-level operations.
	* Example of compute and store σbuilding="Warson"(department) ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_7.jpg)
	* Two ways to run this query

Method_1|Method_2
---|---
Run σbuilding="Warson"(department) completely and obtain a table| Run the select, every time a tuple comes out then do the join

* Using **pipeline**, we can run the join the same time as running the select
	* Pipelined evaluation:  evaluate several operations simultaneously, passing the results of one operation on to the next.
	E.g., in previous expression tree, don’t store result of σbuilding="Warson"(department)
		* instead, pass tuples directly to the join..  Similarly, don’t store result of join, pass tuples directly to projection. 
	* Much cheaper than materialization: no need to store a temporary relation to disk.
	* Pipelining may not always be possible – e.g., sort, hash-join. 
	* For pipelining to be effective, use evaluation algorithms that generate output tuples even as tuples are received for inputs to the operation. 
	* Pipelines can be executed in two ways:  demand driven and producer driven 
	
	
22. Core Transaction Concept is ACID Properties
	* Atomic: Transaction cannot be subdivided.
		* if there are several transaction, either all of them happen, or none of them happen
	* Consistent
		* transforms database from one consistent state to another consistent state
	* Isolated: Transactions execute independently of one another
		* Database changes not revealed to users until after transaction has completed
	* Durable: Database changes are permanent
		* The permanence of the database’s consistent state

23. Atomicity
	* A transaction is a logical unit of work that must be either **entirely** completed or **entirely** undone. (All writes happen or none of them happen).
		* In the following example, the two update must both be completed, or not be completed.
```sql
def transfer(source_acct_id, target_acct_id, amount)
	Check that both accounts exist.
	IF is_checking_account(source_acct_id)
		Check that (source_acct.balance-amount) > source_account.overdraft_limit
	ELSE
		Check that (source_count.balance-amount) >source_account.minimum_balance
	Update source account
	Update target account.
	INSERT a record into transfer tracking table.
```

24. Simplistic Approach

* sql has an auto commit function, by using the following verbs we can decide whether to commit or not  ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_8.jpg)
	* start transction
	* commit
	* roll back
* Define a variable that exists while connected. Once disconnected the variable will go away
```sql
select balance into @a_balance from banking_account where id = 1;
select @a_balance;

select balance into @b_balance from banking_account where id = 2;
select @b_balance;
```
* Suppose a 50 dollars transcaction
```sql
update banking_account set balance = @a_balance-50 where id=1;
update banking_account set balance = @a_balance+50 where id=2;
```
* This will fail at the second update. We can see the change of id = 1 on the engine we run the query, but we are not able to see the change on another engine
* Then, using **roll back**, run the following query
```sql
start transction;
select balance into @a_balance from banking_account where id = 1;
select @a_balance;

select balance into @b_balance from banking_account where id = 2;
select @b_balance;

update banking_account set balance = @a_balance-50 where id=1;
update banking_account set balance = @a_balance+50 where id=2;

commit;
```
* This modification can be seen by others.

25. There are several problems with the simplistic approach.
	* The approach does not solve the problem
		* Some write might succeed.
		* Some might be interrupted by the failure, or require retry.
	* Writes may be random and scattered. N updates might
			* Change a few bytes in N data frames
			* A few bytes in M index frames
		* Transaction rate becomes bottlenecked by write I/O rate, even though a relative small number of bytes change/transaction.
	* Written frames must be held in memory.
		* Lots of transactions
		* Randomly writing small pieces of lots of frames.
		* Consumes lots of memory with pinned pages.
	* Degrades the performance and optimization of the buffer. 
		* The optimal buffer replacement policy wants to hold frames that will be reused.
		* Not frames that have been touched and never reused.
		
* Implementation Subsystems ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_9.jpg)
	* Query processor schedules and executes queries.
		* Return to user once the transaction is done
	* Buffer manager controls reading/writing blocks/frames to/from disk.
	* Log manager journals/records events to disk (**log**), e.g.
		* Transaction start/commit/abort
		* Transaction update of a block and previous value of data.
			* New value of the bytes will be used. An update may touch one 64 key block, then log record will keep the block number, the offset, the old data and the new data(like 20 bytes).
	* Transaction manager coordinates query scheduling, buffer read/write and logging to ensure ACID.
	* Recovery manager processes log to ensure ACID even after transaction or system **failures**.
	* Log is a special type of block file used by the system to optimize performance and ensure ACID.
* Writing is slow, so why does writing log file help?
> When we do something like an update, the data goes into the memory, but the update also goes in to the log stream. While each block is 64k, the only thing we need in the log file is the block id, offset and length, therefore we can run lots of transaction at the same time which all write to the log stream. When a log pages fail of sb does a commit, we can force the log out without forcing the frame.

* Redo Processing  ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_10.jpg)
* Write log events from all transactions into a single log stream.
* Multiple events per page
* Forces (writes) log record on COMMIT/ABORT
	* Single block I/O records many updates
	* Versus multiple block I/Os, each recording a single change.
	* All of a transaction’s updates recorded in one I/O versus many.
* If there is a failure
	* DBMS sequentially reads log.
	* Applies changes to modified pages that were not saved to disk.
	* Then resumes normal processing.
	> just go through to find all transaction committed without writing to the disk.
