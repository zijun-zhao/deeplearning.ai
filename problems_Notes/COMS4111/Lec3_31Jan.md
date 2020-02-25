
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

  

## 31 Jan 2020
----------
1. Prof. Donald F. Ferguson said
> A database server has databases and tables.

2. pymysql.connect in **Python**
  ```Python
  # Like files, we need to import some packages.
  import pymysql
  # This is similar to "opening" the file system.
  conn = pymysql.connect(host="localhost", user="dbuser",
                         password="dbuserdbuser",
                        cursorclass=pymysql.cursors.DictCursor)
  ```
  * cursorclass : the Custom cursor class to use.
  
3. Show table names from a database
  ```Python
    sql = "show tables from " + db_name
    cur = conn.cursor()
    res1 = cur.execute(sql)
    res = cur.fetchall()
  ```
  
4. Use of conn.commit() in **Python**
 * Commit any pending transaction to the database. Note that if the database supports an auto-commit feature, this must be initially off. An interface method may be provided to turn it back on. Database modules that do not support transactions should implement this method with void functionality.
 
5. Prof. Donald F. Ferguson said
> Figuring out how to map from kind of junky file data into structured DB data is part of using databases.

6. Several SQL usage showing up in Lecture 1
   * Show table of certian DB
     ```Python
     conn = pymysql.connect(host="localhost", user="dbuser",
                         password="dbuserdbuser",
                        cursorclass=pymysql.cursors.DictCursor) 
     cur = conn.cursor()
     res1 = cur.execute("show tables from " + db_name)
     res = cur.fetchall()
     ```
   * describe certain table
     ```Python
     res1 = cur.execute("describe " + table_name)
     rows = cur.fetchall()
     ```
   * count
     ```Python
      res2 = cur.execute("select count(*) as count from " + table_name)
      rows = cur.fetchall()
      count = rows[0]['count']
      conn.commit()
      ```
   * Combine pandas for query
     ```Python
     db_cnx = pymysql.connect(host='localhost',
                               user='root',
                               password='dbuserdbuser',
                               db='imdbnew',
                               charset='utf8mb4',
                               cursorclass=pymysql.cursors.DictCursor)
     start_time = time.time()
     name = 'Tom Hanks'                          
     df = pd.read_sql("select * from name_basics_fast where primary_name='" + name + "'", db_cnx)
     elapsed_time = end_time-start_time
     print("Time to find people named ", name, "using a database was ... was ", elapsed_time)
     ```

7. Prof. Donald F. Ferguson said
> Using the file is 𝑂(𝑛) for a simple lookup for 𝑛 rows.
> The database performance is 𝑂𝑙𝑜𝑔(𝑛) .

8. Prof. Donald F. Ferguson said
> Some core concepts we will learn in this course are:
 > - The roles of entity-relationship (data) modeling in solving problems with data and databases.
 > - Design patterns and best practices for modeling and defining data.


