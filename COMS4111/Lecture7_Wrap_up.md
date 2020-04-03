

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
| ID  | v |
| ------------- | ------------- |
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
* Single-valued attribute
* Multivalued attributes
  * multivaled attributes: email addresses(**more than one**)
* Derived attributes
  * Can be compute from other attributes
  * For instance, age, give date_of_birth
  
5. Domain: the set of permitted values for each attribute, for instance, varchar.
