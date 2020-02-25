
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

  
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
2. In lecture3, **Entity Relationship Model*
	* Models an enterprise as a collection of entities and relationships
	* Employs three basic concepts
		* Entity sets
			* A set of entities of the same type which share the same properties
		* Relationship sets
		* Attributes
			* A subset of the attributes form a primary key of the entity set. A combination of cols that has unique values, most of the time it will be a single column

3. Helpful online notes [Ehi Kioya's Notes](https://ehikioya.com/conceptual-logical-physical-database-modeling/) for Lecture3 Entity-Relationship Modelling
	* A useful picture
	![Levels of Data Modeling](https://ehikioya.com/wp-content/uploads/2019/02/Understanding-Conceptual-Logical-And-Physical-Database-Modeling.jpg)
	* Quotes from the above blog
	> * A database model is a **representation of a database** showing its design and structure and the manner in which data can be stored, organized, accessed and manipulated, which is *often presented visually by means of a diagram*
	> * Entity Relationships Diagrams (ERDs) is one of the diagrams used in database modeling. Entity Relationships Diagrams (ERDs) is also known as an **Entity Relationship Model**, which is a **graphical representation** of entities (like people, objects, events, places, etc.) and how they relate to each other.
	> * Database models are easy to review.
	> * With the information from Entity Relationship Diagrams obtained from the conceptual layer, developers at the logical layer can determine how to **physically** design the database and the corresponding software application
	> * Properties of table columns like data type, length of data stored therein, and default values of the columns are defined in a physical database model.
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

5. Degree of a relationship set
	* Binary relationship
		* One to one
		* One to many
		* Many to one
		* Many to many
	* Non-binary relationship
6. Attribute types
	* Simple attributes
	* Composite attributes
		> Allows us to divide attributes into subparts(other attributes)
		* For example, Name may contain first name, middle initial and last name. 
		* Another typical example of a composite attribute is a person’s address, which is composed of atomic attributes, such as City, Zip, and Street.
	* Single-valued
	* Multivalued attribute
		* Phone_numbers. Each person can have multiple phone numbers
		
		
7. Mapping cardinality constratins is most useful in describing binary relationship sets

8. Two mechenisims during modelling
	* Top-down
		* Progressive refinement, do not know what the data is
	* Bottom up
		* Have the data, do not know the data. Trying to figure out the underly schema and how to put it to the relational model.

	
9. There is almost never an optimal version for data modeling
	* When I go to a physical model, there should be no many to many
	
10. 
	> A relationship set is a mathematical **relation** among n>=2 entites

11. Quesetion from student: what is the difference between relation and database:
* Answer by Prof.Ferguson   :
	> **An Entity (class) is the ER representation of what the Relational Model calls a Relation. Everything in the Relational Model is a relation.**
	> **A relation in ER maps to either a foreign key constraint or a table implementing the relation pattern.**

12. Prof. Ferguson  
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
3. When use "from", Prof. Ferguson suggests to only select from one table.
4. String Operation
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
5. between comparison operator
	* For example, between 0---- and 100000 is >= 9000 and <=10000
	
6. Operation involves unknown
* and :
	> (trueand unknown)  = unknown,    
	
	> (falseand unknown) = false, 
	
	> (unknown andunknown) = unknown 
* or:    
	> (unknownortrue)   = true, 
	
	> (unknownorfalse)  = unknown 
	
	> (unknown orunknown) = unknown 
7. null does not equal null. We check null by "is null" or "is not null"

8. 
* Predicates in the **having** clause are applied **after** the formation of groups 
	``` 
	select dept_name, avg(salary) asavg_salary 
	from instructor 
	group by dept_name having avg(salary) > 42000
	```
* predicates in the **where** caluse are applied **before** the formation of tables








## 5 Feb 2020
----------
1. Do not consider NULL entries when calculating average
	* We can use avg(col_name)
	* We can't use col_value/count(col_value)

2. Syntax
	* When order by multiple things, **use comma to separate them, not 'and'**
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
	* An example to find all courses taught in both the fall 2017 and in the Spring 2018 semester
	```
	select course_id
	from section as S
	where semester = 'Fall' and year = 2017 and
		exist( select*
			from section as T
			where semester = 'Spring' and year = 2018
				and S.course_id = T.course_id);
	```
		* exist means 'is the table empty'. Given these parameter, go and look another table and see whetther something exist. Given all the courses in thie semester, back on the same table, see whether itis also on the other semester. If this is not empty, then it is taught in the other semester.
		* Note: relational algebra is close under all operators.

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


