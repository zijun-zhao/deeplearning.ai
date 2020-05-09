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
9. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)
10. [Lecture11-10Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture11_QueryProcessing&Transactions&Recovery.md)
11. [Lecture12-17Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture12_Transactions.md)
12. [Lecture12-17Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture13_NoSQL.md)



* Course Modules 
	* Transaction Concept
	* Transaction State
	* Concurrent Executions
	* Serializability
	* Recoverability
	* Implementation of Isolation
	* * Transaction Definition in SQL
	* Testing for Serializability.

  
## 17 Apr 2020

1. NoSQL Databases
> Database did not support sql, or completely not relational. Original, all databases are noSQL. Then relational database come along, everything become relational database. People begin to realize relational has limitations later.

> Now NoSQL tends to mean "not only SQL", it will suppoty a sql-like query, and it will not be strictly relational, it will relax integrity and consistency constraint.
	* However, by forcing integrity and consistency, relational database limits the scalibility and availability.

* The primary difference between NoSQL and SQL database: Many NoSQL stores compromise consistency. 


> Instead, most NoSQL databases offer a concept of "**eventual consistency**" in which database changes are propagated to all nodes "eventually" (typically within milliseconds) so queries for data might not return updated data immediately or might result in reading data that is not accurate, a problem known as stale reads


2. Types of NoSQL database:
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_15.jpg)
* What is the underline base data model?
	* relational database: Tables, tabula data 
	* document database: json is hierarchical
		* nested and hierarchical
		* mongoDB
	* graph databases: 
		* data model is graph.
	* wide column stores
		* lists of combinations of columns. Domain does not need to be atomatic
	* key-value database
	
3. CAP Theorem: foundamental theory
	* Consistency: Every read receives the most recent write or an error.
	* Availability: Every request receives a (non-error) response, without guarantee that it contains the most recent write.
	* Partition Tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes
		* The system continues to operate even though there are failures. Network can get partitioned. Like nodes in one set cannot talk to other set.
> ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_16.jpg)

* There are no database that can satisfy these three, only two of them can be satisfied. We can classify a database according to the line.

4. Consistency Models
* **STRONG CONSISTENCY**: Strong consistency is a consistency model where all subsequent accesses to a distributed system will always return the updated value after the update.
* **WEAK CONSISTENCY**: It is a consistency model used in distributed computing where subsequent accesses might not always be returning the updated value. There might be inconsistent responses.
	* A statement of shared memory. Do not need to worry about it
* **EVENTUAL CONSISTENCY**: Eventual consistency is a special type of weak consistency method which informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value.
	> What we really cares about
	* if one application does a write, if you keep reading, you will eventually see that value
	* Due to availablity, as the figure below, you will finally get the value you write if you keep reading. However, problem of overwrite may happen.  
Availability and scalability via| Several algorithms for conflict resolution|Illustration
---|---|---
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_18.jpg)|Detect and handle in application&Clock/change vectors/version numbers|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_17.jpg)

* Another tradeoff for eventual consistency: Partition Tolerance
	* Database get logging, before get the put, we can lock all the copys, then we can do the *put* then release all the copy. But we cannot lock all the copy if there are partitions. In a replicate database, we can get consistency through locking, but we will not able to lock a lot of copies. For availablity, we need multiple copies. For consistency, we need lock all the copies since we do not tolerate partitions. If we have partition tolerance, we cannot lock all the copies, then we do not have consistency.

> How many replicate are there? It depends on how many replicate you need. Replicate is for performance and avialbility. If you care for availability, then you will only need two.

## 24 Apr 2020

1. Remember the four modules:
* Module I: SQL realization, relational model
* Module II: Implementation details: transactions, caching, buffering, locking
* Module III: NoSQL
2. NoSQL: not only SQL, since SQL is a pretty good query language.
	* When talking of constraints, the relational integrity are great for insert, but it slows down and make it hard to scale out. By relaxing the constraints we may get better scalibility and performance.
	* Relational database guarantees consistency. Most NoSQL database compromise consistency.
* Two reasons to turn to NoSQL:
	* The performance requires you to relax
	* The data model is hard to represent as a relational model.
	
3. Concept of BASE
	* **Basically Available, Soft state, Eventual Consistency**
	* Most relational database guarantees ACID transaction model, while most NoSQL does some variation of BASE.
