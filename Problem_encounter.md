#### This file will record all problems I have encountered during programming

##### 8 Aug 2019

1. package tidyverse in **R**

```R
there is no package called ‘tidyverse’
```

2. Remove the last n characters from a list in **R**

```R
gsub('.{n}$', '', list_tobechanged)
```

3. range(1,n) in **R**
```R
seq(1,n-1)
```

4. ERROR *\`gap.degree\` parameter has length larger than 1* in **R**

First print out circos.par["gap.degree"] to check where is wrong
```R
circos.par["gap.degree"]
```
If the current_value is not equal to the default value, just set:
```R
circos.par(gap.degree = 1)
```

5. Error *plot.new() : figure margins too large* in **R**
```R
par(mar=c(1,1,1,1))
```

6. *Error in read.table(file = file, header = header, sep = sep, quote = quote, : first five rows are empty: giving up* in **R** and **PyThon**:

When output the dataframe value in a dictionary, only when print the dataframe will output nonempty .csv file. I do not know the reason.
```PyThon
for iter_key in dicHIER.keys():
    tempDF = dicHIER[iter_key]
    print(tempDF)
    (tempDF).to_csv('/home/penglab/Documents/dataSource/dfSET/HIER/'+str(iter_key)+'.csv')
```

 Also, no need to specify the tempDF as pd.DataFrame object, otherwise the output will still be empty.
 ```PyThon
    #WRong example
    pd.DataFrame(tempDF).to_csv('/home/penglab/Documents/dataSource/dfSET/HIER/'+str(iter_key)+'.csv')
```

7. *Error in col2rgb(col, alpha = TRUE) : invalid RGB specification* in **R**

The way to initialize a character object in R is to use c()
```R
col = c("#FFFFB3" , "#FF7F00", "#FF7F00" )
```










