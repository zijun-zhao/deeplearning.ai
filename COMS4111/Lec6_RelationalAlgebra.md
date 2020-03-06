
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
