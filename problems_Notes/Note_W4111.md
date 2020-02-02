
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

## 28 Jan 2020
----------
1. Cread a dbuser for testing in order to run the following code:
```Python
%load_ext sql
%sql mysql+pymysql://dbuser:dbuserdbuser@localhost/w4111
```
Here, it is suggegsted by Prof. Ferguson that *we should create a user called dbuser, in the SQL installation as well. And the set it's password as dbuserdbuser. This way he can test it easily and we'd also be able to run the code too*. 

Solvement according to 
Click on the administration tab, then click on users and privileges on the left pane under the administration tab. There is button on the bottom that says add account

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

## 30 Jan 2020
----------
1. Error *UnicodeDecodeError: 'charmap' codec can't decode byte 0x81* in **Python**
 * Specify the encoding when open files
  ```Python
  with open(fn, "r", encoding='utf-8') as in_file:
  ```
2. CSV Reading and Writing in **Python**
 * csv.DictReader(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel', *args, **kwds)
   * Create an object which operates like a regular reader but **maps the information read into a dict** whose keys are given by the optional fieldnames parameter. If the fieldnames parameter is omitted, the values in the **first row of the csvfile** will be used as the fieldnames.
   ```Python
    # Standard approach to opening a text file.
    with open(fn, "r", encoding='utf-8') as in_file:
        
        # Use the CSV Reader to simplify parsing the the text file.
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
     OrderedDict([('nconst', 'nm0000001'), ('primaryName', 'Fred Astaire'), ('birthYear', '1899'), ('deathYear', '1987'), ('primaryProfession', 'soundtrack,actor,miscellaneous'), ('knownForTitles', 'tt0072308,tt0053137,tt0043044,tt0050419')])


     OrderedDict([('nconst', 'nm0000002'), ('primaryName', 'Lauren Bacall'), ('birthYear', '1924'), ('deathYear', '2014'), ('primaryProfession', 'actress,soundtrack'), ('knownForTitles', 'tt0071877,tt0037382,tt0117057,tt0038355')])


     OrderedDict([('nconst', 'nm0000003'), ('primaryName', 'Brigitte Bardot'), ('birthYear', '1934'), ('deathYear', '\\N'), ('primaryProfession', 'actress,soundtrack,producer'), ('knownForTitles', 'tt0054452,tt0049189,tt0057345,tt0059956')])


     OrderedDict([('nconst', 'nm0000004'), ('primaryName', 'John Belushi'), ('birthYear', '1949'), ('deathYear', '1982'), ('primaryProfession', 'actor,soundtrack,writer'), ('knownForTitles', 'tt0077975,tt0072562,tt0080455,tt0078723')])


     OrderedDict([('nconst', 'nm0000005'), ('primaryName', 'Ingmar Bergman'), ('birthYear', '1918'), ('deathYear', '2007'), ('primaryProfession', 'writer,director,actor'), ('knownForTitles', 'tt0050976,tt0069467,tt0050986,tt0083922')])
     ```
   * We can use OrderedDict[key_name] to get the specific value
   * The OrderedDict can be transfered to a dict by dict(OrderedDict)
   * My understanding is that csv.DictReader reads the csv file to an object csv.DictReader. Although csv.DictReader is not subscriptable, we can may iterate over the rows of the csv file by iterating ove the csv.DictReader object. 
    ```Python
    for r in csv_rdr:
    print(r)
    ```
   * During iteration, each row is a OrderedDict object. Keys of the OrderedDict are the names of the columns
   

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
  * Entity- Relationship data model (mainl for database design)
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
	> A database model is a **representation of a database** showing its design and structure and the manner in which data can be stored, organized, accessed and manipulated, which is *often presented visually by means of a diagram*
	> Entity Relationships Diagrams (ERDs) is one of the diagrams used in database modeling. Entity Relationships Diagrams (ERDs) is also known as an **Entity Relationship Model**, which is a **graphical representation** of entities (like people, objects, events, places, etc.) and how they relate to each other.
	> Database models are easy to review.
	> With the information from Entity Relationship Diagrams obtained from the conceptual layer, developers at the logical layer can determine how to **physically** design the database and the corresponding software application
	> Properties of table columns like data type, length of data stored therein, and default values of the columns are definedin a physical database model.
	> To sum up
		
