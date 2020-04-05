

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
    * In the following example,

![GitHub Logo](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/Erroneous%20use%20of%20relationship%20attributes.png)


5. Difference between a logical model and a more detailed physical model.
