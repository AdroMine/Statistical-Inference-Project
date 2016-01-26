# ToothGrow Analysis
Enelen Brinshaw  
January 22, 2016  

## Overview
This report analyses the effect of different doses and methods of delivery of vitamin C in guinea pigs' odontoblasts (cells responsible for tooth growth).   






## Exploratory Analysis
Let us first explore the dataset to see the general trends.  

```r
data("ToothGrowth")
df <- ToothGrowth
df$dose <- as.factor(df$dose)
require(ggplot2)
ggplot(ToothGrowth,aes(dose,len,colour = supp)) +
     geom_point() + 
     geom_smooth() +
     scale_colour_discrete(name = "Delivery Method", 
                           labels = c("Orange Juice (OJ)", "Ascorbic Acid (VC)")) + 
     labs(x = "Dose (mg/day)", y = "Length", 
          title = "Response in Length of guinea pigs to Vitamin C")
```

![](ToothGrowth_Analysis_files/figure-html/basic-1.png)\

There is a clear advantage of giving the dose via orange juice rather than ascorbic acid, although the advantage is not present for the highest dose of 2mg/day. Let us look at a few more graphs to gain more understanding.  

```r
ggplot(df,aes(supp,len,group = supp,fill = supp)) +
     geom_boxplot() +
     facet_wrap(facets = ~dose,labeller = doses) +
     scale_fill_discrete(name = "Delivery Method", 
                           labels = c("Orange Juice (OJ)", "Ascorbic Acid (VC)")) + 
     labs(y = "Length", x = "", 
          title = "Response in length by delivery method for different doses")
```

![](ToothGrowth_Analysis_files/figure-html/boxplot1-1.png)\

Again we can see Orange juice seems to better than Ascorbic acid except at the highest dose.  

## Statistical tests of significance

Let us now confirm whether these differences are significant or due to luck by conducting t-tests, and developing confidence interval for the differences.   

```r
res <- t.test(df$len[df$supp == "OJ"], df$len[df$supp == "VC"])
require(pander)
htest(res)
```


------------------------------------------------------
 df    Test Statistic   p-Value   Confidence Interval 
----- ---------------- --------- ---------------------
55.31      1.915        0.06063  -0.171 $\quad$ 7.571 
------------------------------------------------------

Table: Welch Two Sample t-test: `df$len[df$supp == "OJ"] and df$len[df$supp == "VC"]`
    

The test fails at 95%, but considering there was no difference at the highest dose, this is quite possible. Let us redo the tests for each dose.      
   
   


```r
k <- split(df, f = df$dose)
tests <- lapply(k, function(x) t.test(x$len[x$supp == "OJ"], x$len[x$supp == 
    "VC"]))
lapply(tests, htest)
```

```
## $`0.5`
## [1] "\n------------------------------------------------------\n df    Test Statistic   p-Value   Confidence Interval \n----- ---------------- --------- ---------------------\n14.97       3.17       0.006359   1.719 $\\quad$ 8.781 \n------------------------------------------------------\n\nTable: Welch Two Sample t-test: `x$len[x$supp == \"OJ\"] and x$len[x$supp == \"VC\"]`\n\n"
## attr(,"class")
## [1] "knit_asis"
## attr(,"knit_cacheable")
## [1] TRUE
## 
## $`1`
## [1] "\n------------------------------------------------------\n df    Test Statistic   p-Value   Confidence Interval \n----- ---------------- --------- ---------------------\n15.36      4.033       0.001038   2.802 $\\quad$ 9.058 \n------------------------------------------------------\n\nTable: Welch Two Sample t-test: `x$len[x$supp == \"OJ\"] and x$len[x$supp == \"VC\"]`\n\n"
## attr(,"class")
## [1] "knit_asis"
## attr(,"knit_cacheable")
## [1] TRUE
## 
## $`2`
## [1] "\n------------------------------------------------------\n df    Test Statistic   p-Value   Confidence Interval \n----- ---------------- --------- ---------------------\n14.04     -0.04614      0.9639   -3.798 $\\quad$ 3.638 \n------------------------------------------------------\n\nTable: Welch Two Sample t-test: `x$len[x$supp == \"OJ\"] and x$len[x$supp == \"VC\"]`\n\n"
## attr(,"class")
## [1] "knit_asis"
## attr(,"knit_cacheable")
## [1] TRUE
```



