#### This file will record all problems I have encountered during programming


## 9 Oct 2019
----------
1. Set the seaborn plot's size  in **Python**
```Python
import seaborn as sns
sns.set(rc={'figure.figsize':(wid,len)})
```

## 11 Oct 2019
----------
1. *extent* parameter in matplotlib.pyplot.imshow() in **Python**
```Python
extent = (0, xsize * xspace, ysize * yspace, 0)
ax.imshow(nda, cmap="Greys", alpha=0.2, extent=extent)
```
extent: [ None | (x0,x1,y0,y1) ]

If origin is not None, then extent is interpreted as in matplotlib.pyplot.imshow(): it gives the **outer pixel boundaries**. In this case, the position of Z[0,0] is the center of the pixel, not a corner. If origin is None, then (x0, y0) is the position of Z[0,0], and (x1, y1) is the position of Z[-1,-1].


## 15 Oct 2019
----------
1. To **repeat each element** n times use ***np.repeat*** in **Python**
```Python 
np.repeat(data, 5)
array([-50, -50, -50, -50, -50, -40, -40, -40, -40, -40, -30, -30, -30,
       -30, -30, -20, -20, -20, -20, -20, -10, -10, -10, -10, -10,   0,
         0,   0,   0,   0,  10,  10,  10,  10,  10,  20,  20,  20,  20,
        20,  30,  30,  30,  30,  30,  40,  40,  40,  40,  40])
```
2. To **repeat the array** n times use ***np.tile*** in **Python**
```Python 
np.tile(data, 5)

array([-50, -40, -30, -20, -10,   0,  10,  20,  30,  40, -50, -40, -30,
       -20, -10,   0,  10,  20,  30,  40, -50, -40, -30, -20, -10,   0,
        10,  20,  30,  40, -50, -40, -30, -20, -10,   0,  10,  20,  30,
        40, -50, -40, -30, -20, -10,   0,  10,  20,  30,  40])
```
## 18 Oct 2019
----------
1. To sort an array in **Python**
```Python 
numpy.sort(a, axis=-1, kind=None, order=None)
```
The default value for *axis* is -1, which sorts along the last axis. If *None*, the array is flattened before sorting. 

2. To **convert an array to list** in **Python**
```Python 
numpy.ndarray.tolist()
```

3. Usage of **np.arange** in **Python**
```Python 
Input: np.arange(3,7,2)
Output: array([3, 5])
```

4. **Store coordinates to tuple** in **Python**
```Python
coords = []
coordi=[]
coords.append([x,y,z])
coordi.append((x,y,z))
 ```
5. Usage of *scipy.spatial.distance.* **cdist** in **Python**
```Python 
a = np.array([[0, 0, 0],
               [0, 0, 1],
               [0, 1, 0],
               [0, 1, 1],
               [1, 0, 0],
               [1, 0, 1],
               [1, 1, 0],
               [1, 1, 1]])
b = np.array([[ 0.1,  0.2,  0.4]])
distance.cdist(a, b, 'cityblock')
```
  * Here when the metric input is *sqeuclidean*, it will calculate the squared distance
  * cdist will return a m<sub>A</sub> by m<sub>B</sub> distance matrix. For each i and j, the metric dist(u=XA[i],V=XB[j]) is computed and stored in the ij-th entry.
  
6. Rename a column in pandas in **Python**
```Python 
df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'}, inplace=True)
```
7. Set value for elements inside a single column in pandas in **Python**
```Python 
#In this example, requirement is =='b', column is 'c', the value to set is 'D'
df.loc[df['A']=='b','C'] = 'D'
```
## 19 Oct 2019
----------
1. *Unknown projection '3d'* in **Python**
```Python 
from mpl_toolkits.mplot3d import Axes3D
```
2. Rotate a 3D plot using **Jupyter notebook**
```Python 
%matplotlib notebook
```
Be put in the top of that cell

3.*The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()* in **Python**
```Python 
mask = ((r["dt"] >= startdate) & (r["dt"] <= enddate))
```
Use '&' to connect


## 21 Oct 2019
----------
1. Jupyter notebook's output too large to open in **Jupyter Notebook**
```Python
pip install nbstripout
nbstripout file.ipynb
```
## 30 Oct 2019
----------
1. *color kwarg must have one color per dataset* in **Python**

Make sure the input is one-dimension

2. Modified column data type in **Python**
```Python
df['col_test'] = df.col_test.astype(float)
```
3. Check the data type of a certain column in **Python**
```Python
df['col_test'].dtype
```
