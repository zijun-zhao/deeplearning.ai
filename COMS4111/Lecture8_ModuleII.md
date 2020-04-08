

### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

### Table of Contents

1. [Lecture1&2-24Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture1&2_Intro&Overview.md)
2. [Lecture3-31Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture3.md)
3. [Lecture4-7Feb](#my-second-title)
4. [Lecture5-14Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture5_ERModel_SQL.md)
5. [Lecture6-21Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture6_RelationalAlgebra.md)
6. [Lecture7-28Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture7_Wrap_up.md)
7. [Lecture7-6Mar](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture8_ModuleII.md)



* Course Modules 
  * A reminder because we are transition from I to II. 
  * Miscellaneous/wrappingupModule I topics: 
      * ER/Datadesignissues. 
      * AccessingSQL from programs. 
          * Basicconcepts. 
          * ORM. 
          * Connections,cursors,transactions. 
  
## 6 Mar 2020

1. The course logically has four modules/sections: 
* 1. Foundational concepts: This module covers concepts like data models, relational model, relational databases and applications, schema, normalization, ... The section focuses on the relational model and relational databases. The concepts are critical or all types of databases and data centric applications. 
    * SQL databases are still the predominary form of databases 
* 2. Database management system architecture and implementation: This module covers the software architecture, algorithms and implementation techniques that allow databases management systems to deliver functions. Topics include memory hierarchy, storage systems, caching/buffer pools, indexes, query processing, query optimization, transaction processing, isolation and concurrency control. 
* 3. NoSQL --"Not Only SQL" databases: This section provides motivation for "NoSQL" data models and databases, andcovers examples and use cases. The section also includes cloud databases and databases-asa-service. 
* 4. Data Enabled Decision Support: This section covers normalization/denormalization, data warehouses, import and cleanse, OLAP, Pivot Tables, Star Schema, reporting and visualization, and provides and overview of analysis techniques, e.g. clustering, classification, analysis, mining. 

We are transitioning from Module I to Module II.

3.E-R Design Decisions
* The use of an attribute or entity set to represent an object. 
* Whether a real-world concept is best expressed by an entity set or a relationship set.
* The use of a ternary relationship versus a pair of binary relationships. 
  * how to decompose a ternary relationship into bunch of binary relationships
* The use of a strong or weak entity set.
* The use of specialization/generalization –contributes to modularity in the design. 
* The use of aggregation 
  * can treat the aggregate entity set as a single unit without concern for the details of its internal structure.

4. Erroneous use of relationship attributes
    * In the following example, this is an example of incorrect use of attributes. If model "stud_dept" as a relationship, then there should not be an attribute "dept_name" in student. Since student **is related to** department.
      * That is the diffence between logical model and a more detailed physical model. When go to relational, the concept "stud_dept" does not exist. Then you need to show it in terms of table, use FK etc.

![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/Erroneous%20use%20of%20relationship%20attributes.png)

5. In database design, if it is possible, you do not want more than two paths between two entities. Otherwise you need to think about updating.

6. Normally, people will have multiple phone number. Also, phone number can be compound. The phone number's country code may related to the country code. There fore we prefer the right choice.

![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/phonenumber.jpg)

7. Let’s walk through this one from start to finish. 
![mAR6eXAMPLE](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/ER_EXAMPLE6mAR.jpeg)
    * Do we add label in InstructorPhone as PK?
      * Prof. Ferguson would, because sometimes the home phone and the work phone are the same. By using the label, the relationship is unique. It can be A's work phone and B's home phone. CountryCodes has a **fixed set of value**. You have choices. Here he assumed that same number can be both home phone and work phone.
    ![mAR6eXAMPLE2](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/ER_EXAMPLE6mAR2.jpeg)
    * What if one phone number is mandatory like the graph above?? There is no statement that we could insert to two tables. If trying to insert a phone number we will fail since we do not have the instructor. Similarly, when inserting the instructor, we will fail since we do not have the number. This is **not an uncommon issue for a foreign key relationship to be mandaroty on both ends of the relationships**. The answer is: **the concept of transcactions**.
      * There is a way to think the two insert statements as a single unit of work, and some databases allow you to do defer referential integrity constraints until the transcation commence. In that case, I can do two inserts, two updates and then commit, this will work.
      * But some databases do not support this way.
      * We will learn that in module II.
    * In the example below, by introducing a unique constraint, Prof. Ferguson assumed that the combination of instructor_id and label is unique, then a person can not have the same value for two different labels. So you cannot two home phone numbers.
    ![mAR6eXAMPLE3](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/ER_EXAMPLE6mAR3.jpeg)
    * The logical model cannot be implemented. There will always be information loss. By plotting the ER diagram in Mysql workbench you can actually generate the schema


8. In lucichart by using export, we will be able to generate the sql code:

Lucichart version            |  After modification
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/365506.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/365594.jpg)

  * Then we will have the table
  * Then modify the key constraints further for **instructor**

Original Table            |  Alteration
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/instructor661303.jpg)  | ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/instructor603284.jpg)

    ```mysql
    ALTER TABLE `w411example2`.`instructor` 
    CHANGE COLUMN `name` `name` VARCHAR(128) NOT NULL ,
    CHANGE COLUMN `title` `title` ENUM('idiot', 'super idior') NOT NULL ;
    ```
  * Then modify the key constraints further for **CountryCodes**
    ```mysql
    ALTER TABLE `w411example2`.`countrycodes` 
    CHANGE COLUMN `name` `name` VARCHAR(128) NOT NULL ;
    ```
  * Then modify the key constraints further for **Phone**
    ```mysql
    ALTER TABLE `w411example2`.`phone` 
    CHANGE COLUMN `CC` `CC` CHAR(4) NOT NULL ,
    DROP PRIMARY KEY,
    ADD PRIMARY KEY (`number`, `CC`);
    ```

  * Note that Phone will have a FK **p_to_cc**. It will reference country code table, and the column is code
  
