# Time Series Econometrics
## This is a project that programmatically approximates and tests concepts and models in Time Series Econometrics.

## Table of Contents
- [Tool](#tool)
- [Results and Findings](#results-and-findings)
  - [1. Autoregressive Model AR(1)](#1-autoregressive-model-ar1)

### Tool
- Python
  -   [Numpy](https://pandas.pydata.org/docs/): Model Calculation
  -   [SciPy](https://scipy.org/): Model Interpolatin & Optimization
  -   [Matplotlib](https://matplotlib.org/stable/): Result Visualization   
- R

### Results and Findings
#### 1. Autoregressive Model AR(1)
An AR(1) model is represented as $X_{t+1} = a + bX_{t} + \epsilon$. The model is built on the assumption of stationarity where the initial condition {X_0} is drawn from the normal distribution $\mathcal{N} (\mu_{stat}, \sigma^2_{stat})$

This section uses Python to create functions that create an AR(1) process, and the code to produce the result can be found [here](https://github.com/kshao19/time_series_econometrics/blob/main/Code/AR(1)).

To do this, the first step is to create a sample path as an event history for the AR(1) process. I initialize the sample path with 3 given variables to construct the normal distribution.
- $\mu$: the mean of the distribution
- $\sigma$: the standard deviation of the distribution
- $\rho$: the autoregressive parameter that represents the correlation between any two elements in a time series

To create the sample path, I first normalize the distribution and compute the mean and variance of the normal distribution where the mean of the normal distribution $\mu_stat = \frac{\mu}{1-\rho}$






