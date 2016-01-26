StatsInference
========================================================
author: Enelen Brinshaw
date: 

First Slide
========================================================


Now we are going to simulate exponential Distribution.   

```r
lambda = 0.2
mns <- vr <- vector()
for(i in 1:1000){
temp <- rexp(40,lambda)
mns <- c(mns,mean(temp))
vr <- c(vr,var(temp))
}

dat = data.frame(means = mns, variance = vr)
```



Distribution of means
========================================================

![plot of chunk unnamed-chunk-2](StatsInference-figure/unnamed-chunk-2-1.png)

Slide With Plot
========================================================

![plot of chunk unnamed-chunk-3](StatsInference-figure/unnamed-chunk-3-1.png)
