

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
8. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)
8. [Lecture10-29Mar&3Apr](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture10_Index&QueryProcessing.md)


* Course Modules 
  * 
  * 
  
## 10 Apr 2020


1. Recall
              ![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_1.png)
* Execution Engine: Runs query quickly
* Index/file/recored manager: locate tuples and rows quickly
* Buffer&Buffer manager: optimize transfer between memory and disk, also use caching to guarantee block access in memory.
* Control access assures the multiple users/programs do not accidently overwrite/corrupt data.
* Durability ensures that when a transaction commits, update is not lost.
2. Recall **



![Image of Yaktocat](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/imgs/10Apr_2.jpg)
* After the SQL query comes in text, it get parsed and translated to some executable languages(similar to the compliling). The internal expression is transfered in the relational algebra expression.CPU will understand how to deal with it using Optimizer. The optimizer pools in statistics of information to decide how to execue it.
For example, there are two ways to implement σsalary<75000(instructor)
Scan the instructor table and then find everything that matches
Use index, use the refinement first and do the scan next
    
    
    
    
    
    
    
    
    
    
    
    
    
