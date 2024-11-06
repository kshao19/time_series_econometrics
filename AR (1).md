# Autoregressive Model
## This is a project that programmatically approximates and tests for assumptions that are used to construct an AR(1) model.

## Table of Contents
- [Tool](#tool)
- [Introduction to Autoregressive Model AR(1)](#introduction-to-autoregressive-model-ar1)
- [1. Mean and Variance of AR(1) from Theory](#1-mean-and-variance-of-ar1-from-theory)
- [2. Mean and Variance of AR(1) from Monte Carlo Simulation](#2-mean-and-variance-of-ar1-from-monte-carlo-simulation)
- [3. Theory and Approximation Result Comparison](#3-theory-and-approximation-result-comparison)
- [4. Examine Stationarity and Ergodicity of AR(1)](#4-examine-stationarity-and-ergodicity-of-ar1)
  -[Results](#results) 

<br/>

### Tool
- Python
  -   [Numpy](https://pandas.pydata.org/docs/): Model Calculation
  -   [SciPy](https://scipy.org/): Model Interpolatin & Optimization
  -   [Matplotlib](https://matplotlib.org/stable/): Result Visualization

The Python code to reproduce all results in this file can be found [here](https://github.com/kshao19/time_series_econometrics/blob/main/Code/AR(1)).

<br/>

###  Introduction to Autoregressive Model AR(1)
An AR(1) model is represented as $X_{t+1} = a + bX_{t} + \epsilon$. The model is built on the assumption of stationarity where the initial condition {X_0} is drawn from the normal distribution $\mathcal{N} (\mu_{stat}, \sigma^2_{stat})$

This section runs the Monte Carlo Simulation on an AR(1) process and shows how well the approximated mean and variance simulate the actual mean and variance of random walks. 

To start, let the AR(1) process be defined by the following variables:
- $\mu = 0.1$: the mean of the distribution
- $\sigma = 0.5$: the standard deviation of the distribution
- $\rho = 0.9$: the autoregressive parameter that represents the correlation between any two elements in a time series
- $N_t = 100$: the number of periods of the process (i.e., the length of event history)
- $z_0 = 0 $: the value of the initial condition

<br/>

### 1. Mean and Variance of AR(1) from Theory
This subsection computes the **actual mean and variance** of an AR(1) process by constructing **a sequence of unconditional distribution for the AR1** (see method *"dist_seq"* defined under the class *"AR1_process"* in the code section). 

The first step is to normalize the stationary distribution and compute the mean $\mu_{stat}$ and variance $\sigma^2_{stat}$:
- $\mu_{stat} = \frac{\mu}{1-\rho}$
- $\sigma^2_{stat} = \frac{\sigma^2}{1-\rho^2}$

Using the normalized mean and variance, I can compute the probability distribution function (pdf) of $z \sim \mathcal{N} (\mu_{stat}, \sigma^2_{stat})$. The pdf is defined by the vector $x_{vec}$ and the corresponding $pdf_{vec}$. 
- $x_{vec}$ is a linearly space vector of $N_t$ elements, with
  - $x_{min} = \mu_{stat} - 5 \sigma_{stat}$
  - $x_{max} = \mu_{stat} + 5 \sigma_{stat}$
- The probability distribution function for normal distribution is given as $f(x) = \frac{exp(-x^2/2)}{\sqrt{2\pi}}$

Then, I compute the sequence of unconditional distributions for the AR(1) with the following inputs:
- $N_t = 100$: the number of periods of the process (i.e., the length of event history)
- $z_0 = 0 $: the value of the initial condition
- $x_{vec}$: the support for the pdf found in the step above

Given the AR(1) model $z_{t+1} = \rho z_{t}+ \epsilon$,
- $\mu_{t+1, actual}$:
  - For $t=0$, $\mu_{1, actual} = E[z_{1, actual}] = E[\rho z_{0} + \epsilon] = \rho E[z_{0}] + E[\epsilon] = \rho z_{0} + \mu $. 
  - For $t=1,...,t-1$, $\mu_{t+1} = E[z_{t+1}] = E[\rho z_{t} + \epsilon] = \rho E[z_{t}] + E[\epsilon] = \rho \mu_t + \mu$.
- $\sigma^2_{t+1, actual}$:
  - For $t=0$, $Var_{1, actual} = Var(\rho z_{0} + \epsilon) = \sigma^2$. 
  - For $t=1,...,t-1$, $Var_{t+1, actual} = Var(\rho z_{t} + \epsilon) = \rho^2 Var(z_{t}) + \sigma^2$.
- The probability distribution function for normal distribution is given as $f(x) = \frac{exp(-x^2/2)}{\sqrt{2\pi}}$

<br/>

### 2. Mean and Variance of AR(1) from Monte Carlo Simulation
This subsection uses the Monte Carlo Simulation to approximate the mean and variance of an AR(1) process. 

The first step is to **create a random walk** (i.e., a sample path/event history) for the AR(1) process (see method *"sample_path"* defined under the class *"AR1_process"* in the code section). I initialize the sample path $z_{t, approx}$.

For each $t=1...,N_t$, I compute $z_{t, approx} = \rho z_{t-1, approx} + \epsilon_t$ for $\epsilon_t \in \epsilon$.
- $\epsilon$ is the residual vector consisting of $N_t$ number of random variables drawn from a normal distribution defined by n $\mathcal{N} (\mu, \sigma^2)$.

To approximate the mean and variance, simulate $N_s = 100,000$ sample paths of the AR(1) process using the method defined above. Each path is of length $N_t = 100$.
For each period $t=0,...,T$,
- $\widehat{\mu_t} = \frac{1}{N_s} \sum^{N_s}_{i=1} z_t(i)$
- $\widehat{\sigma_t} = \frac{1}{N_s} \sum^{N_s}_{i=1} [z_t(i) - \widehat{\mu_t}]^2$
- For the probability distribution function, for each $x_j \in x_{vec}$, 

  - if $x_j$ is not $x_0$ or $x_{N_{x-1}}$, for $x_{j-1} < z_t(i) \leq x_j$:
  
    $\widehat{pdf_t}(x_j) = \frac{1}{\Delta x_j} \frac{1}{N_s}  \sum^{N_s}_{i=1} 1$ 

  - if $x_j = x_0$, for $z_t(i) \leq x_0$:

    $\widehat{pdf_t}(x_0) = \frac{1}{\Delta x_j} \frac{1}{N_s}  \sum^{N_s}_{i=1} 1$ 

  - if $x_j = x_{N_x-1}$, for $x_{N_x-1} \geq z_t(i)$:

    $\widehat{pdf_t}(x_{N_x-1}) = \frac{1}{\Delta x_j} \frac{1}{N_s}  \sum^{N_s}_{i=1} 1$

  - For linearly spaced vector, $\Delta x_j = \frac{x_{max}-x_{min}}{N_x-1}$

<br/>

### 3. Theory and Approximation Result Comparison

![image](https://github.com/user-attachments/assets/147c0a76-5c73-4c82-a09f-1e9ee174b953)

![image](https://github.com/user-attachments/assets/625c1918-abc4-41c9-af02-5304cb2b1845)

<br/>

### 4. Examine Stationarity and Ergodicity of AR(1)
AR(1) has 2 important assumptions: (1) the time series is stationary, which suggests that the series' properties, such as mean and variance, do not change overtime. (2) the time series is also ergodic, suggesting that the mean and variance of an individual overtime is the same as the mean and variance of the population at a period of time. 

This subsection provides pseudocode to examine these 2 properties by (1) computing the stationary mean and variance from 1 random period of the simulated sample path and (2) comparing the results with the cross-sectional approximated ($\widehat{\mu_{99}}$, $\widehat{\sigma_{99}}$) and actual ($\mu_{99}$, $\sigma_{99}$) mean and variance from the last period of the sequence. 

I used the sample path starting at $t=50$ to $N_t$, where $\tilde{N} = N_t - 50 +1$. The stationary mean, variance, and pdf are calculated as the following:
- The mean: $\tilde{\mu} = \frac{1}{\tilde{N}} \sum^{N_t}_{t=50} z_t$
- The variance: $\tilde{\sigma} = \frac{1}{\tilde{N}} \sum^{N_t}_{t=50} (z_t - \tilde{\mu})^2 $
- The pdf, for each $x_j \in x_{vec}$:

  - if $x_j$ is not $x_0$ or $x_{N_{x-1}}$, for $x_{j-1} < z_t(i) \leq x_j$:
  
    $\widetilde{pdf_t}(x_j) = \frac{1}{\Delta x_j} \frac{1}{\tilde{N}}  \sum^{N_s}_{i=50} 1$ 

  - if $x_j = x_0$, for $z_t(i) \leq x_0$:

    $\widetilde{pdf_t}(x_0) = \frac{1}{\Delta x_j} \frac{1}{\tilde{N}}  \sum^{N_t}_{i=50} 1$ 

  - if $x_j = x_{N_x-1}$, for $x_{N_x-1} \geq z_t(i)$:

    $\widetilde{pdf_t}(x_{N_x-1}) = \frac{1}{\Delta x_j} \frac{1}{\tilde{N}}  \sum^{N_t}_{i=50} 1$

  - For linearly spaced vector, $\Delta x_j = \frac{x_{max}-x_{min}}{N_x-1}$   

<br/>

#### Results

| Statistic   | Simulated Sample Path | Approximated from AR(1) | Actual AR(1) |
| -------- | ------- |------- |------- |
| Mean  | 0.99    | 1.00   |1.00    |
| Variance | 1.32     | 1.31     | 1.32     |

![image](https://github.com/user-attachments/assets/c7e6138c-0d0b-4adc-8384-f3ab5ceba921)

The three means and variances are approximately equal to one another.

This is because:

(1) our assumption of stationarity: unconditional joint probability distribution does not change when shifted in time. Therefore, the sample path I randomly drawn for only one period has similar sample statistics and similar distributions as the last periods of the sample paths for one hundred period.

(2) Law of large numbers

(3) our assumption of ergodicity since joint probability distribution is approximately the same across time and across sections.
