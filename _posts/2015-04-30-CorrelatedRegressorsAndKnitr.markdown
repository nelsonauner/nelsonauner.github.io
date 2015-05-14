---
layout: post
title:  "Correlated Regressors and R Markdown"
subtitle: ""
picture: "/images/createSampleData-2.png"
pwidth: "250"
pheight: "200"
categories: R
long: "T"
---
##Regression of correlated variables

Given linear model

$[V_{1},V_{2}] \sim N(\mu,\Sigma)$

With $cov(V_1,V_2)=.9$, $var(V_1) = var(V_2) = 1$

$V_3 \sim N(V_2,.5)$

$Y \sim N(V_1 + V_2,1)$

###Let's say we only know that $V1$ and $V2$ are correlated predictors of $Y$

###Questions:

1. Is it informative/safe to look at linear model coeffecients? 

2. Is ANOVA better?

3. Does running a linnear model on the residuals of a first stage model help?

4. How can we measure results in terms of false postives vs. false negatives? 


------------

###Approach:

First, we create sample data

```r
library(MASS)
library(magrittr)
set.seed(5000)
sampleData <- mvrnorm(n = 20, mu=c(5,0), Sigma = matrix(c(1,.9,.9,1),nrow = 2), tol = 1e-6, empirical = FALSE, EISPACK = FALSE) %>% as.data.frame
sampleData$e <- rnorm(20)
sampleData$e2 <- rnorm(20,sd=.5)


sampleData <- transform(sampleData, Y=  V1 + V2 + e)
sampleData <- transform(sampleData, V3 = V1+e2)

with(sampleData,
     plot(Y,V1))

with(sampleData,
     lines(Y,V2,pch="*"))
```

![plot of chunk createSampleData](images/createSampleData-1.png) 

```r
with(sampleData,
     plot(V2,V1))
```

![plot of chunk createSampleData](images/createSampleData-2.png) 

And inspect linear model


```r
fullModel <- lm(Y ~ V1 + V2 +V3, data=sampleData)
v1Model <- lm(Y ~ V1, data=sampleData)
v2Model <- lm(Y ~ V1 + V2, data=sampleData)
v3Model <- lm(Y ~ V1 + V3, data=sampleData)

#examine this output
summary(fullModel)
```

```
## 
## Call:
## lm(formula = Y ~ V1 + V2 + V3, data = sampleData)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -1.48169 -0.21114 -0.03183  0.43365  1.13535 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept)  3.01843    2.51026   1.202   0.2467  
## V1           0.54433    0.58477   0.931   0.3658  
## V2           1.26916    0.46461   2.732   0.0148 *
## V3          -0.07616    0.26230  -0.290   0.7753  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.7454 on 16 degrees of freedom
## Multiple R-squared:  0.8528,	Adjusted R-squared:  0.8252 
## F-statistic: 30.89 on 3 and 16 DF,  p-value: 6.868e-07
```

```r
#does this work better?
anova(fullModel)
```

```
## Analysis of Variance Table
## 
## Response: Y
##           Df Sum Sq Mean Sq F value   Pr(>F)    
## V1         1 47.307  47.307 85.1404 8.31e-08 ***
## V2         1  4.143   4.143  7.4564  0.01481 *  
## V3         1  0.047   0.047  0.0843  0.77527    
## Residuals 16  8.890   0.556                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
#be careful, this is several tests: V2 vs. small, then V3 vs V2.

#ideally you would do 
anova(v1Model,v2Model)
```

```
## Analysis of Variance Table
## 
## Model 1: Y ~ V1
## Model 2: Y ~ V1 + V2
##   Res.Df    RSS Df Sum of Sq      F  Pr(>F)  
## 1     18 13.080                              
## 2     17  8.937  1    4.1431 7.8809 0.01212 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
#Note that Sum of Squares are the same as the first comparison in anova(fullModel), but degrees of freedom are different.
#That's why anova(model1,model2) is best. 

#vs
anova(v1Model,v3Model)
```

