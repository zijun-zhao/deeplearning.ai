#### This file will record all problems I have encountered during programming


## 8 Dec 2019
----------
1. warning *No parallel backend registeredexecuting %dopar% sequentially: no parallel backend registered* in **R**

  * the warning message which indicates that the loop ran sequentially. To execute your code in parallel, beside using dopar, you have to register parallel backend.
 
 Specifically，**register a parallel backend using library(doParallel) or library(parallel)**
```R
library(doParallel)
library(foreach)
### Register parallel backend
cl <- makeCluster(detectCores())
registerDoParallel(cl)
getDoParWorkers()
#insert code here
### Stop cluster
stopCluster(cl)
```
  
2.  could not find function "registerDoParallel" in **R**

3.  warning *closing unused connection 10 (<-LAPTOP-T31BM162:11371)* in **R**

4. **names** in **R**
```R
Functions to get or set the names of an object.
```
5. count number of occurance of element inside a list in **Python**
```Python
list.count(x)
```
