

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


  * ER patterns and Realization
      * Complex/Compound Attributes; Derived Attributes
      * Weak Entities
      * Ternary(and more Complex)Relationships
      * Aggregation
      * Miscellaneous Design Issues
   * Advanced SQL
    * Recursion
    * View and Security
  
## 28 Feb 2020
1. Difference betwen union and join
* Given two tables, color and fruits

Color
    
| ID  | v |
| ------------- | ------------- |
|1  | R  |
| 2  | G |
| 3 | B |

Fruits
    
| ID  | v |
| ------------- | ------------- |
|1  | T  |
| 2  | B |
| 3 | O |
| 4 | K |

By union operation, the resulted table will have **same number of columns**, the default setting of union won't have duplicates. If you want duplicate, then need to **union all**.
  * **#(Fruits∪Colors)≤#(Fruits)+#(Colors)**

| ID  | v |
| ------------- | ------------- |
|1  | R  |
| 2  | G |
| 3 | B |
|1  | T  |
| 2  | B |
| 3 | O |
| 4 | K |


By natural join, it will use the column in common:id to combine the two
  * **#(colors⨝fruit)≤#(Fruits)\*#(Colors)**
      * The equal happens when cross join: match every possible pair
      
| ID  | v | v |
| ------------- | ------------- | ------------- |
|1  | R  |T  |
| 2  | G | B |
| 3 | B | O |

2. When do SQL, it generates query to do the efficient convertion. Parse the SQL statement-> produce the parse tree, then provides all equivalent statement and then pick the most efficient one. 

* ***Most important law in relational algebra***:
  * σp(R⨝L)=σp(R)⨝σp(L)
  * After σ, what is the size of the table that comes out: selectivity of the predicate. 
 
 3. In table
 * Attributes are columns, in an entity it is the name of the attributes
 * Entity is a set of attributes and values.
 * Entity set is a set of entiteis.
 
4. Attribute types:
> Entities are represented by means of thier properties, called attributes. All attributes have values.

* Simple attribute
* Composite attribute
  * Composite attribute: for instance, name: Tom Hanks
  * It allows us to divide attributes into **subparts**(other attributes)
   * For example, name can be divided in to first_name, middle_name, last_name
* Single-valued attribute
* Multivalued attributes
  * multivaled attributes: phone numbers, email addresses(**more than one**)
* Derived attributes
  * Can be computed from other attributes
  * For instance, age, give date_of_birth
  
5. Domain: the set of permitted values for each attribute, for instance, varchar.
 * Domain is **atomic** if its elements are considered to be indivisible units.
 * Examples of **non-atomic** domains
    * Set of names
    * Identification numbers like CS101 can be broken into parts. Its composite can be divided into department and class number.

6. A relational schema R is the **first normal form** if the domains of all attributes of R are atomic.

7. **What makes something a well-defined DB schema**
* According to Prof. Ferguson:
  > **Relational database normalization** is the process of structuring a **relational database** in accordance with a serires of so-called **normal form** in order to reduce **data redundancy** and improve **data integrity**.

  > Normalization is formal, methodology for think about what makes a "good relational schema"
  
8. Two of the most significant issues and problems that **not applying first normal form** can cause:
    * Difficulty implementing integrity constraints
    * Poor performance due to inability to use indexes.
  
  
  
  
  
  
9. 

nf           |  Non normal form
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_1NF1.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_1NF2.jpg)

* Consider a course number like "COMSW4111"
    * Strings/VARCHAR is usually an atomic domain. 
    * But is not in this case. By setting it to be a string, we cannot use foreign key constraint to restrict the first four characers
* Representing the course number as a string 
    * Precludes integrity constrains on the department code, faculty code and level. 
    * Precludes adding an index on elements of the number like ‘4111.’ 
    * We will see why in the next module. 
* If we want to treat the composite attribute as a string, we can
    * Generate the column. 
    * If it is a stored, generate column, we can also index it.
    ```sql
    CREATE TABLE `course_nf`(
    `dept_code` char(4) NOT NULL,
    `level` enum('1','2','3','4','6','9') NOT NULL,
    `number` int NOT NULL,
    `title` varchar(32) NOT NULL,
    `faculty_code` enum('W','B','E','G') NOT NULL,
    `credits` int NOT NULL,
    `course_no` varchar(16) GENERATED ALWAYS AS (concat(`dept_code`,`faculty_code`,`level`,`number`)) STORED,
    PRIMARY KEY(`dept_code`,`faculty_code`,`level`,`number`),
    KEY `course_no1` (`level`,`number`),
    KEY `full_no`(`course_no`),
    CONSTRAINT `c_to_d` FOREIGN KEY (`dept_code`) REFERENCES `department_nf`(`dept_code`),
    CONSTRAINT `course_nf_chk_1` CHECK((((`level` = _utf8mb4'4') and (`credits`=4)) or  
    ((`credits`<>_utf8mb4'4') and (`credits`>0) and (`credits`<=3))))
    ) ENGINE=InnoDB DEFAULT CHARSET=utfm64 COLLATE=utf8mb4_0900_ai_ci;
    ```
    