Table Logistics           |  Modulation detail
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk1.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk2.png)

    ```mysql
    ALTER TABLE `w411example2`.`phone` 
    ADD CONSTRAINT `p_to_cc`
      FOREIGN KEY (`CC`)
      REFERENCES `w411example2`.`countrycodes` (`code`)
      ON DELETE NO ACTION
      ON UPDATE NO ACTION;
      ```
  * Then look at the instructorPhone relationship. 
    * By setting instructor_id to be unique, we are actually assuming there will be only one entry for any instructor. If check the label to be unique, only one person could have a home number. This is not our assumption.
    * Therefore need to go to **Indexes** and set the constraint.
      * In this way, the instructor can only have one home phone number and one work phone number. No matter what is the number, you will only see "dff9, home" once.
      * Then how about the PK for this table. We only care about the constraint that a person only has one type of phone numbers. Maybe we will just set an auto_id, which is auto implemented.
    
Indexes&FK        |  Columns
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk4.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk5.jpg)

    ```mysql
    ALTER TABLE `w411example2`.`instructorphone` 
    ADD COLUMN `auto_id` INT NOT NULL AUTO_INCREMENT AFTER `label`,
    CHANGE COLUMN `instructor_ID` `instructor_ID` VARCHAR(12) NOT NULL ,
    CHANGE COLUMN `CC` `CC` CHAR(4) NOT NULL ,
    CHANGE COLUMN `number` `number` VARCHAR(64) NOT NULL ,
    CHANGE COLUMN `label` `label` VARCHAR(32) NOT NULL ,
    ADD UNIQUE INDEX `uq_label` (`instructor_ID` ASC, `label` ASC) INVISIBLE,
    ADD INDEX `p_to_p_idx` (`number` ASC, `CC` ASC) VISIBLE,
    ADD PRIMARY KEY (`auto_id`);
    ;
    ALTER TABLE `w411example2`.`instructorphone` 
    ADD CONSTRAINT `p_to_i`
      FOREIGN KEY (`instructor_ID`)
      REFERENCES `w411example2`.`instructor` (`ID`)
      ON DELETE NO ACTION
      ON UPDATE NO ACTION,
    ADD CONSTRAINT `p_to_p`
      FOREIGN KEY (`number` , `CC`)
      REFERENCES `w411example2`.`phone` (`number` , `CC`)
      ON DELETE NO ACTION
      ON UPDATE NO ACTION;
    ```
* By using database->reverse engineer we can generate the **physical model**

Mandotory       |  Non Mandatory
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk6.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk7.jpg)

* Similar operation is done to the p_to_p
* Note the solid and dash line here.
    * Difference here is that **Phone** is a weak entity, we cannot form the PK without the key from countrycodes. The solid line is a **identifying relationship** 
    ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/fk8.png)



* According to the diagram, phone can exist not have a instructor, instructor can also exist if he does not has a phone number. **instructorPhone** is a relationship, and there will be an entry if it connects the phone and the instructor. That is the purpose of that table.



9. An ENUM is a type of attribute in a table. They may have one of a fixed set of values. Those value are character strings. ENUM is a string object with a value chosen from a list of permitted values that are enumerated explicitly in the column specification at table creation time. For example:
```mysql
CREATE TABLE shirts (
    name VARCHAR(40),
    size ENUM('x-small', 'small', 'medium', 'large', 'x-large')
)
```

10. Difference between varchar(12) and char(4)?
    * varchar is a variable length string, it may have length 0 to 12, any combination of character that has length 0 to 12. While car(4) means that it has to have exactly 4 characters, which is a fixed length string.

11. Accesing SQL from Programming Languages
* Question is are you writing the code
* Or are you using the application written by others.

12. A typical structure of a web application
> Figure from Prof. Ferguson
         ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/typical.jpg)
* Interact using the browser
* HTML, images, scripts coming from the block database. A webpage comes down from the Block DB and then use rows from the structured DB to show in the browser.

13. **SQL injection attack** happens when you thinking you are doing 
```mysql
select * from accounts where acct_no = " " +account_no +""
```
But you are actually doing 
```mysql
select * from accounts where acct_no = " xy-0101 or 1 =1"
```
![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/typical2.jpg)
* **SQL injection** is one of the most common security penetration
* The solution is about programming and it is the primary vulnerability that happens because things are in database.

14. In some compiled programing languages, you can have SQL STATEMENT inside the programming.

15. Object-Relational Mapping
1:19:54

16. **The data base description is represented at the logical level in the same way as ordinary data, so that authorized users can apply the same relational language to its interrogation as they apply to the regular data.** In relational database, all the metadata is itself relational data.
```mysql
select * from information_schema.tables
```
* Onle select the tables in schema `schema_name`
```mysql
select* from information_schema where table_schema = `schema_name`
```
* Show all the columns in schema `schema_name`
  * The ORDINAL_POSITION is the order you define it
```mysql
select* from information_schema.columns where table_schema = `schema_name`
```
* Onle select the tables in schema `schema_name`
```mysql
select * from information_schema.referential_constraints where constraint_schema = `schema_name`
```
* The data about data is itself a data. In information_schema.tables, there is an entry for the table information_schema.tables. So the definition of table is in the table.



## 8 Mar 2020
1. In order not to modify the original data, we can use 
```mysql
create table orders_midterm as select* from orders
```
