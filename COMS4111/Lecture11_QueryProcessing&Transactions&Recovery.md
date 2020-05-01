

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
    * **distinct key values (NKeys)** and NPages for each index.
    * **Index height, low/high key values (Low/High)** for each tree index.
* Catalogs updated periodically.
* Updating whenever data changes is too expensive; lots of approximation anyway, so slight inconsistency ok.
* More detailed information (e.g., histograms of the values in some field) are sometimes stored