* **Stored** means remember the formula, but when saving the file the computed data is also saved. We only use it to update.


10. How ***Not*** to Model in Relational

A wrong example:

```sql
CREATE TABLE `W4111Examples`.`bad_table` ( 
`id` VARCHAR(32) NULL, 
`name` VARCHAR(256) NULL, 
`address1` VARCHAR(256) NULL, 
`address2` VARCHAR(256) NULL, 
`emails` VARCHAR(256) NULL, 
`phone_work` VARCHAR(45) NULL, 
`phone_other` VARCHAR(45) NULL);
```

Approaches like this create several problems: 
    * Query on things like last_nameor country.
    * Integrity: emails are unique 
    * Insight:
        * Finding people in the same zip code. 
        * Which people have business emails? 
    * Difficult to index for efficiency.
We will study database models that support and recommend “document” or “object” like approaches.

11. Relational Databases have built in functions for many types：link[https://www.mysqltutorial.org/mysql-functions.aspx]. List is complete.

12. Compound attributes can be multiple columns in an entity, it can also be a separate table/entity with multiple columns.

13. In a **relational database**, a **weak entity** is an entity that cannot be uniquely identified by its attributes alone; therefore, it must use a **foreign key** in conjuction with its attributes to create a **primary key**. The foreign key is typically a primary key of an entity it is related to.
  * For instance, a course requires an instructor, but instructor is not in the course.

14. A **weak entity set** is one whose existence is dependent on another entity, called its **identifying entity**. Instead of associating a primary key with a weak entity, we use the identifying entity, along with extra attributes called **discriminator** to uniquely identifying a weak entity.

* Every weak entity must be associated with an identifying entity. The weak entity set is said to be **existence dependent** on the identifying entity set.
  * The identifying entity set is said to **own** the weak entity set that it identifies.
  * The relationship associating the weak entity set with the identifying entity set is called the **identifying relationship**.

15. We cannot use double rectangle to express weak entity set in ER diagram.
![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/phonenumber.jpg)


16. Dashed lines in ER diagram refers to an **identifying relationship**. A dashed line means that the relationship is strong, whereas a solid line means that the relationship is weak. 

17. For ER diagram with a **Ternary Relationship**, in SQL/RDB, this requires four tables(relations):
 * Intructor
 * Student
 * Project
 * Project Participants

18. Entity sets of a relationship need not be distinct.
 * Each occurrence of an entity set plays a "**role**" in the relationship.
 * According to Prof. Ferguson, **The concept of roles applies to all relationships/associations**.
 
19. If a key(combination of columns) is unique, and non of the values is null. Then it is a candidate key.

```sql P0P'[P=-1 Q
CREATE TABLE `W4111Examples`.`Person` (
  `uni` VARCHAR(16) NOT NULL,
  `auto_id` INT NOT NULL AUTO_INCREMENT,
  `last_name` VARCHAR(64) NOT NULL,
  `first_name` VARCHAR(64) NOT NULL,
  `middle_initial` CHAR(1) NOT NULL,
  `DOB` DATE NOT NULL,
  UNIQUE INDEX `uni_UNIQUE` (`uni` ASC) VISIBLE,
  PRIMARY KEY (`auto_id`),
  INDEX `name_idx` (`last_name` ASC, `first_name` ASC, `middle_initial` ASC) VISIBLE);
```
In this example, the benifit of having a auto increment column. When we insert a row, the uni is generated. The question is how do I know what is the exact uni. Therefore in most database system it asks for the last inserted row's auto_id.

* Notes
     * uni and auto_id are candidate keys. Prof. Ferguson chose auto_id for the primary key, and we will see why later in the lecture.

    * The index on last_name, first_name may significantly improve performance for SQL operations.
        * The index is an index on both last_name alone, and last_name and first_name together. **ORDER MATTERS**
        * It is not NN, therefore duplicates may exist.
        * It is not an index on first_name alone.
        * Thus, it helps with
            * where last_name='Ferguson'
            * where last_name='Ferguson' and first_name='Donald'
        * But does not help with,
            * where first_name='Donald'
            * where last_name='Ferguson' OR first_name='Donald'
            * We will understand in the next module.

20. Note that `DOB` is DATE type. If we use string type, **it will sort that date as if they were strings, like names**.

```sql
CREATE TABLE `W4111Examples`.`test_date_time` (
  `date_string` VARCHAR(64) NULL,
  `time_string` VARCHAR(64) NULL,
  `date_type` DATE NULL,
  `time_stamp_type` TIMESTAMP NULL);
  
insert into test_date_time(date_string)
    values
        ('January 1st, 1960'),
        ('April 1st, 2019');
```
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_2.jpg)

* Luckily, most languages have functions and libraries to handle string/date and time conversion.

```sql
select date_string, str_to_date(date_string, "%M %D, %Y") as parsed_string from
    test_date_time;
```
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_3.jpg)

* Further, we can update the schema further
```sql
update test_date_time
    set date_type = str_to_date(date_string, "%M %D, %Y")
```
Before Update          |  After Update
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_4.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_5.jpg)


