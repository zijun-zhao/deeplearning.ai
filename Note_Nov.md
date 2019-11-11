#### This file will record all problems I have encountered during programming


## 8 Nov 2019
----------
1. 'continue' not properly in loop in **Python**

  continue is only allowed within a **for** or **while** loop.

2. Remove an element from list in **Python**
```Python
T_list =  = ['no surfing', 'flippers']
T_list.remove('no surfing')
```

3. Remove certain elements from numpy.array in **Python**
```Python
a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
b = np.array([3,4,7])
c = np.setdiff1d(a,b)
```
The result will be [1, 2, 5, 6, 8, 9]


## 11 Nov 2019
----------
1. Swap two axis in an numpy array in **Python**
```Python
x = np.array([[1,2,3]])
np.swapaxes(x,0,1)
```
2. Padding array with 0 in **Python**
```Python
result = np.zeros(b.shape)
# actually you can also use result = np.zeros_like(b) 
# but that also copies the dtype not only the shape
result[:a.shape[0],:a.shape[1]] = a
```
