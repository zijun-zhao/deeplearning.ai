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
    > A collection of conceptual tools for describing data, data relationships, data semantics, and consistency constraints
  > Data abstraction
    > Hide the complexity of data structures to represent data in the database from users through several levels of data abstraction
   
4. Data Models
  * Relational model
    > Only has entities, do not have functions
  * Entity- Relationship data model (mainl for database design)
  * Object-based data models(Object-oriented and Object-relational)
    > has function & method
  * Semi-structured data model(XML) 
    > be replaced by json
  * Other older models
    * Network model
    * Hierarchical model
5. Relational Model -> Table, collumn, Row
  * All the data is stored in various tables
    > All data stored in relation(table)
    * A row: an entity
    * A column: all properties of entity
  * **Relationships are represented by shared columns and values**
6. Levels 
   
   
