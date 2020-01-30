#### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.


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
  * Magic commands are a set of convenient functions in Jupyter Notebooks that are designed to solve some of the common problems in standard data analysis. IPython SQL magic extension makes it possible to write SQL queries directly into code cells as well as read the results straight into pandas DataFrames. 
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
2. CSV Reading and Writing in Python
 * csv.DictReader(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel', *args, **kwds)
  * Create an object which operates like a regular reader but **maps the information read into a dict whose keys are given by the optional fieldnames parameter**. If the fieldnames parameter is omitted, the values in the **first row of the csvfile** will be used as the fieldnames.
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
  * Returns an object of type *csv.DictReader*
  * The object csv.DictReader is not subscriptable. Each element of this object is a *collections.OrderedDict* object. An OrderedDict is a dict that **remembers the order that keys were first inserted**. If a new entry overwrites an existing entry, the original insertion position is left unchanged. Deleting an entry and reinserting it will move it to the end.
