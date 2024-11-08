# ARCH-M
## This is a project that programmatically replicates the ARCH-M Model of the excess return to holding 6-month Treasury Bill in Engle, Lilien, and Robins paper 

## Table of Contents
- [Tool](#tool)
- [Introduction to ARCH-M](#introduction-to-arch-m)
- [Raw Data](#raw-data)

<br/>

### Tool
- R

The R code to reproduce all results in this file can be found [here](https://github.com/kshao19/time_series_econometrics/blob/main/Code/ARCH-M).

<br/>

###  Introduction to ARCH-M
ARCH-M stands for Autoregressive Conditional Heteroskedasticity Mean Model, which specifies that the mean of a series is a function of its conditional variance. This model assumes that the variance of $y_t$ is conditional on the variance of $y_{t-1}$.

This project replicates part of the results in Engle, Lilien, and Robins' (1987) paper ["Estimating time varying risk premia in the term structure: the ARCH-M model"](https://www.jstor.org/stable/1913242).

Let $R_t$ be the yield on 6-month Treasury Bills observed in month $t$ (annualized to precentage) and $r_t$ be that on 3-month Treasury Bills. The excess holding (i.e., risk premium) is given by $y_t = 2R_t - r_{t+3} - r_t$

<br/>

### Raw Data
The data set "treasury_rates.csv" is available [here](https://github.com/kshao19/time_series_econometrics/blob/main/Raw%20Data/treasury_rates.csv). It contains monthly data on constant maturity 3- and 6-month Treasury security rates from January 1984 to January 2008. 

<br/>

### Estimation using GARCH












