#### This file will record all problems I have encountered during programming


## 6 Jan 2020
----------
1.  isin() in **Python**

## 8 Jan 2020
----------
1. Update dictionary value in **Python**
```Python
my_dict.update({'key1': 'value1', 'key2': 'value2'})
```
## 18 Jan 2020
----------
1. How to sort a dataframe in **Python**
```Python
df.sort_values('columnname', ascending=False)
```

2. Delelte the last character for every element in a list in **Python**
```Python
newtest = [x[:-1] for x in test]
```

3. Make enumerate starts from 1, not 0 in **Python**
```Python
for i,r_id in enumerate(List,1):
```

## 19 Jan 2020
----------
1. Converting a list to dictionary with list elements as keys in dictionary and values are enumerated index starting from 1 in **Python**
```Python
dictOfWords = { listOfStr[i] : i for i in range(1, len(listOfStr)+1 ) }
```

## 20 Jan 2020
----------
1. Save figure with background color in **Python**
```Python
fig.savefig('whatever.png', facecolor=fig.get_facecolor(), edgecolor='none') 
# specifying the edgecolor here because the default edgecolor for the actual figure is white, which will give you a white border around the saved figure
```

2. Set figure with background color in **Python**
```Python
ax.set_facecolor((1.0, 0.47, 0.42))
```

## 29 Jan 2020
----------
1. Check if a Python list item contains a string inside another string in **Python**
```Python
matching = [s for s in some_list if "abc" in s]
```

2. Extract value from Pandas series in **Python**:

```Python
series.values
```
3. Scatter plot with text above the point in ** Python**

```Python
y = np.arange(0,21/2, 0.5)
z = np.arange(0,42*5,10)


fig = plt.figure(figsize=(20, 2))
ax = fig.add_subplot(111)
ii = 0
for i in related_region_name:
    plt.scatter(z[ii], y[ii])
    ii = ii+1
#namelist has the same lenth with y and z
for i, txt in enumerate(namelist):
    ax.annotate(txt, (z[i], y[i]))
```
