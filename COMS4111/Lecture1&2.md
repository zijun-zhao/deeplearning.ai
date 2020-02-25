
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

## 24 Jan 2020
----------
1. An example of web application refers to [Mark Dabbs' Blog](https://reinvently.com/blog/fundamentals-web-application-architecture/)
![Web Application Architecture](https://reinvently.com/wp-content/uploads/2019/08/scheme.jpg)
	> A web application consists of many components like the user interface, a login-screen, an in-app store, the database, etc. 
	> To manage these components, software engineers devised web application architecture to logically define the relationships and manner of interactions between all of these components for a Web app.
	> Every Web application consists of both a front-end and a back-end.
		> The **front-end**, also known as the **client-side**, is everything that the user sees and interacts within inside their browser(Chrome). The main purpose of the client-side is to *collect data from users*. It is written in variants of HTML, CSS, and JavaScript.
			> The prowser sends a network message that is effectively a function call
		> Next, we have the **back-end**, also known as the **server-side** of the app. It is the part, which is not accessible by users; it *stores and manipulates data*. The backend **processes HTTP requests which essentially “fetch” the data (text, images, files, etc.) called for by the user**. Unlike the frontend, many languages like PHP, Java, Python, JavaScript, and others can be used to write the backend of a Web Application.
			> Several pieces come together on the server to implement the method:
				> Route handler code implementing method
				> Static content that forms a document/document elements
				> Downloaded code that will process and interpre data in the browser
				> Data in one or more databases


2. A TSV file is a simple text format for storing data in a tabular structure. Often used in data exchange to move tabular data between different computer programs that support the format. Ex, transfer information from a databases program to a spreadsheet.

3. SQL **DOES NOT SUPPORT** input from users, output to displays or communication over the network.	
	> HOST LANGUAGE is needed to do these computations and actions. Using C/C++, jAVA, Python with embedded SQL queries that access the data in the database.
	
4. Functional components of a database system:
	* The storage manager
	* The query processor component
	* The transaction management component
	> Details refer to page 70-71 of Lecture 1
5.
> A database system is a colluection of interrelated data and a set of programs that allow users to access and modify these data.
> A major purpose of a database system is to provide users with a n abstract view of the data
  > Data models
    > A collection of conceptual tools for describing **data**(couse, person), **data relationships**(donald teaches the class), **data semantics**(meaning of the data, person develops the data knows it), and **consistency constraints**(for example, not null-> this field must have a value for a given field)
  > Data abstraction
    > Hide the complexity of data structures to represent data in the database from users through several levels of data abstraction
   
6. ***Data Models***
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

7. ***Relational Model*** -> Table, collumn, Row
  * **All the data is stored in various tables**
    > All data stored in relation(table)
    * A row: an entity
    * A column: all properties of entity
  * **Relationships are represented by shared columns and values**

8. ***Levels of Abstraction***: progressive refinement, progressivecompleteness
	* **Physical level**: describes how a record is stored
		* > What is the column; Type of the column; Maximum length of a column
	* **Logical level**: describes data store in database, and the relationships among the data
	* **View level**: application programs hide details of data types. Views can also hide information(such as an employee's salary) for security purposes.

9. Instances and Schemas
	* **Logical Schema**: the overall logical structure of the database
		> The database consists of information about a set of customers and accounts in a bank and the relationship between them
		* Analogous to type information of a variable in a program
	* **Physical schema**: the overall physical structure of the database
	* **Instance**: the *actual content* of the database at a particular point in time
		* Analogous to the value of a variable
		
10. All databases have ***two languages*** associated with them. 
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
	
11. A special DB called catelog / **data dictionary**: it is a database has all the data about other databases. 
	> A table has all information about tables
	* Data dictionary contains metadata(data about data)
		* Database schema
		* Integrity constriants
			* Primary key
		* Authorization
			* Who can access what
	* We cannot create a table the second table. Table is a primary key for the table of tables.

12. SQL Query Language
	 * Nonprocedual.
	 	* A query takes as input several tables(posssibly only one) and always returns a single table
	* ***NOT a Turing machine equivalent language***
	* To be able to compute complex functions SQL is usually embedded in some higher-level language
	

13. Database Access from Application Program
	* SQL does not support actions such a s input from users, output to displays or communication over the network.
	> We cannot access data from application. We can write an application which deals with the raw data, or write a program that lie on the database then declare what you want.
	> Even the IMDb case there is an application on top of the database.
	
14. **Database Users**
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

15. **Structured Data**

	* High degree of organization.
	* All records for a given type have
		* The same fields.
		* The fields are strongly typed.
		* There are _integrity constraints_ on values, e.g. __NOT NULL.__

16. **Unstructured**
	- No explicit records or delimiters. No fields. No properties.
	- Just a sequence of characters/bytes that can be interpreted by some processing engine.
	- Images, audio files, etc. are examples.
	
17. **Metadata**
	> Metadata is "data information that provides information about other data". 
		* Three distinct types of metadata exist:
			* Descriptive metadata
			* Structural metadata
			* Administrative metadata.
	> Descriptive metadata describes a resource for purposes such as discovery and identification. It can include elements such as title, abstract, author, and keywords.
	< Structural metadata is metadata about containers of data and indicates how compound objects are put together, for example, how pages are ordered to form chapters. It describes the types, versions, relationships and other characteristics of digital materials.
	> Administrative metadata provides information to help manage a resource, such as when and how it was created, file type and other technical information, and who can access it.
	> We will cover metadata, especially while studying relational data.

18. There are many, many, many different kinds of database management systems. The differences are:
	* Type of data: tables, documents, images, complex design models, ...
	* Use case optimizations: ready only/query, business transactions, realtime data sharing, ...
	* Deployment scenarios: mobile device, application embedded, high-performance cloud, ...

- We will go through a few representative databases in this course.
19. We will cover the DBMS implementation in Module II. That is, what is the architecture and implementation of the DBMS SW. To understand DBMS models and usage, we will primarily focus on 4 database management systems:
	* Relational (MySQL)
	* Graph Database (Neo4J)
	* Key/Value- data structure store (Redis)
	* Document (CouchDB)
	
20. In the slcass, core concepts
	* The relational algebra
	* The actual SQL realization
	
## 28 Jan 2020
----------
1. Cread a dbuser for testing in order to run the following code:
```Python
%load_ext sql
%sql mysql+pymysql://dbuser:dbuserdbuser@localhost/w4111
```

Here, it is suggegsted by Prof. Ferguson that *we should create a user called dbuser, in the SQL installation as well. And the set it's password as dbuserdbuser. This way he can test it easily and we'd also be able to run the code too*. 

Solvement:
	> Click on the administration tab, then click on users and privileges on the left pane under the administration tab. There is button on the bottom that says add account

2. No module named 'pymysql'
	```Python
	!pip install PyMySQL
	import pymysql
	```
## 29 Jan 2020
----------
1. What does *%load_ext sql* mean in the code cell?
  * Magic commands are a set of convenient functions in Jupyter Notebooks that are designed to solve some of the common problems in standard data analysis. IPython SQL magic extension makes it possible to **write SQL queries directly into code cells** as well as read the results straight into pandas DataFrames. 
  > "IPython will treat any line whose first character is a % as a special call to a ‘magic’ function. These allow you to control the behavior of IPython itself, plus a lot of system-type features. They are all prefixed with a % character, but parameters are given without parentheses or quote." [link](https://ipython.readthedocs.io/en/stable/interactive/reference.html#magic)
  > There are typically two processes (running programs) in a database application.
	>The database server, which processes database commands.
	>The client application that sends commands to and receives responses from the database server.
		>The magic statement defines a database connection.
  
  * Installing SQL module in the notebook
    ```Python
       !pip install ipython-sql
    ```
  * **Loading the SQL module**
     ```Python
     %load_ext sql
     ```
2. Add image in Jupyter notebook
	```Python
	<img src="./file.jpg">
	```
3. URL connection to database
	```
	%load_ext sql
	%sql mysql+pymysql://dbuser:dbuser@localhost/lahman2017
	%sql select * from newbook.student where dept_name in ('Comp. Sci.', 'Elec. Eng.')
	```
	* Note the second line is a URL connection specification.
	> "A Uniform Resource Locator (URL), colloquially termed a web address, is a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it. A URL is a specific type of Uniform Resource Identifier (URI), although many people use the two terms interchangeably. URLs occur most commonly to reference web pages (http), but are also used for file transfer (ftp), email (mailto), database access (JDBC), and many other applications."
	> A URL has the format
	```
	URI = scheme:[//authority]path[?query][#fragment]
	```
	> The components are
		* Scheme: Information about the protocol, connector library, ...
		* Authority: Usually userid:password.
		* Path: File system like folder path to the resource.
		* We will cover query string later.
		* Fragment: A location or subset of the resource, e.g. a section with heading.
	> We connect to MySQL from Python using PyMySQL library by
	```
	default_cnx = pymysql.connect(host='localhost',
				     user='dbuser',
				     password='dbuser',
				     db='lahman2017',
				     charset='utf8mb4',
				     cursorclass=pymysql.cursors.DictCursor)	
	```
	> Some connector libraries support a single connection string of the form
		> jdbc:mysql://someuserid:somepassword@www.myurl.com:3306

4. Command for a structured Query Languages
	* Command and connections:
		* If it is an SQL/DB connection, the commands are SELECT, INSERT, ...
		* If it is a web/HTTP connection, the commands are GET, POST, ...
	* An SQL database is a sever that processes SQL commands and returns SQL data.
	* A web server is a server that processes HTTP commands and returns HTML/JSON, etc.

## 30 Jan 2020
----------
1. Error *UnicodeDecodeError: 'charmap' code can't decode byte 0x81* in **Python**
	* Specify the encoding when open files
		```Python
		with open(fn, "r", encoding='utf-8') as in_file:
		```
2. CSV Reading and Writing in **Python**
	* csv.DictReader(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel', *args, **kwds)
		* Create an object which operates like a regular reader but **maps the information read into a dict** whose keys are given by the optional fieldnames parameter. If the fieldnames parameter is omitted, the values in the **first row of the csv file** will be used as the fieldnames.
	```Python
	# Standard approach to opening a text file.
	with open(fn, "r", encoding='utf-8') as in_file:

	# Use the CSV Reader to simplify parsing the text file.
	# Delimiter specifies what separates the columns.
	# There are several other options, including whether or not quotes characters have a role.
	csv_rdr = csv.DictReader(in_file, delimiter=delimiter, quoting=csv.QUOTE_NONE)

	# For each row in the file.
	for r in csv_rdr:
	    try:
		if result == 0:
		    print("For file = ", fn)
		    print("The colums are: ", csv_rdr.fieldnames)
		    print("The first rows as a dict is: ", r)
		result = result + 1
	    except Exception as e:
		print("Could not read row, Exception = ", e)
		print("r = ", r)
		print("row count = ", result)
	```
		* quoting=csv.QUOTE_NONE indicates the *Quoting value for strings* .
		* Returns an object of type *csv.DictReader*
		* The object csv.DictReader is not subscriptable. Each element of this object is a *collections.OrderedDict* object. 
		* An OrderedDict is subscriptable. It is a dict that **remembers the order that keys were first inserted**. If a new entry overwrites an existing entry, the original insertion position is left unchanged. Deleting an entry and reinserting it will move it to the end.
		```Text
		OrderedDict([('nconst', 'nm0000001'), ('primaryName', 'Fred Astaire'), ('birthYear', '1899'), ('deathYear', '1987'), ('primaryProfession', 'soundtrack,actor,miscellaneous'), ('knownForTitles', 'tt0072308,tt005


		37,tt0043044,tt0050419')])


		OrderedDict([('nconst', 'nm0000002'), ('primaryName', 'Lauren Bacall'), ('birthYear', '1924'), ('deathYear', '2014'), ('primaryProfession', 'actress,soundtrack'), ('knownForTitles', 'tt0071877,tt0037382,tt0117057,tt0038355')])


		OrderedDict([('nconst', 'nm0000003'), ('primaryName', 'Brigitte Bardot'), ('birthYear', '1934'), ('deathYear', '\\N'), ('primaryProfession', 'actress,soundtrack,producer'), ('knownForTitles', 'tt0054452,tt0049189,tt0057345,tt0059956')])


		OrderedDict([('nconst', 'nm0000004'), ('primaryName', 'John Belushi'), ('birthYear', '1949'), ('deathYear', '1982'), ('primaryProfession', 'actor,soundtrack,writer'), ('knownForTitles', 'tt0077975,tt0072562,tt0080455,tt0078723')])


		OrderedDict([('nconst', 'nm0000005'), ('primaryName', 'Ingmar Bergman'), ('birthYear', '1918'), ('deathYear', '2007'), ('primaryProfession', 'writer,director,actor'), ('knownForTitles', 'tt0050976,tt0069467,tt0050986,tt0083922')])
		```
		* We can use OrderedDict[key_name] to get the specific value
		* The OrderedDict can be transfered to a dict by dict(OrderedDict)
		* My understanding is that csv.DictReader reads the csv file to an object csv.DictReader. Although csv.DictReader is not subscriptable, we may iterate over the rows of the csv file by iterating ove the csv.DictReader object. 
		```Python
		for r in csv_rdr:
		print(r)
		```
		* During iteration, each row is a OrderedDict object. Keys of the OrderedDict are the names of the columns
   
3. Several SQL usage showing up in *Lecture 1*
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

9. According to Lecture2,
	* **A data definition Statement Defines or "Creates" a definition/type of data, which is needed for DML.
	* **A data Manipulation Statement Creates, Retrieves, Updataes or Deletes Data.**

10. Almost all database engines and models have the concepts of
	* Objects that are some form of array of (name, value) pairs.
	* Sets of similar or related objects.
	* **Four basic (CRUD) operations** on a set
		* CREATE a new object and add to a set.
		* RETRIEVE an object in a set based on a criteria.
		* UPDATE an object in a set, e.g. change the data in the object.
		* DELETE an object from a set, specifying the object(s) by some criteria.

11. For example, in the file systems/CSV model-refers to first homework
	* A set is a file, e.g. People.csv.
	* Each object is a row in the file.
	* The header row gives the names of each column.
	* The CRUD processing involves writing a program that reads the file, changes the two-dimensional array and writing the file.
		* CREATE: Append a row and save the file.
		* RETRIEVE: Scan the table and apply some kind of IF statement.
		* UPDATE: Change a row in the two dimensional array.
		* DELETE: Remove a row from the array.
12. In the ***"pure" relational model***
	* A set is a relation.
	* An object is a row or tuple.
	* There is no support for CREATE, UPDATE or DELETE.
	* There is an *algebra* and language from producing a new relation from existing relations that implements a support set of RETRIEVE.

13. In SQL,
	* A set is a table.
	* An object is a row or tuple.
	* INSERT is the create operation.
	* UPDATE is the delete operation.
	* DELETE is the delete operation.
	* SELECT is the statement that realizes the relational algebra.
14. In the web (http) and [Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer) REST.
	* A set is a resource that is a collection of resources.
	* An object is a resource.
	* CREATE is HTTP POST
	* RETRIEVE is HTTP GET
	* UPDATE is HTTP PUT (or PATCH)
	* DELETE is HTTP DELETE.	

## 1 Feb 2020
----------
1. In Lecture, the topic is *Intro to Relational Model*
> Some knowledge from the notes
	* The set of allowed values for each attribute is called the **domain**
	* Attribute values are (normally) required to be **atomic**
	* NULL indicades that the value is unknow, it causes complications in the definition of many operations
	* **Relations are Unordered**, order of tuples is irrelevant(tuples may be stored in an arbitrary order

2. What does **atomicity**  mean in database?
  * Atomicity is one of the ACID (Atomicity, Consistency, Isolation, Durability) transaction properties. 
  * An atomic transaction is an indivisible and irreducible series of database operations such that either all occur, or nothing occurs.
  * Example: Transfer of funds from one account to another should either complete or not happen at all.

3. Database schema: the logical structure of the database

4. **Pure** Languages
	* Relational algebra
		* Consists of 6 basic operations
	* Tuple relational calculus
	* Domain relational calculus

5. Database instance: a snapshot of the data in the database at a given instant in time

6. Relational Algebra
> A procedural language consisting of a set of operations that take one or two relations as input and produce a new relation as their result
	* Six basic operations
		* select
		* project
		* union
		* set difference
		* cartesian product
		* rename


7.  Integrity: the  DDL includes commands for specifying integrity constraints. 
	* Types of integrity constraints
		* Primary key
		* Foreign key
		* not null
	* An example of creating a table
	```
	create table course(
		course_id  varchar(8),
		dept_name varchar(20), 
		credits numeric(2,0), 
		primary key (course_id), 
		foreign key (dept_name) references department);
	```
8. View definition: The DDL  includes commands for defining views. 

9. String Operation
> SQL includes a string-matching operator for comparisons on character strings.  The operator **like** uses patterns that are described using two special characters: 
	> percent ( % ).  The % character matches any substring. 
		* 'Intro%' matches any string beginning with “Intro”. 
		* '%Comp%' matches any string containing “Comp” as a substring. 
	> underscore ( _ ).  The _ character matches any character. 
		* '_ _ _' matches any string of **exactly** three characters. 
		* '_ _ _ %' matches any string of **at least** three characters
	* Find the names of all instructors whose name includes the substring “dar”. 
	```
	select name from instructor wherename like '%dar%' 
	```
	* Match the string “100%” 
	``` like '100 \%' escape  '\' 
	```
		* in that above we use backslash (\) as the escape character.
10. between comparison operator
	* For example, between 0---- and 100000 is >= 9000 and <=10000
	
11. Operation involves unknown
* and :
	> (trueand unknown)  = unknown,    
	> (falseand unknown) = false, 
	> (unknown andunknown) = unknown 
* or:    
	> (unknownortrue)   = true, 
	> (unknownorfalse)  = unknown 
	> (unknown orunknown) = unknown 

13. In this lecture, core concepts, relational algebra and SQL realization will be involved. **We will focus most on DML and DDL**.

14. SQL Parts
	* DML: All database have 4 constructs: ***CURD***
		* Create
		* Retreat
		* Update
		* Delete
	> Provides the ability to query information from the database and to insert tuples into, delete tuples from and modify tuples in the databse.
		* In SQL, it is composed of：
			* **Insert**
			* **Select**
			* **Update**
			* **Delete**
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