```
## Analysis of Variance Table
## 
## Model 1: Y ~ V1
## Model 2: Y ~ V1 + V3
##   Res.Df    RSS Df Sum of Sq      F Pr(>F)
## 1     18 13.080                           
## 2     17 13.036  1  0.043668 0.0569 0.8142
```

```r
#to replicate anova(fullModel)
anova(v2Model,v3Model)
```

```
## Analysis of Variance Table
## 
## Model 1: Y ~ V1 + V2
## Model 2: Y ~ V1 + V3
##   Res.Df    RSS Df Sum of Sq F Pr(>F)
## 1     17  8.937                      
## 2     17 13.036  0   -4.0994
```

```r
#yay. 

#now, using the second method of residuals: 

v2ResidModel <- lm(resid(v1Model) ~ sampleData$V2 - 1)
summary(v2ResidModel)
```

```
## 
## Call:
## lm(formula = resid(v1Model) ~ sampleData$V2 - 1)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -1.9466 -0.1631  0.2225  0.4177  1.3402 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)
## sampleData$V2   0.1758     0.1882   0.934    0.362
## 
## Residual standard error: 0.8113 on 19 degrees of freedom
## Multiple R-squared:  0.04389,	Adjusted R-squared:  -0.006435 
## F-statistic: 0.8721 on 1 and 19 DF,  p-value: 0.3621
```

```r
#not significant :( 

v3ResidModel <- lm(resid(v1Model) ~ sampleData$V3 -1 )
summary(v3ResidModel)
```

```
## 
## Call:
## lm(formula = resid(v1Model) ~ sampleData$V3 - 1)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1758 -0.2433  0.1877  0.4265  1.3466 
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)
## sampleData$V3 -0.001173   0.036868  -0.032    0.975
## 
## Residual standard error: 0.8297 on 19 degrees of freedom
## Multiple R-squared:  5.324e-05,	Adjusted R-squared:  -0.05258 
## F-statistic: 0.001012 on 1 and 19 DF,  p-value: 0.975
```


We'll use V1 and V3 since he thinks his second variables is not is not useful.
Does the `update` command help us? 


```r
summary(v3Model)
```

```
## 
## Call:
## lm(formula = Y ~ V1 + V3, data = sampleData)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1983 -0.2061  0.1869  0.4484  1.3219 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -3.30616    1.13959  -2.901 0.009938 ** 
## V1           1.79598    0.42684   4.208 0.000592 ***
## V3          -0.07353    0.30815  -0.239 0.814245    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.8757 on 17 degrees of freedom
## Multiple R-squared:  0.7841,	Adjusted R-squared:  0.7587 
## F-statistic: 30.87 on 2 and 17 DF,  p-value: 2.192e-06
```

```r
summary(lm(Y~V3+V1, data=sampleData))
```

```
## 
## Call:
## lm(formula = Y ~ V3 + V1, data = sampleData)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1983 -0.2061  0.1869  0.4484  1.3219 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -3.30616    1.13959  -2.901 0.009938 ** 
## V3          -0.07353    0.30815  -0.239 0.814245    
## V1           1.79598    0.42684   4.208 0.000592 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.8757 on 17 degrees of freedom
## Multiple R-squared:  0.7841,	Adjusted R-squared:  0.7587 
## F-statistic: 30.87 on 2 and 17 DF,  p-value: 2.192e-06
```

```r
up <- update(v1Model, . ~ . + sampleData$V3)
summary(up)
```

```
## 
## Call:
## lm(formula = Y ~ V1 + sampleData$V3, data = sampleData)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1983 -0.2061  0.1869  0.4484  1.3219 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   -3.30616    1.13959  -2.901 0.009938 ** 
## V1             1.79598    0.42684   4.208 0.000592 ***
## sampleData$V3 -0.07353    0.30815  -0.239 0.814245    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.8757 on 17 degrees of freedom
## Multiple R-squared:  0.7841,	Adjusted R-squared:  0.7587 
## F-statistic: 30.87 on 2 and 17 DF,  p-value: 2.192e-06
```


##Quiz!

1. looking at linear model coeffecients is safe/not safe (pick one)

2. ANOVA is better/not better

3. We can measure results in terms of false positives and false negatives as ... 

