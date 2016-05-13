<!-- rmarkdown v1 -->
---
Title: "Comparison of the Exponential Distribution and Central Limit Theorem in R"
Author: "Andrew Marsh"
Date: "May 12, 2016"
output: html_document
---


```r
knitr::opts_chunk$set(echo = TRUE)
```
# Overview - 
In this project you will investigate the exponential distribution in R and compare it with the Central Limit Theorem. The exponential distribution can be simulated in R with rexp(n, lambda) where lambda is the rate parameter. The mean of exponential distribution is 1/lambda and the standard deviation is also 1/lambda. Set lambda = 0.2 for all of the simulations. You will investigate the distribution of averages of 40 exponentials. Note that you will need to do a thousand simulations.

# Simulations
Illustrate via simulation and associated explanatory text the properties of the distribution of the mean of 40 exponentials. Set up data and plot using a histogram.


```r
    lambda = 0.2
    n = 40
    set.seed(25)
    nbrExponentials = 1:1000
    means <- data.frame(x = sapply(nbrExponentials, function(x) {mean(rexp(n, lambda))}))
    colnames(means) <- "sampleMean"
    means$sampleMean <- as.numeric(means$sampleMean)
    hist(means$sampleMean,col = "darkgreen",xlab = "Sample Means with lambda = 0.2", main="Simulation Means 
        (1000 Simulations with n=40)")
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png)

#1. Show the sample mean and compare it to the theoretical mean of the distribution.
        Sample Mean = 4.9986

```r
# Sample Mean of Simulations
     mean(means$sampleMean)
```

```
## [1] 4.998511
```
        Expected Mean = 5    


```r
# Expected mean
    (1/lambda)
```

```
## [1] 5
```
#Comparison of Means
Again,the sample mean from the above simulation is 4.9986. The expected mean is 5. The mean of exponential distribution is quite close to the expected mean (which is 1/£f).

#2.Show how variable the sample is (via variance) and compare it to the theoretical variance of the distribution.
        Sample variance =  0.6113794

```r
# Sample Variance of our Distribution =  0.6113794
    var(means$sampleMean)
```

```
## [1] 0.6113794
```
        Expected Variance = .625    

```r
# Expected variance = 
  ev <- ((1/lambda)/sqrt(40))^2
  ev
```

```
## [1] 0.625
```
#Comparison of Variances
Again,the sample variance from the above simulation is 0.6113794. The expected variance is .625. The variance of exponential distribution is quite close to the expected variance (which is ((1/£f)/sqrt(n))^2.

#3.Show that the distribution is approximately normal.
    We will use ggplot2 to show the distribution as approimately normal and indeed it is based upon the plot.
             

```r
   library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.2.4
```

```r
    distPlot <- ggplot(data = means, aes(sampleMean)) + 
                geom_histogram(aes(y=..density..), fill = "darkgreen", binwidth = 0.30, col = "black") 
    distPlot + stat_function(fun = dnorm, args = list(mean = 5, sd = sqrt(ev)),col="purple")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)
    The distribution appears approimately normal based upon the plot as the histogram mostly follows the distribution
    of the bell shaped curve created by the expected mean 5 and expected variance of .625 (standard deviation = the
    square root of .625)
