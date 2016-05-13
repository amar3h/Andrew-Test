<!-- rmarkdown v1 -->
---
Title: "Tooth Growth Analysis"
Author: "Andrew Marsh"
Date: "May 12, 2016"
output: html_document
---


```r
knitr::opts_chunk$set(echo = TRUE)
```
# Overview - 
Analyize Tooth Growth Data

# 1. Load the ToothGrowth data and perform some basic exploratory data analyses
Load Tooth Growth data into r and examine data


```r
toothGrowth <- ToothGrowth
head (toothGrowth )
```

```
##    len supp dose
## 1  4.2   VC  0.5
## 2 11.5   VC  0.5
## 3  7.3   VC  0.5
## 4  5.8   VC  0.5
## 5  6.4   VC  0.5
## 6 10.0   VC  0.5
```

```r
str(toothGrowth)
```

```
## 'data.frame':	60 obs. of  3 variables:
##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
```

# 2. Provide a basic summary and plot of the data.
Provide summaries (summary and table counts)

```r
summary(toothGrowth)
```

```
##       len        supp         dose      
##  Min.   : 4.20   OJ:30   Min.   :0.500  
##  1st Qu.:13.07   VC:30   1st Qu.:0.500  
##  Median :19.25           Median :1.000  
##  Mean   :18.81           Mean   :1.167  
##  3rd Qu.:25.27           3rd Qu.:2.000  
##  Max.   :33.90           Max.   :2.000
```

```r
toothGrowth$dose <- as.factor(toothGrowth$dose)
table(toothGrowth$supp, toothGrowth$dose)
```

```
##     
##      0.5  1  2
##   OJ  10 10 10
##   VC  10 10 10
```

```r
#Mean
mean(toothGrowth$len)
```

```
## [1] 18.81333
```

```r
#Variance
var(toothGrowth$len)
```

```
## [1] 58.51202
```

```r
#Standard Deviation
sd(toothGrowth$len)
```

```
## [1] 7.649315
```
# Plot of effectiveness of dosage/type

```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.2.4
```

```r
plot <- ggplot(ToothGrowth, 
               aes(x=factor(dose),y=len,fill=factor(dose)))
plot + geom_boxplot(notch=F) + facet_grid(.~supp) +
     scale_x_discrete("Dosage (Milligrams)") +   
     scale_y_continuous("Length of Teeth") +  
     ggtitle("Effect of Dosage & Type on Tooth Growth")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)
# Plot of effectiveness of type

```r
plot <- ggplot(toothGrowth, 
               aes(x=factor(supp),y=len,fill=factor(supp)))
plot + geom_boxplot(notch=F) + facet_grid(.~supp) +
     scale_x_discrete("Dosage (Milligrams)") +   
     scale_y_continuous("Length of Teeth") +  
     ggtitle("Effect of Type on Tooth Growth")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)
# 3. Use confidence intervals and/or hypothesis tests to compare tooth growth by supp and dose. (Only use the techniques from class, even if there's other approaches worth considering) - First by type

```r
#Compare supplement
VC<-subset(toothGrowth,supp=='VC')
OJ<-subset(toothGrowth,supp=='OJ')
t.test(VC$len, OJ$len)$conf.int
```

```
## [1] -7.5710156  0.1710156
## attr(,"conf.level")
## [1] 0.95
```
# Dosage Tests

```r
# Test for dosage
Dose5 <- subset(toothGrowth, dose=="0.5")
Dose1 <- subset(toothGrowth, dose=="1")    
Dose2 <- subset(toothGrowth, dose=="2")  

# 0.5 and 1
t.test(Dose5$len, Dose1$len)$conf.int
```

```
## [1] -11.983781  -6.276219
## attr(,"conf.level")
## [1] 0.95
```

```r
# 1 and 2
t.test(Dose1$len, Dose2$len)$conf.int
```

```
## [1] -8.996481 -3.733519
## attr(,"conf.level")
## [1] 0.95
```

```r
# 0.5 and 2
t.test(Dose5$len, Dose2$len)$conf.int
```

```
## [1] -18.15617 -12.83383
## attr(,"conf.level")
## [1] 0.95
```
# 4. State your conclusions and the assumptions needed for your conclusions.
# Conclusions and Assumptions
    - The 95% difference contains 0, so the supplement types are not signficant to an alpha of .05.
    - All 3 dosage comparisons of dosage type (0.5 vs 1, 0.5 vs 2 and 1 vs 2) are shown to be 
            significanly different as all 3 intervals do not contain 0 at the alpha = .05 level.
            As dosage increases of either type tooth length is shown to be larger as well.
    - We assume variance between the various subsamples are equal and these are not paired samples.
    
