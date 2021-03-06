
<!-- README.md is generated from README.Rmd. Please edit that file -->
# rmoo - R Multi-Objective Optimization <img src="man/figures/logo.png" align="right" width="150px" alt=""/>

<!-- badges: start -->
[![R build status](https://github.com/benitezfj/rmoo/workflows/R-CMD-check/badge.svg)](https://github.com/benitezfj/rmoo/actions) [![CRAN status](https://www.r-pkg.org/badges/version/rmoo)](https://CRAN.R-project.org/package=rmoo) [![Codecov test coverage](https://codecov.io/gh/benitezfj/rmoo/branch/master/graph/badge.svg?token=QK4Z2yVUSw)](https://codecov.io/gh/benitezfj/rmoo?branch=master) [![Travis build status](https://travis-ci.com/benitezfj/rmoo.svg?branch=master)](https://travis-ci.com/benitezfj/rmoo) [![Lifecycle: maturing](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing) <!-- badges: end -->

## Overview

A Non-Dominated Sorting based Multi-Objective Optimization package, built upon the ['GA' package](https://CRAN.R-project.org/package=GA).

'rmoo' provides a complete and flexible framework for optimizing multiple supplied objectives. You will have at your disposal a wide range of configuration options for the NSGA, NSGA-II and NSGA-III algorithms, as well as representation of real numbers, permutations and binaries.

## Installation

You can install the **stable** version on [R CRAN](https://cran.r-project.org/package=rmoo):

``` r
install.packages("rmoo")
```

Or you can install the **development** version from [GitHub](https://github.com/benitezfj/rmoo):

``` r
# install.packages("devtools")
devtools::install_github("benitezfj/rmoo")
```

## Usage

A simple example of running nsga3 solving the DTLZ1 problem:

``` r
library(rmoo)

DTLZ1 <- function (x, nobj = 3) 
{
    if (is.null(dim(x))) {
        x <- matrix(x, 1)
    }
    n <- ncol(x)
    y <- matrix(x[, 1:(nobj - 1)], nrow(x))
    z <- matrix(x[, nobj:n], nrow(x))
    g <- 100 * (n - nobj + 1 + rowSums((z - 0.5)^2 - cos(20 * 
        pi * (z - 0.5))))
    tmp <- t(apply(y, 1, cumprod))
    tmp <- cbind(t(apply(tmp, 1, rev)), 1)
    tmp2 <- cbind(1, t(apply(1 - y, 1, rev)))
    f <- tmp * tmp2 * 0.5 * (1 + g)
    return(f)
}

result <- nsga3(fitness = DTLZ1,
                type = "real-valued",
                lower = c(0,0,0),
                upper = c(1,1,1),
                popSize = 92,
                n_partitions = 12,
                maxiter = 300)
```

![](https://github.com/benitezfj/rmoo/blob/master/man/figures/README-example-1.jpeg)<!-- -->

![](https://github.com/benitezfj/rmoo/blob/master/man/figures/README-example-2.png)<!-- -->
