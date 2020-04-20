

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

15. We cannot use double rectangle to express weak entity set using crow's foot
![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/phonenumber.jpg)


16. Dashed lines in ER diagram refers to an **identifying relationship**. A dashed line means that the relationship is strong, whereas a solid line means that the relationship is weak. 

17. For ER diagram with a **Ternary Relationship**, in SQL/RDB, this requires four tables(relations):
>![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_14.jpg)
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
Before Update          |  After Update|  New Format
:-------------------------:|:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_4.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_5.jpg) |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_6.jpg)

* Use date_format, we can further adjust the display
```sql
select date_type,
    date_format(date_type, "(%a) %b-%d-%y") as formated_date
from
    test_date_time;
```

21. Data Formatting and Parsing
> ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_7.jpg) 

22. Treating the value as a DATE type enables functions on the domain:
    * Ordering
    * BETWEEN
    * Operators
```sql
select
    *,
    dayofweek(date_type) as day_of_week,
    dayofmonth(date_type) as day_of_month,
    dayofyear(date_type) as day_of_year
from
    test_date_time;
```
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_8.jpg) 


23. Age can be computed using **timestampdiff**
```sql
create view personv as select
	*,
    timestampdiff(year, dob, now()) as age
    from
    Person;
```

24. Date can be updated in **view**(here we do not have the authority to insert through a view)

Table **person**          |  View **personv**
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_9.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_10.jpg) 

```sql
update personv set dob='1934-06-09' where uni='dd2'
```
Updated View **personv**       |  Updated Table **person** 
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_11.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_12.jpg) 

25. User-Defined Types

* We can create a type, for example

```sql
create type Dollars as numeric(12,2) final
```

In that case, a table can be created as 
```sql
create table department 
(dept_name varchar(20), 
building varchar(15), 
budget Dollars);

```

However, according Prof. Ferguson, differnet databases the support is not consistent. Not all databases support that.
> Not sure how common using UDT is. I almost never see it. 
> Support across various RDBMS products is inconsistent.

26. User-defined Domains
```sql
create domain person_namechar(20) not null 
```

* Types and domains are similar. **Domains can have constraints, such as not null, specified on them**.
* Example: 
```sql
create domain degree_levelvarchar(10) 
	constraint degree_level_test 
		check
	(value in ('Bachelors', 'Masters', 'Doctorate')); 
```

> Prof. Ferguson is not sure how common this is or how consistent the support is.

27. An Example
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/ER_EXAMPLE_28fEB.jpeg)


	* Note hear, a person has one or more emails. You cannot have an email that does not reference back to a person. And you cannot insert a person if the email is not there.
	* Note here the address is also separated into three parts.

28.  Example of Classic models Schema: inventory of a store

A subset of the model is shown below:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/classic28fEB.jpeg)

* Whyisoneofthelines“dashed?” 
	* MySQLreferstothisasan “identifying relationship.” 
	* orderdetailscannot form a primary key without a field from orders. 
	* It is a Weak Entity 
	* orderdetailsprimary key contains a column of orders primary key. 
* An order "owns" or "contains" itsorderdetails.
29. 
* In this example, a relationship table is used as project_guide. There are two reasons when using a relationship table:
	* The property is many to many
	* The relationship has properties that are independent of the entiteis
	
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/classic28fEB_2.jpeg)


* This example satisfies both reasons.

30. Aggregation example

Wrong ER diagram         |  Improved version
:-------------------------:|:-------------------------:
![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_15.jpg)  |  ![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_16.jpg) 

* For the **left** diagram, the eval_for cannot reference any project, student, instructor.It must reference 3 reference by a proj_guide.
	* Relationship sets eval_forand proj_guiderepresent overlapping information
		* Every eval_forrelationship corresponds to a proj_guiderelationship 
		* However, some proj_guiderelationships may not correspond to any eval_forrelationships § So we can’t discard the proj_guiderelationship
	* Eliminate this redundancy via aggregation 
		* Treat relationship as an abstract entity 
		* Allows relationships between relationships
		* Abstraction of relationship into new entity
* For the **right** diagram: A student is guided by a particular instructoron a particular project.A student, instructor, project combination may have an associated evaluation
	* The evaluation is related to the rectangular as a whole. The idea is **eval_for** goes to proj_guide, but in ER diagram we cannot have two relationships connected. Instead, we can make **eval_for** a table, one key references *evaluation*, one key references *proj_guide*.
	* However, a more common approach for this example is shown as follows:
		* s_ID, project_id, i_ID uniquely identify the rectangular. We just need to join evaluation with those three keys. According to Prof. Ferguson, We can collapse eval_for and proj_guide together **As long asyou are OK with evaluation_idbeing NULL until there is an evaluation**.

31. For the love of peace, do not do the recursion in SQL. BUT SQL CAN DO RECURSION.

32. Roles

![](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/28Feb_17.jpg) 


* Whenever you have a relationship, both entity sets participates in a row. Here *prereq_id* is a role. Some times you may have a particular entity participating in several times with different roles. We can think prereq as a table. Two columns in that table both contains courseid, one is course_id, one is prereq_id. 
* According to Prof. Ferguson, **The concept of roles applies to all relationships/associations.**

33. Reasons to use views
* Views, which are a type of virtual tables allow users to do the following
	* Structure data in a way that users or classes of users find natural or intuitive.
	* Restrict access to the data in such a way that a user can see and (sometimes) modify exactly what they need and no more. 
	* Summarize data from various tables which can be used to generate reports. 
	
34. **create table table_2 like table_1**
* Create another table exactly like table_1(column type, primary key etc.)

35. Example of loading students who have registered the class COMS4111 
* Students may show up simultaneously in different sections lect '002','H02','V02'
* Using the following code, concate the uni, studnet_id and section to show duplicates
```sql
select a.uni, a.student_id, group_concat(a.uni) as unis, 
	group_concat(a.student_id) as ids,
	group_concat(a.section) as sections,
	count(*) as count from
	(select *, '002' as section from class_roster_0
	union all
	select*, 'H02' as section from class_roster_h
	union all
	select*, 'V02' as section from class_roster_v) as a 
	group by a.uni, a.student_id
	order by count desc;
```
* When loading the data, record the section in the section column. Here Prof. Ferguson's strategy is to ignore the enrollment in the 'H02' section and go with '002'
```sql
drop table if exists class_roster;
create table class_roster as 
	select class_roster_0.*, '002' as section from class_roster_0;
	
insert into class roster
	select class_roster_h.*, 'H02' as section from class_roster_h
		where 
			uni not in (select uni from class_roster);

insert into class roster
	select class_roster_v.*, 'V02' as section from class_roster_v
		where 
			uni not in (select uni from class_roster);
```
* Since the poins "3" in csv file is a string, further need to modify the type
```sql
update class_roster set Points=substr(Points,1,1);

ALTER TABLE`w4111examples`.`class_roster`
CHANGE COLUMN`Points``Points` INT NULL,
CHANGE COLUMN`section``section` ENUM('002,'H02','V02') NOT NULL,
ADD PRIMARY KEY(`Student_ID`),
ADD UNIQUE INDEX `Uni_UNIQUE` (`Uni` ASC) VISIBLE;
```

* Some of the fields are potentially sensitive, e.g. 
	* UNI
	* student_id 
* We can create a view only showing the first three characters for secirity
```sql
create view secure_roster as
SELECT
	concat(substr(uni,1,3),"*****") as uni_prefix,
	First_name,
	Middle_name,
	Last_name,
	Email,
	School,
	Section
FROM w4111examples.class_roster;
```
