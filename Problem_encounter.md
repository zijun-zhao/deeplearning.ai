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

5. Error *plot.new() : figure margins too large*
```R
par(mar=c(1,1,1,1))
```