## 1 Feb 2020
----------
1. Several SQL usage showing up in Lecture 1
   * open soma files(Open records of name_basics which name is 'Tom Hanks'
     ```Python
     import pymysql
     %load_ext sql
     %sql mysql+pymysql://root:dbuserdbuser@localhost/imdbnew
     th = %sql select * from imdbnew.name_basics_fast where primary_name='Tom Hanks'
     ```
   * From title_principals_rel, get all the titles that Tom Hanks stared in
     ```Python
     th = %sql select * from imdbnew.title_principals_rel where  + \
        nconst = (select nconst from imdbnew.name_basics_fast where primary_name='Tom Hanks')
     ```
   * Find the co-principals in the form (principal nconst, role).
     ```Python
     ans = %sql select nconst, primary_name from name_basics_fast where nconst in \
            (select distinct nconst from title_principals_rel where tconst in \
                (select tconst from imdbnew.title_principals_rel where  \
					             nconst = (select nconst from imdbnew.name_basics_fast where primary_name='Tom Hanks')))
     ```
2. What does **atomicity mean** in database?
  * Atomicity is one of the ACID (Atomicity, Consistency, Isolation, Durability) transaction properties. 
  * An atomic transaction is an indivisible and irreducible series of database operations such that either all occur, or nothing occurs.
  * Example: Transfer of funds from one account to another should either complete or not happen at all.

3.
> A database system is a colluection of interrelated data and a set of programs that allow users to access and modify these data.
> A major purpose of a database system is to provide users with a n abstract view of the data
  > Data models
    > A collection of conceptual tools for describing **data**(couse, person), **data relationships**(donald teaches the class), **data semantics**(meaning of the data, person develops the data knows it), and **consistency constraints**(for example, not null-> this field must have a value for a given field)
  > Data abstraction
    > Hide the complexity of data structures to represent data in the database from users through several levels of data abstraction
   
4. ***Data Models***
  * Relational model
    > Entity only has properties, do not have functions or methods
  * Entity- Relationship data model (mainly for database design)
  * Object-based data models(Object-oriented and Object-relational)
    > has function & method
  * Semi-structured data model(XML) 
    > almost completely be replaced by json
  * Other older models
    * Network model
    * Hierarchical model

5. ***Relational Model*** -> Table, collumn, Row
  * **All the data is stored in various tables**
    > All data stored in relation(table)
    * A row: an entity
    * A column: all properties of entity
  * **Relationships are represented by shared columns and values**

6. ***Levels of Abstraction***: progressive refinement, progressivecompleteness
	* **Physical level**: describes how a record is stored
		* > What is the column; Type of the column; Maximum length of a column
	* **Logical level**: describes data store in database, and the relationships among the data
	* **View level**: application programs hide details of data types. Views can also hide information(such as an employee's salary) for security purposes.

7. Instances and Schemas
	* **Logical Schema**: the overall logical structure of the database
		> The database consists of information about a set of customers and accounts in a bank and the relationship between them
		* Analogous to type information of a variable in a program
	* **Physical schema**: the overall physical structure of the database
	* **Instance**: the *actual content* of the database at a particular point in time
		* Analogous to the value of a variable
		
8. All databases have ***two languages*** associated with them. 
	* DDL
		> **How you define and modify the schema: type, size**
		```sql
		create table instructor(
			ID	char(5),
			name	varchar(20),
			dept_name	varchar(20),
			salary	numeric(8,2))
		```
		> Makes an entry of the table??????/？？？？？？？
		* DDL compiler generates a set of table templates stored in a **data dictinary**
	* DML
		> How you change the instance data
		* also known as *query language*
		* Two types of data-manipulation language
			* Procedural DML: requires a user to specify what data are needed and **how** to get those data
			> If, then, while. Also called perative language
			* Declarative DML: require a user to specify what data are needed without specifying how to get those data
			> like I just describe what answer I want, declaring what you want.
				* SQL is a declarative language, **more compact**
				* Just write statement, no need to write program								
	
9. A special DB called catelog / **data dictionary**: it is a database has all the data about other databases. 
	> A table has all information about tables
	* Data dictionary contains metadata(data about data)
		* Database schema
		* Integrity constriants
			* Primary key
		* Authorization
			* Who can access what
	* We cannot create a table the second table. Table is a primary key for the table of tables.
10. SQL Query Language
	 * Nonprocedual.
	 	* A query takes as input several tables(posssibly only one) and always returns a single table
	* ***NOT a Turing machine equivalent language***
	* To be able to compute complex functions SQL is usually embedded in some higher-level language
	
11. Database Access from Application Program
	* SQL does not support actions such a s input from users, output to displays or communication over the network.
	> We cannot access data from application. We can write an application which deals with the raw data, or write a program that lie on the database then declare what you want.
	> Even the IMDb case there is an application on top of the database.
	
12. **Database Users**
	 * Four different types of database-system users
	 	* Naive users
			> Do not see the data. Like when using IMDb
		* Application programers
			> Write programs that naive users use
			> When writing an application, you are just use a view of the data, not designing the database
			> Understand how to do the basic DML
			> Are not sophisticated
			> Uses view of data, not designing the database
		* Sophisticated users
			> Database programmer/ Database application developer
			> Define the query, schema, views
		* Specialized users
			> Called database admins: do mostly management stuff
13. In this lecture, core concepts, relational algebra and SQL realization will be involved. **We will focus most on DML and DDL**.

14. SQL Parts
	* DML: All database have 4 constructs: ***CURD***
		* Create
		* Retreat
		* Update
		* Delete
	> Provides the ability to query information from the database and to insert tuples into, delete tuples from and modify tuples in the databse.
		* In SQL, it is composed of：
			* Insert
			* Select
			* Update
			* Delete
	* Integrity: the DDL includes commands for specifying integrity constraints
	> Rules associated with the data
	* View: the DDL includes commands for defining views
	> A layer put on top of the data
	* Transaction control: includes commands for specifying the beginning and ending of transactions
	* Embedded SQL and dynamic SQL
	* Authorization: includes commands for specifying access rights to relations and views
	> Who is allowed to do what on the data
	
15. Domain Types in SQL
* char
	> fixed length
	
16. Create Table Construct
	* An SQL relation is defined using the **create table** command
		```sql
		create table r
			(A1 D1,
			A2 D2,
			A3 D3,
			.....,
			An Dn,
			(integrity-constraint1),
			.....,
			(integrity-constraintn)
			)
		```
		* r is the name of the **relation**
		* each A is an **attribute name** in the schema of relation r
		* each D is the **data type** of values in the domain of corresponding attribute 
	
	
17. Integrity Constraints in Create Table
	*Types of integrity constranints 
		* **primary key**(A<sub>1</sub>,A<sub>2</sub>,..., A<sub>n</sub>)
		> A set of columns whose value must be unique, ex, postcode needs to be unique whithin a contry
		> primary key is unique within a table. 
		* **foreign key** (A<sub>m</sub>,..., A<sub>n</sub>) **references** r
		> B can only exist if A exist. For examply, there can't be an entry in the transcript table if there is no matching student entry. UNI and transcript will be forein key into student table. Similarly, class and transcript is a foreign key in the class table.
		> forieign key is a dependence statement. uni will not be unique in the class table since each student can take many classes. 
		* **not null**
		
	* Any update to the database that violates an integrity will be prevented
	
	
18. **Updates to tables**
	* Insert
		* Insert values
	* Delete
		* Remove all tuples from the student relation
			* delete from student
			> Delete only the row, but the table is still exist
	* Drop Table
		* drop table r
		> The whole table is removed
	* Alter
	> Change definitioni of a table, it changes the existing thing.
		* alter table/ add A D
			* where A is the name of the attribute to be added to relation r and D is the Domain of A
			* All existing tuples in the relation are assigned null as the value for the new attribute
		* alter table r drop A
			* where A is the name of attribute of relation r
			* Dropping of attributes not supported by many databases
	> Table should have a primary key. But if it doesn't, you can later alter it.
	> Simple pure version of SQL tells that table must have a primary key. But actually we can create one without primary key and later alter table with primary key.  But if there are already duplicate values in that field, alter will fail. 
	> Table has definition, it won't allow you to put data that violates the defination
	> It also won't allow you to change the definition that violates the data

19. **Basic Query Structure**
	```sql
		select A1, A2, ..., An
		from r1, r2, ..., rm
		where P
	```
	* P is a predicate
	> implicit join is not recommended, so never use more than one r

20. Several "visual notations" for drawing ER Diagrams
	* [Crow's Foot Notation], which is what MySQL uses.
	* The recommended textbooks uses a different model ER-Modeling notation.
	* Unified Modeling Language also supports ER models.
	
21. Several SQL usage showing up in Lecture 2
	* Select from the table of students all the columns with the departure name 'Comp. Sci.'
		```Python
		%sql * from newbook.student where dept_name in ('Comp. Sci.')
		```
	* Only want the name and department name
		```Python
		%sql select ID, dept_name from newbook.student where dept_name in ('Comp. Sci.')
		```
22. SQL is the standard, which has several versions including SQL 92, sql 2003. Mysql is a product that company makes. Oracle 10G is also a product made by oracle. SQL server is a product made by microsofy. They all supprt the SQL language. If you code in standard SQL, it can run everywhere. They all have extensions, some of them do not work on others. mySQL has an extension of Spatial data which others don't.

## 2 Feb 2020
----------
1. According to Prof. Ferguson/
	> homework will be composed of common written questions, common core ER, DDL and DML Core scenarios and Web Application CSV Data Engine. The programming trach will build a part of the full stack web application.
	> Only by working on hard homework problems can one learn sth.
	* DFF and TAs with provide helper code, soma data/databases, helper code/libaries, implementation template
	* Both tracks will cover the same core: databases, data models, queries, data transformation
	* Programming Track: REST API, web appication, code to manage/transform data
	* Data Environment/Subsystem: Analytics <-> Operational
		* The Analytics takes information from Operational, like it take transaction that happen.  Using tools to figure out how to handle the data.
		* The Operational takes extra information, it is read-only, not changing the part of operation. Programmeing track will focus on Analytics.
	* Web Application will be built to access the information above.
2. 
	* Take the jason format and map them to a relational DB.
	* Put in a graph database, map the file to a structure I can load
	* Additionally, 
3. Helpful online notes [Ehi Kioya's Notes](https://ehikioya.com/conceptual-logical-physical-database-modeling/) for Lecture3 Entity-Relationship Modelling
	* A useful picture
	![Levels of Data Modeling](https://ehikioya.com/wp-content/uploads/2019/02/Understanding-Conceptual-Logical-And-Physical-Database-Modeling.jpg)
	* Quotes from the above blog
	> * A database model is a **representation of a database** showing its design and structure and the manner in which data can be stored, organized, accessed and manipulated, which is *often presented visually by means of a diagram*
	> * Entity Relationships Diagrams (ERDs) is one of the diagrams used in database modeling. Entity Relationships Diagrams (ERDs) is also known as an **Entity Relationship Model**, which is a **graphical representation** of entities (like people, objects, events, places, etc.) and how they relate to each other.
	> * Database models are easy to review.
	> * With the information from Entity Relationship Diagrams obtained from the conceptual layer, developers at the logical layer can determine how to **physically** design the database and the corresponding software application
	> * Properties of table columns like data type, length of data stored therein, and default values of the columns are definedin a physical database model.
	> * To sum up
	>	* **Conceptual database modeling** is data modeling done at a very high level by project stakeholders and architects. Unlike physical database modeling, conceptual modeling ***does NOT*** contain low-level, granular information.
	>	* **Physical modeling** is used for actual database design. Nothing high level here. This is pure low-level goodness. Almost as beautiful as raw code!
	>	* **Logical database modeling** sits somewhere in-between conceptual and physical database modeling.
4. Refered to [datawarehousing](https://www.1keydata.com/datawarehousing/data-modeling-levels.html):

Feature | Conceptual | Logical |Physical
------------ | -------------| -------------| -------------
Entity Name | √ | √  |
Entity Relationships| √ | √ |
Attributes | | √ |
Primary Keys | | √ | √
Foreign Keys | | √ | √ 
Table Names | | | √ 
Column Names | | | √ 
Column Data Types | | | √ 

5. What does cardinality constratins mean?

6. Two mechenisims during modelling
	* Top-down
		* Progressive refinement, do not know what the data is
	* Bottom up
		* Have the data, do not know the data. Trying to figure out the underly schema and how to put it to the relational model.

7. Does it matter where to put the self-relationship line to the rectangular?
	
8. There is almost never an optimal version for data modeling
	* When I go to a physical model, there should no many to many
	
9. 
	> A relationship set is a mathematical **relation** among n>=2 entites

10. Quesetion from student: what is the difference between relation and database:
* Answer by Prof. :
	> **An Entity (class) is the ER representation of what the Relational Model calls a Relation. Everything in the Relational Model is a relation.**
	> **A relation in ER maps to either a foreign key constraint or a table implementing the relation pattern.**

11. Prof. Ferguson  
	> Addition is fine for string, while ordering string may cause problems

## 4 Feb 2020
----------
1. The SQL WHERE BETWEEN syntax **sql**
	```sql
	SELECT column-names
	  FROM table-name
	 WHERE column-name BETWEEN value1 AND value2
	```
2. Select unique rows:
	* Use select **distinct**


## 5 Feb 2020
----------
1. Do not consider NULL entries when calculating average
	* We can use avg(col_name)
	* We can't use col_value/count(col_value)

2. Syntax
	* When order by multiple things, use comma to separate them, not 'and'
	```Python
	order by teamID desc, yearID desc
	```
	* Similarly, group by also applies to this rule
	```Python
	group by teamID, yearID
	```
3. Usage of **in** and **exists** in query
	* According to Prof. Ferguson  
	> **IN supports only equality relations and is used to compare one value to another**
	> **EXISTS supports multiple types of relations and will tell you whether a query returned any results**


## 6 Feb 2020
----------
1. Change the space between the itemize “items” in **LaTeX**?
	* \itemsep can adjust the space
	```latex
		\begin{itemize}
			\setlength\itemsep{1em}
			\item one
			\item two
			\item three
		\end{itemize}
	```
2. Add an empty line between paragraphs	in **LaTeX**
	```latex
	\bigskip
	\medskip
	\smallskip
	```
3. Cancel the first line indent in **LaTeX**
	```latex
	\noindent
	```
4. Instert graph in **LaTex**
	```latex
	\begin{figure}[H]
	    \begin{center}
		\includegraphics[width=0.7\columnwidth]{fig2.png}
			\caption{An example ER model for Columbia classes without assumption}
			\label{fig:2}
	    \end{center}
	\end{figure}
	```
	* To center the graph, add [H]
	* To place the graph on top of the page, add [t]
	* To place the graph at the bottom of the page, add [b]
	* To place the graph on top of a page allowing float object, use [p]
5.Cite a graph in **LaTex**
	```latex
	\ref{label_of_graph}
	```
6. Draw a simple table
	```latex
	\begin{center}
	\begin{tabular}{|c|c|c|c|c|c|}
	\hline  % line on top of the table
	uni&Gender& Firstname & Lastname & BirthCountry & Advisor\\
	\hline  % line above and below the first row
	zz2563&Female& Zijun&Zhao&China&Gil\\
	\hline % line at the bottom of the table
	\end{tabular}
	\end{center}
	```
7. To reduce the white space between paragraphs: 
	```Latex
	\vspace{-5pt}
	```
## 7 Feb 2020
----------
1. Question solved in OH
	* Difference between = and in in SQL
		* Prof. Ferguson said we cannot do select * from People where schoolID = (select schoolID from Schools where state = 'NJ)
	* How to connect two different tables using crow's foot:
		* It does not matter where put the end of the link.
	* Do we need to represent relation using diamond shape?
		* diamand shape is not needed in our form of ER diagram
## 8 Feb 2020
----------
1. When designing a database from logical to physical model, a lot is needed to consider.
	* An associate  entity
		* Aims to relate other entites. A row in that table represents a line between a specific student and a specific section. Each row represents the connection between two instances. If not having this associate table, we may lose information. For instance, if a student changes his advisor, then we need another attribute in the associate relation table to record the start_data he chose one advisor.
	* Corner condition met when implementing in physical model
	```sql
	CREATE TABLE 'Course'(
	'dept_code' char(4) NOT NULL,
	'faculty_code' enum('W','E','C','G','B') NOT NULL,
	'level' enum('0','1','2','3','4','6','9') NOT NULL,
	'number' char(3) NOT NULL,
	'site' varchar(12) NOT NULL,
	'course_no' varchar(12) GENERATED ALWAYS AS (concat('dep_code','faculty_code','level','number'))STORED,
	PRIMARY KEY ('dept_code','faculty_code','level','number'),
	UNIQUE KEY 'course_no_UNIQUE'('course_no')
	)ENGINE=innoDB DEFAULT CHARSET=ut8mb4 COLLATE=ut8mb4_0900_ai_ci;
	```
		* Note here we cannot set the course_no since it is generated by others and we store it, which is a *generation statement*. After storing, we can represent as a foreign key to represent it. We need to speicify in the SQL workbench.
		
2. Primary key is **unique** for the table it is defined as primary key.	

3. **A strength of the relational model: integrity rules are inextricably attached to the data**. The database engine has got the data that allows you to run a fixed set of operation against it and it get a set of integrity constrains, computer readable descriptions of the data,
	* Example operates to columns: not null is an example, default is also an example. In relational model, integrity is inherent part of the model
	* Example operates to table level: key is an example. Like uni need to be unqiue. In relational theory key is what most people cares about. 
		* For the student entity, it has uni and email. Email has to be unique, so as the uni. If you have a table of people and you change the email, it is still the same person. For *primary key, we need something immutablae*. uni is an  immutable property.
		
4. For each entry, there are 
	* "atomic": number of string
	* "multi-valued": lists or dictionaries. For example, the one-to-many relation between episode and location

5. Type "scenes" can be a list of things, including sceneStart, location, sublocation, character. In addition to the entity the episode, there is an entity type 'scene' which his not on a separate file, scenes are *contained* in the episode type. When two entities are related, many ways to think about **one-to-many, many-to-one, required or contained**. In university, faculty and student are all instances of person. If two entities are not isa, we need to ask is it sort of containment. If we delete a scene, the character is not contained in that scene, it is just referred to. But throw away an episode, the scnens are also deleted. There is a one-to-many relationship between episode and scene. One episode has one or more scenes. A scene is linked to exactly one location. There is location that does not shown in scene, like people talked about it but never shows it.

If a table is contained in something, you require to have a container. But you could have acquired relationships that are not containment, and so these are the ways you need to think about during physical model.

6. What is the difference between references and contains:
	* Contains: **weak entity**
	> "In a relational database, a weak entity is an entity that cannot be uniquely identified by its attributes alone; therefore, it must use a foreign key in conjunction with its attributes to create a primary key. The foreign key is typically a primary key of an entity it is related to." For example, the prmary key of an episode is the season number+episode. We cant't make a key for scene without making a key for episode. We make a key value, you will always make a key in another table, it kinds of a part of it. The primary key for a scene is (seasonNumber, episodeNumber, sceneNumber). You cannot form the key without the properties of an episode.
	* What is an example of references that is not weak? 
		* Consider a banking account. Hypothetically, accountNumber is the primary key. customerID is a foreign key that links acount to customer.
		
7. As to the integrity constraints in create table
	* Except for primary key, foreign key, there are also check constrains, default values and generated column values.
8. Set operation has three types: union, intersect and difference. Each of these operations automatically eliminate duplicates. To retain all duplicates need to use
		```sql
		union all
		intersect all
		except all
		```
	* According to Prof. Ferguson:
	> **Not all databases support all the set operation, there are workarounds using JOIN and UNION**
9. In relational thoery, a relation is **a set** of tuples, we cannot have exactly the smae tuple twice. The reason why a relation is a set is that they all have primary key, and a primary key is unique,
	* In relational databases, that is not true. Relational database tables can have duplicates. The way you prevent duplicate is to define a primary key. **primary key is unique** so no two rows are exactly the same.
		* In relational databses, primary key is optional since **relational algebra is close under all the operators**, doing a select statement we do not need to pick all the columns. If my select statement doesn't pick the primary key column, we cannot guarantee the rows are unique in the resulted table.
		* Therefore derived table, are not sets in theory.

10. **view** looks like a table, but it will be calculated every time you view it.

