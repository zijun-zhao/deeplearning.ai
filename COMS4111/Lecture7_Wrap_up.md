

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

By union operation, the resulted table will have **same number of columns**, the default setting of union won't have duplicates. If you want duplicate, then need to union all.
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


By natural join, it will use the coloumn in common:id to combine the two
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
  * After σ, what is the size of the table that comes out: selectivity of the predicate. If xxxxxxxxxxxxxxxxxxxxxxx40:34
 
 3. In table
 * attributes are columns
 * Entity is a set of attributes and values.
 * Entity set is set of entiteis.
 
4. Attribute types:
> Entities are represented by means of thier properties, called attributes. All attributes have values.

* Simple attribute
* Composite attribute
  * Composite attribute: for instance, name: Tom Hanks
  * It allows us to divide attributes into **subparts**(other attributes)
   * For example, name can be divided in to first_name, middle_name, last_name
* Single-valued attribute
* Multivalued attributes
  * multivaled attributes: email addresses(**more than one**)
* Derived attributes
  * Can be compute from other attributes
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
9. Compound attributes can be multiple columns in an entity, it can also be a separate table/entity with multiple columns.

10. In a **relational database**, a **weak entity** is an entity that cannot be uniquely identified by its attributes alond; therefore, it must use a **foreign key** in conjuction with its attributes to create a **primary key**. The foreign key is typically a primary key of an entity it is related to.
  * For instance, a course requires an instructor, but instructor is not in the course.

11. A **weak entity set** is one whose existence is dependent on another entity, called its **identifying entity**. Instead of associating a primary key with a weak entity, we use the identifying entity, along with extra attributes called **discriminator** to uniquely identifying a weak entity.

* Every weak entity must be associated with an identifying entity. The weak entity set is said to be **existence dependent** on the identifying entity set.
  * The identifying entity set is said to **own** the weak entity set that it identifies.
  * The relationship associating the weak entity set with the identifying entity set is called the **identifying relationship

12. We cannot use double rectangle to express weak entity set in ER diagram.

13. Dashed lines in ER diagram refers to an **identifying relationship**. A dashed line means that the relationship is strong, whereas a solid line means that the relationship is weak. 

14. For ER diagram with a **Ternary Relationship**, in SQL/RDB, this requires four tables(relations):
 * Intructor
 * Student
 * Project
 * Project Participants
