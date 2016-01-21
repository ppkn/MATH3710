# Climate Change
ccf  
January 20, 2016  

This is an R Markdwon (Rmd) file for the climate change data shared in class.  We will attempt to translate some of the SAS code into R.


```r
climate <- read.table("~/Documents/MATH3710/climate/Climate.txt", header = TRUE)
climate$co2_cent <- climate$co2 - mean(climate$co2)
```


Above are simple summary statistics of the data.

Now let's look at a scatter plot of the data.


```r
plot(globaltemp ~ co2, data = climate, bg = "lightblue", 
     col = "black", cex = 1.5, pch = 21)
```

![](climate_files/figure-html/unnamed-chunk-2-1.png)\


Now we fit a linear model in R using the lm function. We will store it in a variable for more use.


```r
fit <- lm(globaltemp ~ co2, data = climate)
summary(fit) # displays a summary of the model.
```

```
## 
## Call:
## lm(formula = globaltemp ~ co2, data = climate)
## 
## Residuals:
##       Min        1Q    Median        3Q       Max 
## -0.201240 -0.080707 -0.004359  0.079892  0.208964 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -3.083756   0.182014  -16.94   <2e-16 ***
## co2          0.009532   0.000519   18.37   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.09203 on 53 degrees of freedom
## Multiple R-squared:  0.8642,	Adjusted R-squared:  0.8616 
## F-statistic: 337.3 on 1 and 53 DF,  p-value: < 2.2e-16
```

Here we will plot the data with confidence intervals too.  This is quite involved in R so the code is quite long.


```r
x <- climate$co2
y <- climate$globaltemp
plot(y~x,type="n")
m <- lm(y~x)
wx <- par("usr")[1:2]
new.x <- seq(wx[1],wx[2],len=100)
pred <- predict(m, new=data.frame(x=new.x), interval="conf")
lines(new.x,pred[,"fit"],lwd=2)
lines(new.x,pred[,"lwr"],lty=3)
lines(new.x,pred[,"upr"],lty=3)
points(x,y,pch=16,col="steelblue")
# now to add the prediction intervals
predp <- predict(m, new=data.frame(x=new.x), interval = "pred")
lines(new.x, predp[,"lwr"], lty=2)
lines(new.x, predp[,"upr"], lty=2)
title(main="Fit Plot for Temp")
```

![](climate_files/figure-html/unnamed-chunk-4-1.png)\


Now we will use R to check the assumptions of the model. First we will store the standardized residuals from "fit" and store them in a variable.


```r
e <- rstandard(fit)
summary(e)
```

```
##       Min.    1st Qu.     Median       Mean    3rd Qu.       Max. 
## -2.2180000 -0.8858000 -0.0484100 -0.0007238  0.8774000  2.3020000
```

```r
# plot residuals by co2
plot(climate$co2, e, bg = "lightblue", col = "black", pch = 21,
     xlab = "Co2", ylab = "Residuals", main = "Residuals vs Co2")
abline(h=0)
```

![](climate_files/figure-html/unnamed-chunk-5-1.png)\

More diagnostic plots of our model residuals:

```r
qqnorm(e)
qqline(e)
```

![](climate_files/figure-html/unnamed-chunk-6-1.png)\

```r
hist(e, breaks = 25, col = "green", freq = FALSE, xlim = c(-4,4), 
     main = "Histogram of Standardized Residuals", xlab = "rstandard")
```

![](climate_files/figure-html/unnamed-chunk-6-2.png)\
