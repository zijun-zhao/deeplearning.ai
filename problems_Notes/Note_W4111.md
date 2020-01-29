#### This file will record all problems I have encountered during studying the course W4111 in 20220 Spring.


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
