
<!-- README.md is generated from README.Rmd. Please edit that file -->
autoSEM
=======

The autoSEM package takes a number of heuristic search algorithms and applies them to structural equation models. This work was popularized by George Marcoulides and a good summary can be found in his 2012 Handbook of Structural Equation Modeling (Editor Rick Hoyle; 2014) chapter 40.

The package currently has three working algorithms: genetic algorithm (GA package), ant colony optimization (written in R by Ross Jacobucci), and the tabu search algorithm (written in R by Ross Jacobucci). The GA package is well established and works very well. The two algorithms written by Ross Jacobucci are experimental and may not work ideally. It is advised to try multiple algorithms to look for agreement and plausibility.

autoSEM is built upon the cfa function from the lavaan package. Currently, it is only supported to use lavaan for confirmatory factor analysis models. The use of covariates is not supported. However, every the use of the cfa function allows for the inclusion of categorical items (use categorical =), missing data (missing = "fiml"), among many other examples. Currently, the use of CV = TRUE is not supported with missing="fiml".

As a simple example, just looking at the indicators of a one factor model using the Holzinger Swineford (1939) dataset from the lavaan package:

``` r
library(lavaan)
devtools::install_github("RJacobucci/autoSEM")
library(autoSEM)

myData =  HolzingerSwineford1939[,7:15]

f1.vars <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9")

out = autoSEM(method="GA",data=myData,nfac=1,
        varList=list(f1.vars),CV=F,std.lv=TRUE,
        criterion="NCP",minInd=3,niter=10)
summary(out)
```

### The most important arguments to specify are:

-   nfac: number of factors to test
-   varList: list containing the variable names to be included in the model
-   criterion: the fit index to use
-   CV: whether cross-validation should be done. CV="bootstrap" is recommended
-   minInd: minimum number of indicators for each factor in the model
-   niter: number of iterations for the algorithm. It is recommended to start low (10) and increase based on computation time
-   std.lv: Set as TRUE, sets each factor variance to 1, therefore all factor loadings are estimated.

If your goal is to test multiple numbers of factors, use multFac:

``` r
f1.vars <- c("x1","x2","x3","x4","x5","x6","x7","x8","x9")
rrr = list(f1.vars)
facs <- 1:4

uu = multFac(facList=facs,parallel="yes",ncore=4,method="GA",
             data=myData,orth=FALSE,CV=FALSE,std.lv=TRUE,
             varList=rrr,criterion="RMSEA",niter=3)
uu
```

Given the number of factors, parallelization is important. autoSEM uses the snowfall package to make it easy. Just specify parallel="yes" and set the ncore= the number of factors you are testing or less.

### Questions, Suggestions, Errors?

send an email to: <rcjacobuc@gmail.com>