4. Eventual Consistency


	* If we want high availability, we need to have more than one computer serving the data. More than one database server is needed.
		* When doing a write, it goes to one of the instances. At that point, the instance will propagate the change(replication). If we do a put(take 0.1s), it may take 0.3s to get to the destination, but a get may only take 0.2s, then the replication has not arrived yet. By keeping doing the get, we will eventually be consistent. One of the problem is that if we do a write after the get.
			* To solve that, we will set a timestamp with it to trigger an abort.
	* To ensure consistency, we can lock all the copies. Then write on the four pieces before release the lock. 
		* Drawback is that by locking all four copies, the network has to be connected, and all the nodes need to be connected. We are not partition tolerant

	high availability| high consistency
	---|---
	![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_23.jpg)|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_24.jpg)

5. Graph Databases
* Two core types
	* Node
	* Edge (link)
		* link can have multiple lables
* Nodes and Edges have
	* Label(s) = “Kind”
		* If two Node have the same label, then they are "in the same table", the difference is that a specific node can have **multiple labels**. 
	* Properties  (free form)
		* Values on the edge
		* Not tight, no schema.
* Query is of the form
	* p1(n)-p2(e)-p3(m)
		* Apply predicade to all the node p1(n) and see which matches. Then apply p3(n). On all the edge that connect the two sets, apply predicatep2 
	* n, m are nodes; e is an edge
	* p1, p2, p3 are predicates on labels

6. ![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_25.jpg)
* Here color represents the label
	* Blue are movie
	* Green are people
	* Return all the edges where act in 
7. Why Graph Databases?
	* Very natural to express graph related problem with traversals
8. Six degress of Kevin Bacon
```SQL
MATCH p=shortestPath(
(bacon:Person {name:"Kevin Bacon"})-[*]-(meg:Person {name:"Meg Ryan"})
) 
RETURN p
```

* Create a node, labeld Fan. This node is both a fan and a person
```sql
CREATE (p: Person:Fan {uni:"dff9",lastname:"Ferguson",firstname:"Donald"})
```
* Create a node, labeld Fan. This node is both a fan and a person
```sql

match (m:Movie{title:"Apollo 13"}) return m
```
Same with
```sql

match (m:Movie) where m.title="Apollo 13" return m
```

match (m:Movie{title:"Apollo 13"}) ,(f:Fan {uni:"dff9"}) return m,f|match (m:Movie{title:"Apollo 13"}) ,(f:Fan {uni:"dff9"}) create(f)-[r:LIKES{stars:"xxx"}]->(m)|match (m:Movie{title:"Apollo 13"}) ,(f:Fan {uni:"dff9"}) return m,f
---|---|---
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_26.jpg)|-|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/17Apr_27.jpg)

* If we want to know what kind of moview Ferguson likes, run
```sql
match(f:Fan {uni: "dff9")-[r:LIKES]->(m) return f,r,m
```
* Find people that acted in Apollo 13 and then find movie they stared in
```sql
match(f:Fan {uni: "dff9"})-[r:LIKES]->(m)<-[r2:ACTED_IN]-(p2)-[r3:ACTED_IN]-(m2) return f,r,m,r2,p2,m2
```
* Movies and actors up to 4 hops away from Kevin Bacon
	* Here (hollywood) will find all of the nodes
```sql
MATCH (bacon:Person {name:"Kevin Bacon"})-[*1..4]-(hollywood)
RETURN DISTINCT hollywood
```
* Movies and actors up to 4 hops away from Kevin Bacon
	* Only return the movie
```sql
MATCH (bacon:Person {name:"Kevin Bacon"})-[*1..4]-(hollywood: Movie)
RETURN DISTINCT hollywood
```

* Bacon path, the shortest path of any relationships to Meg Ryan
```sql
MATCH p=shortestPath(
(bacon:Person {name:"Kevin Bacon"})-[*]-(meg:Person {name:"Meg Ryan"})
)
RETURN p
```

* find all the movie Kevin Bacon acted in
match (p:Person{name:"Kevin Bacon"}) return p| match (p:Person{name:"Kevin Bacon"})-[r:ACTED_IN]-(m:Movie) return p,r,m
![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/24Apr_1.jpg)|![Image](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/24Apr_2.jpg)




9.
