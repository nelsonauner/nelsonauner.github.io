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







Searching for significance in correlated variables
--------------------------------------------------

Business problem: A manager suspects that a given variable
*V*<sub>2</sub> is significantly predictive of a *Y*, but
*V*<sub>2</sub> is extremely correlated with *V*<sub>1</sub>, a variable
already incorporated into your company's simple linear model.

The manager wants the answer *now*: is *V*<sub>2</sub> important or not?

Some things to consider:

1.  Is it informative/safe to look at linear model coeffecients in a
    regression of *V*<sub>1</sub> and *V*<sub>2</sub> on *Y*?

2.  What other strategies are worth considering?

3.  Does running a linear model on the residuals of a first stage model
    help?

4.  How can we measure results in terms of false postives vs. false
    negatives?

* * * * *

Setup
-----

Let's use the following setup to test
[*V*<sub>1</sub>, *V*<sub>2</sub>] ∼ *N*(*μ*, Σ)

With *c**o**v*(*V*<sub>1</sub>, *V*<sub>2</sub>) = .9,
*v**a**r*(*V*<sub>1</sub>) = *v**a**r*(*V*<sub>2</sub>) = 1

Let *V*<sub>3</sub> ∼ *N*(*V*<sub>2</sub>, .5)

*Y* ∼ *N*(*V*<sub>1</sub> + *V*<sub>2</sub>, 1)

### Approach:

We test both under the null hypothesis that *V*<sub>2</sub> contains no
additional information (*V*<sub>2*n**u**l**l*</sub>) and the alternative
hypothesis in which *V*2 does contain information
(*V*<sub>2*a**l**t*</sub>)

    library(MASS)
    library(magrittr)
    library(dplyr)
    set.seed(5000)

    createSigma <- function(corr=.9) {
    Sigma <- diag(4) 
    Sigma[1,2] = Sigma[2,1] = .9
    return(Sigma)
    }

    simulateData <- function(V2coef=1,corr=.9,n=20) {
      mvrnorm(n = n, mu=rep(0,4), createSigma(corr), tol = 1e-6, empirical = FALSE, EISPACK = FALSE) %>% 
      as.data.frame %>%
      rename(V2_alt = V2, e1 = V3, e2 = V4) %>%
      transform(Y=  V1 + V2_alt + e1,
                V2_null = V1 + e2) %>% return
    }

    sampleData <- simulateData()
    with(sampleData,
         plot(Y,V1))

![]("nelsonauner.com/images/createSampleData-2.png")

Inspecting the linear models, you'd probably be surprised to see

    simulation <- list(data = sampleData)
    simulation$results <- with(simulation$data, list(
      fullModel = lm(Y ~ V1 + V2_alt +V2_null),
      v1Model = lm(Y ~ V1),
      v2_alt_Model = lm(Y ~ V1 + V2_alt),
      v2_null_Model = lm(Y ~ V1 + V2_null)))

    #to do lapply(simulation$results, function(x) {assign(names(x)) = x})
    attach(simulation$results) #just for this demo
    summary(fullModel)$coef

    ##              Estimate Std. Error   t value   Pr(>|t|)
    ## (Intercept) 0.3592781  0.1689855 2.1260884 0.04941089
    ## V1          0.6044075  0.3548356 1.7033451 0.10784533
    ## V2_alt      0.9538173  0.3316292 2.8761561 0.01097033
    ## V2_null     0.1791071  0.2109676 0.8489793 0.40841601

So, the new variable *V*2 is significant but your old variable *V*1 no
longer is... My first thought would be to use ANOVA:

    anova(fullModel)

    ## Analysis of Variance Table
    ## 
    ## Response: Y
    ##           Df Sum Sq Mean Sq F value    Pr(>F)    
    ## V1         1 46.195  46.195 83.1396 9.767e-08 ***
    ## V2_alt     1  4.901   4.901  8.8208  0.009027 ** 
    ## V2_null    1  0.400   0.400  0.7208  0.408416    
    ## Residuals 16  8.890   0.556                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    anova(v1Model,v2_alt_Model)

    ## Analysis of Variance Table
    ## 
    ## Model 1: Y ~ V1
    ## Model 2: Y ~ V1 + V2_alt
    ##   Res.Df     RSS Df Sum of Sq      F   Pr(>F)   
    ## 1     18 14.1918                                
    ## 2     17  9.2907  1    4.9011 8.9681 0.008147 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Note that Sum of Squares are the same as the first comparison in
anova(fullModel), but degrees of freedom are different. That's why
anova(model1,model2) is best.

    anova(v1Model,v2_null_Model)

    ## Analysis of Variance Table
    ## 
    ## Model 1: Y ~ V1
    ## Model 2: Y ~ V1 + V2_null
    ##   Res.Df    RSS Df Sum of Sq     F Pr(>F)
    ## 1     18 14.192                          
    ## 2     17 13.486  1   0.70526 0.889  0.359

Someone I talked to suggested first regressing Y on V1, and then the
residuals on V2.

    v2_alt_ResidModel <- lm(resid(v1Model) ~ sampleData$V2_alt - 1)
    summary(v2_alt_ResidModel)$coef

    ##                    Estimate Std. Error  t value Pr(>|t|)
    ## sampleData$V2_alt 0.3193023  0.2057836 1.551641 0.137246

    v2_null_ResidModel <- lm(resid(v1Model) ~ sampleData$V2_null -1 )
    summary(v2_null_ResidModel)$coef

    ##                      Estimate Std. Error   t value  Pr(>|t|)
    ## sampleData$V2_null 0.07502617  0.1360327 0.5515304 0.5876987

I wouldn't recommend this approach.

Followup Questions!
-------------------

1.  looking at linear model coeffecients is safe/not safe (pick one)

2.  ANOVA is better/not better

3.  We could compare procedures by using what metrics?

Simulations
-----------

It would be better to draw conclusions from more than one observation!

    #todo!
    testMethod <- function(significantMethod, corr, n_obs,n_sim) {
      #for each combination of parameters (creates a matrix of all possible parameters)
           #generate data
           #extract resuts of significantMethod for both false positive and false negative
      #return aggregated results over matrix of parameters, both false positive and false negative. 
    }
    significantMethod <- function(data) {} #return indicator of significance for V2_null and v2_alt
