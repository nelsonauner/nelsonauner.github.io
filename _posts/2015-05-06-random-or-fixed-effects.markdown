---
layout: post
subtitle: ""
picture: "images/2015-05-06-random-or-fixed_files/figure-markdown_strict/ant111b.png"
pwidth: "500"
pheight: "200"
long: "T"
showpic: "F"
categories: data

---

Test for whether you should use fixed or random effects
=======================================================

I've been thinking about random effects this week. I don't think many
people in business use them and I'm not sure if there's a good reason
for this. One explanation is that fixed effects are common in
introductory statistics and econometrics classes, so many people doing
"data" work have not heard of them. A second explanation might be that
the craze around machine learning has encouraged people to hack at data
without much regard to the underlying statistical processes.

My thought here is to try both on a couple of different data sets to
understand the difference!

1 - Corn Sites
--------------

from
<http://www.stat.wisc.edu/courses/st572-larget/Spring2007/handouts13-2.pdf>

Random effect assumes

-   *y*<sub>*i**j*</sub> = *μ* + *a*<sub>*i*</sub> + *e*<sub>*i**j*</sub>, *j* = 1, ..., *n*, *i* = 1, ..., *k*

-   *a*<sub>*i*</sub> ∼ *N*(0, *σ*<sub>*a*</sub><sup>2</sup>)

-   *e*<sub>*i*</sub> ∼ *N*(0, *σ*<sub>*e*</sub><sup>2</sup>)

<!-- -->

    # thanks dirk
    local({r <- getOption("repos")
           r["CRAN"] <- "http://cran.r-project.org" 
           options(repos=r)
    })

    #install.packages("dplyr")
    library(dplyr)
    #install.packages("lme4")
    library(lme4)

    #install.packages("DAAG")
    library(DAAG)

So, here is what the data looks like. We observe *harvwt* clustered by
site, also some variance components...

    str(ant111b)

    ## 'data.frame':    32 obs. of  9 variables:
    ##  $ site  : Factor w/ 8 levels "DBAN","LFAN",..: 1 2 3 4 5 6 7 8 1 2 ...
    ##  $ parcel: Factor w/ 4 levels "I","II","III",..: 1 1 1 1 1 1 1 1 2 2 ...
    ##  $ code  : num  58 58 58 58 58 58 58 58 58 58 ...
    ##  $ island: num  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ id    : num  3 40 186 256 220 ...
    ##  $ plot  : num  3 4 5.5 4.5 3.5 5 7 7 15.5 15 ...
    ##  $ trt   : num  111 111 111 111 111 111 111 111 111 111 ...
    ##  $ ears  : num  43.5 40.5 20 42.5 31.5 32.5 43.5 50 46 46.5 ...
    ##  $ harvwt: num  5.16 2.93 1.73 6.79 3.25 ...

    xyplot(harvwt ~ site, pch = 16, col = "blue",data=ant111b)

![plot of chunk
ant111b](http://nelsonauner.com/images/2015-05-06-random-or-fixed_files/figure-markdown_strict/ant111b.png)

    # still not clear why FE is a bad idea
    corn1.lmer = lmer(harvwt ~ 1 + (1 | site),data=ant111b)
    summary(corn1.lmer)

    ## Linear mixed model fit by REML ['lmerMod']
    ## Formula: harvwt ~ 1 + (1 | site)
    ##    Data: ant111b
    ## 
    ## REML criterion at convergence: 94.4
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -2.0349 -0.5607  0.0706  0.7437  1.8864 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  site     (Intercept) 2.368    1.54    
    ##  Residual             0.578    0.76    
    ## Number of obs: 32, groups:  site, 8
    ## 
    ## Fixed effects:
    ##             Estimate Std. Error t value
    ## (Intercept)     4.29       0.56    7.66

Always check residuals!

    plot(fitted(corn1.lmer), residuals(corn1.lmer))

![plot of chunk
ant11b\_residuals](http://nelsonauner.com/images/2015-05-06-random-or-fixed_files/figure-markdown_strict/ant11b_residuals.png)
So here's one difference--the fitted values are not the sample means (as
in FE), but rather shrinkage estimates.

    with(ant111b, {
      means = sapply(split(harvwt, site), mean)
      siteFit = sapply(split(fitted(corn1.lmer), site), mean)
      data.frame(mean = means, fitted = siteFit)
    })

    ##       mean fitted
    ## DBAN 4.885  4.851
    ## LFAN 4.207  4.212
    ## NSAN 2.090  2.217
    ## ORAN 6.915  6.764
    ## OVAN 4.833  4.801
    ## TEAN 3.036  3.108
    ## WEAN 5.526  5.455
    ## WLAN 2.841  2.925

    # means
    # returns error since 'with' command keeps objects in temporary namespace (pretty cool)
    with(ant111b, {
    corn1.aov = aov(harvwt ~ Error(site))
    summary(corn1.aov)
    })

    ## 
    ## Error: site
    ##           Df Sum Sq Mean Sq F value Pr(>F)
    ## Residuals  7   70.3    10.1               
    ## 
    ## Error: Within
    ##           Df Sum Sq Mean Sq F value Pr(>F)
    ## Residuals 24   13.9   0.578
