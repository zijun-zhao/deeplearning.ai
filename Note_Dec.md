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
6. A simple categorical heatmap in **Python**, cited from https://matplotlib.org/3.1.1/gallery/images_contours_and_fields/image_annotated_heatmap.html
```Python
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
# sphinx_gallery_thumbnail_number = 2

vegetables = ["cucumber", "tomato", "lettuce", "asparagus",
              "potato", "wheat", "barley"]
farmers = ["Farmer Joe", "Upland Bros.", "Smith Gardening",
           "Agrifun", "Organiculture", "BioGoods Ltd.", "Cornylee Corp."]

harvest = np.array([[0.8, 2.4, 2.5, 3.9, 0.0, 4.0, 0.0],
                    [2.4, 0.0, 4.0, 1.0, 2.7, 0.0, 0.0],
                    [1.1, 2.4, 0.8, 4.3, 1.9, 4.4, 0.0],
                    [0.6, 0.0, 0.3, 0.0, 3.1, 0.0, 0.0],
                    [0.7, 1.7, 0.6, 2.6, 2.2, 6.2, 0.0],
                    [1.3, 1.2, 0.0, 0.0, 0.0, 3.2, 5.1],
                    [0.1, 2.0, 0.0, 1.4, 0.0, 1.9, 6.3]])


fig, ax = plt.subplots()
im = ax.imshow(harvest)

# We want to show all ticks...
ax.set_xticks(np.arange(len(farmers)))
ax.set_yticks(np.arange(len(vegetables)))
# ... and label them with the respective list entries
ax.set_xticklabels(farmers)
ax.set_yticklabels(vegetables)

# Rotate the tick labels and set their alignment.
plt.setp(ax.get_xticklabels(), rotation=45, ha="right",
         rotation_mode="anchor")

# Loop over data dimensions and create text annotations.
for i in range(len(vegetables)):
    for j in range(len(farmers)):
        text = ax.text(j, i, harvest[i, j],
                       ha="center", va="center", color="w")

ax.set_title("Harvest of local farmers (in tons/year)")
fig.tight_layout()
plt.show()
```

7. Modified font size in matplotlib
```Python
font = {'family' : 'normal',
        'weight' : 'bold',
        'size'   : 22}

matplotlib.rc('font', **font)
```

8. Modified figure size in matplotlib
```Python
from matplotlib.pyplot import figure
figure(num=None, figsize=(8, 6), dpi=80, facecolor='w', edgecolor='k')
```

