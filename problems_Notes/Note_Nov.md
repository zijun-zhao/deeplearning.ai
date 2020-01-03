#### This file will record all problems I have encountered during programming


## 8 Nov 2019
----------
1. 'continue' not properly in loop in **Python**
   ```
    continue is only allowed within a **for** or **while** loop.
   ```


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

## 12 Nov 2019
----------
1. Set figure size in **Python**
```Python
from matplotlib import pyplot as plt
plt.figure(figsize=(1,1))
x = [1,2,3]
plt.plot(x, x)
plt.show()
```
2. Color choice in matplotlib in **Python**
```Python
'b', 'g', 'r', 'c', 'm', 'y', 'k', 'w'
```
3. Cancel a local commit
```Shell
git reset --soft HEAD^
```



## 19 Nov 2019
----------
1. Seaborn.countplot : order categories by count in **Python**
```Python
sf = ns.features['soma_features'].region
ct = sf["Region"].value_counts().sort_values(ascending=False)
thre = 0
sns.set(rc={'figure.figsize':(11.7,15)})
_ = sns.countplot(y="Region", 
                  data=sf.loc[sf.Region.isin(ct.index[ct>thre])],order = ct.index
                 )
```
2. add a string prefix to each value in a string column in **Python**
```Python
df['col'] = 'str' + df['col'].astype(str)
```

## 20 Nov 2019
----------
1. add subplot title **Python**
```Python
fig = plt.figure()
ax1 = fig.add_subplot(221)
ax2 = fig.add_subplot(222)
ax3 = fig.add_subplot(223)
ax4 = fig.add_subplot(224)
ax1.title.set_text('First Plot')
ax2.title.set_text('Second Plot')
ax3.title.set_text('Third Plot')
ax4.title.set_text('Fourth Plot')
plt.show()
```
2. Drop duplicate rows in **Python**
```Python
DataFrame.drop_duplicates(self, subset=None, keep='first', inplace=False)
```
3. Change column's data type in pandas in **Python**
```Python
df['col'] = 'str' + df['col'].astype(str)
```
4. Unpack the first two elements in list/tuple
```Python
a, b, *ignore = ....
```
5. Change data type of specific column in pandas in **Python**
```Python
df['colname'] = df['colname'].astype(int)
```


## 24 Nov 2019
----------
1. subprocess.Popen  in **Python**
   * subprocess.Popen does not wait for a command to complete before returning. 
2. tdout and stderr arguments in subprocess.run() in **Python**
   * The standard input and output channels for the process started by subprocess.run() are bound to the parent’s input and output. That means *the calling program cannot capture the output of the command*. Pass PIPE for the stdout and stderr arguments to capture the output for later processing.
```Python
import subprocess
completed = subprocess.run(
    ['ls', '-1'],
    stdout=subprocess.PIPE,
)
print('returncode:', completed.returncode)
print('Have {} bytes in stdout:\n{}'.format(
    len(completed.stdout),
    completed.stdout.decode('utf-8'))
)
```
   * Note that **passing check=True and setting stdout to PIPE is equivalent to using check_output()**.
  
3. check=True in subprocess.run() in **Python**
   * If check is true, and the process exits with a non-zero exit code, a CalledProcessError exception will be raised.*
4. 
