# Repairs
Cody Frisby  
December 15, 2015  



Let's get started with R.  This first data set is small so we have the data loaded into two variables: minutes and units.  We then put them together with the cbind function and store the result in a new variable called repair.


```r
units <- c(1,2,3,4,4,5,6,6,7,8,9,9,10,10,3)
minutes <- c(23,29,49,64,74,87,96,97,109,119,149,145,154,166, NA)
repair <- as.data.frame(cbind(units, minutes))
head(repair)
```

```
##   units minutes
## 1     1      23
## 2     2      29
## 3     3      49
## 4     4      64
## 5     4      74
## 6     5      87
```

```r
describe(repair)
```

```
##         vars  n  mean    sd median trimmed   mad min max range  skew
## units      1 15  5.80  2.96    6.0    5.85  4.45   1  10     9  0.01
## minutes    2 14 97.21 46.22   96.5   97.67 59.30  23 166   143 -0.09
##         kurtosis    se
## units      -1.48  0.76
## minutes    -1.37 12.35
```

Above is a summary of the data table "repair".


```r
cor(repair, method = "pearson", use = "complete.obs")
```

```
##             units   minutes
## units   1.0000000 0.9936987
## minutes 0.9936987 1.0000000
```

Now let's plot the data look at the variable minutes being modeled as a function of units.

![](repair_files/figure-html/unnamed-chunk-4-1.png) 

Below is a plot of the residuals vs fitted, Normal Quantile, Scale-Location, Residuals vs Leverage.


```r
par(mfrow=c(2,2)) #setup your window to fit the next plots
plot(m)
```

![](repair_files/figure-html/unnamed-chunk-5-1.png) 
