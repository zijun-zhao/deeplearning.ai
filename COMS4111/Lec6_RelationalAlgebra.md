
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

### Table of Contents

1. [Lecture1&2-24Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture1&2_Intro&Overview.md)
2. [Lecture3-31Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture3.md)
3. [Lecture4-7Feb](#my-second-title)
4. [Lecture5-14Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture5_ERModel_SQL.md)
5. [Lecture6-21Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lec6_RelationalAlgebra.md)
6. [Lecture6-28Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lec7)


## 21 Feb 2020


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

```mysql
mysql> SELECT MAKETIME(12,15,30);
        -> '12:15:30'
```
