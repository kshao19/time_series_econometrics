# Time Series Econometrics: AR(1) and ARCH-M


This repository contains two mini-projects exploring time series econometric models with theory, simulation, and empirical estimation:

1. AR(1) Model – Simulation and approximation of the Autoregressive(1) process: theoretical mean/variance vs Monte Carlo approximation, stationarity, and ergodicity.
2. ARCH-M Model – Replication of Engle, Lilien, and Robins (1987) using U.S. Treasury data to test whether risk premia depend on conditional volatility.

Both projects are designed to be self-contained and reproducible, using Python (for AR(1)) and R (for ARCH-M).


## Table of Contents
- [Projects](#projects)
- [References](#references)

### Projects

#### 1. AR(1) Model: Theory and Simulation with Monte Carlo Simulation
Goal:
- Derive theoretical mean & variance of AR(1)
- Approximate them via Monte Carlo simulation
- Compare theoretical vs simulated results
- Examine stationarity and ergodicity assumptions

Key Features: 
- Theoretical recursion for mean/variance
- Monte Carlo with $N_s = 100,000$ sample paths
- Time-series diagnostics and plots

Code
- Results available [here](https://github.com/kshao19/time_series_econometrics/blob/main/Exploring%20AR(1)%20Models.md)
- Python scripts available [here](https://github.com/kshao19/time_series_econometrics/blob/main/Code/AR(1))



#### 2. ARCH-M Model: Risk Premia in Treasury Bills
Goal:
- Replicate results from Engle, Lilien, and Robins (1987)
- Estimate GARCH vs GARCH-M models for Treasury Bill excess returns
- Test whether conditional volatility affects the risk premium

Key Features: 
- Monthly 3- and 6-month U.S. Treasury Bill data (1984–2008)
- Maximum Likelihood estimation via rugarch (R)
- Model comparison using BIC

Code
- Results available [here](https://github.com/kshao19/time_series_econometrics/blob/main/ARCH-M%20Modeling.md)
- Python scripts available [here](https://github.com/kshao19/time_series_econometrics/blob/main/Code/ARCH-M)

### References
- Engle, R., Lilien, D., & Robins, R. (1987). Estimating Time Varying Risk Premia in the Term Structure: The ARCH-M Model. Econometrica, 55(2), 391–407.
- Hamilton, J. (1994). Time Series Analysis. Princeton University Press.
