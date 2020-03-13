
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

### Table of Contents

1. [Lecture1&2-24Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture1&2_Intro&Overview.md)
2. [Lecture3-31Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture3.md)
3. [Lecture4-7Feb](#my-second-title)
4. [Lecture5-14Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture5_ERModel_SQL.md)
5. [Lecture6-21Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture6_RelationalAlgebra.md)
6. [Lecture7-28Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lec7)


## 21 Feb 2020
1. SQL is a realization and extension of the formal model.
 - SQL is the relational model for all practical purposes
 - For all intents purposes, SQL is the relational model. It implements the capabilities of the relational model and it extends it. 
 - We can think SQL as a alternative notation for the formal model, and it has extensions
2. Why study the relational model?
 * Critical to how RDBs implement query processing and optimization
 * Many elements of SQL become clearer when you understand the underlying algebra
 * For relational DB engine, by giving the engine a SQL statement, we declare a result that we want, 'on this set of relations, please produce this relation'. Because it is a declarative and you are expressing an outcome(not the algorithm about how to do it), the query engine has to get significant flexibility to go and decide to actually look alternative ways to produce same result.
 * The relational algebra rules allow the query optimizer to convert the query you wrote into an equivalent query that you should have written and that is much more efficient.
 * An algebra allows us to determine when different statements are equivalent
 
3. **Relational Algebra**
 * A procedural language consisting of a set of perations that *take one or two relations as input and produce a new relation* as their output
 * six basic operators
   * select: σ
   * project: π
   * union: ∪
   * set difference: - 
   * cartesian product: ⨯
   * rename: ρ
 * Relation has tupples. Each tupple is a set of names and values. 
   * In SQL and RDB we call the relation as table, tupple as rows, attribute as columns.

4. join⨝ in relational algebra
 * A left join B = B left join A
5. Way of think about those **set diagram**
 - The cartesian product is all possible pairs of rows
 - The joins represent the subsets of the cartesian product

6. Prodedural v.s. non Procedural
 * Non procedural is also known as declarative: what output will be  without specifying how to do it

7. **Pure languages**, the following three pure languages are **equivalent in computing power**
> given a computation, no one is more powerful than others
 * Relational algebra
 * Tuple relational calculus
 * Domain relational calculus
 >
8. σ<sub>p</sub>, here p is called the selection predicate**
* select produces a subset of the rows, only contain rows which match the predicate, but it will **contain all the columns**

9. In the pure relation model, a relation is a set. You can't have two tuples that have the same value for all properties 
* In relational model we will never see a tuple twice. Therefore doing a project it will not duplicate the original tables but if you project out some of the columns and differentiate it, you will have duplicates, like project out the uni. 
* In relational model, because relation is a set, the duplicate will go away, which is exactly the opposite of what happens in SQL, SQL never removes the duplicate unless you put the key word ‘distinct’ in. So just note that Different schemes for the relational model and SQL exist.

10.  **Duplicate rows removed from result, since relations are sets**
 * According to Prof. Ferguson, **SQL does not behave this way**

11. The **result** of a relational-algebra operation is **relation**, so you can apply relational operator to the result of relational operator.


## 5 Mar 2020

1. Difference between **natural join**, **inner join** and **join** in **SQL**
  * Inner join will output duplicate columns 
  * inner join operates based on the 'on' clause
  * natural join automaticly join two relations using columns with same name and datatype, no duplicate columns
  
 
2. When import the script from the book [Database System Concepts](https://www.db-book.com/db7/university-lab-dir/sample_tables-dir/index.html), need to **SET SQL_SAFE_UPDATES = 0** otherwise there will be an empty table
  * According to Eric Minner's answer on Piazza
    > Go to Edit --> Preferences

    > Click "SQL Editor" tab and uncheck "Safe Updates" check box

    > Query --> Reconnect to Server // logout and then login

    > Now execute your SQL query
    
3. **A Cartesian product is not a JOIN in relational algebra.**

4. In homework4, Prof. Ferguson asks us to find *Classrooms that do not have a section*. 
    * Note here using natural join, section ⨝ classroom, it will utilize building and room_number to join the result.
    * To find out classroom that does not havs a section, we need to use classroom-section ⨝ classroom, but here the dimension is not correct.
    ```
    section ⨝ classroom
    ```
    is the same as
    ```
    section ⨝section.building=classroom.building∧section.room_number = classroom.room_number classroom
    ```
    * For the classroom has a section, the detailed information can be extracted using
    ```
    π building, room_number (section ⨝ classroom)
    ```
    * 
 5. Group by in relational algebra in **RelaX**
    * For example, group by section using the building, and show the minimum room_number of each group
    ```
    γ building; min_num <-min(room_number) (section)
    ```
  
6. A null-value can be written as null or NULL (without single quotes) in **RelaX**

7. We can only manipulate columns using a project, like π double<-2*s
```
π double<-2*s, time_slot_id (γ time_slot_id; s<-sum(start_hr) (time_slot))
```
 * Project is the operation that makes new columns
 * Group by operates on rows. Can not be done at the same time as project
 
8. full outer join
 * Everything in table 1 will have 
 
9. Clarification of natural join and inner join by Prof.Ferguson 
 * **A natural join is an inner join in which the values for common column names match. If both tables have a column ID, the condition is on r.ID=l.ID.**
 * **An equijoin in an inner join in which the comparison operator is '=', e.g. on r.ID=l.UNI.**
 * **A join (theta join) uses an arbitrary predicate.**


## 6 Mar 2020
1. MAKETIME(hour,minute,second) in **mySQL**
Returns a time value calculated from the hour, minute, and second arguments.

```
mysql> SELECT MAKETIME(12,15,30);
        -> '12:15:30'
```
2. Add a column to existing table
```
ALTER TABLE vendors
ADD COLUMN phone VARCHAR(15) AFTER name;
```

3. Add a sequential value to a column
```
SELECT @i:=0;
UPDATE yourTable SET yourField = @i:=@i+1;
```
 * Note that here the column must exist 
4. case condition in **sql**
```
select time_slot_id1,time_slot_id2, day_of_week_overlap,
    id_value, 
    CASE WHEN start_time1 >= start_time2 THEN start_time1 ELSE start_time2 END AS s_time,
    CASE WHEN end_time1 <= end_time2 THEN end_time1 ELSE end_time2 END AS e_time
    from overlap_time
```
5. When left join, using **on** instead of where for predicate
```
%%sql 
select * from s1_q3_1 as a
left join 
time_slot_fixed as b 
on a.time_slot_id1 = b.time_slot_id
```
``    
