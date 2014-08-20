---
layout: post
title: Distributed Lag Models
category : Distributed Lag Models
tagline: "Mi(xed) Da(ta) S(ampling) regression (henceforth MIDAS regression) construct regressions combining data with different sampling frequencies"
tags : [intro, Distributed Lag Models, Data Aggregation, Econometrics]
---




## Augmented distributed lag functions

When the difference in sampling frequencies between the regressand and the regressors is large, distributed lag functions are typically employed to model dynamics avoiding parameter proliferation


## Distributed Lag Models
- A distributed lag model is a model for time series data in which a linear regression—regression equation is used to predict current values of a dependent variable based on both the current values of an explanatory variable and the lagged values of this
explanatory variable.
- finite
$$y_t+1^Q = \mu+ \beta_1 x_t^Q + \beta_2 x_{t-1}^Q + \beta_3 x_{t-2}^Q +...+u_{t+1}$$

- infinite

$$y_t+1^Q = \mu+ \beta_1 x_t^Q + \beta_2 x_{t-1}^Q + \beta_3 x_{t-2}^Q +...+ \beta_n x_{t-n+1}^Q + u_{t+1}$$

## Distributed Lag Models - Unrestricted estimation

- The simplest way to estimate parameters by OLS, assuming a fixed maximum lag. However, multicollinearity among the lagged regressors often arises, leading to high variance of the coefficient estimates.
- The most important finite distributed lag model is due to Almon (1965). The Almon lag assumes that $$n + 1$$ lag weights are related to $$P + 1$$ linearly estimable underlying parameters $$(n < k)$$ according to
$$\beta_i = \sum_{j=0}^P \theta_j i^j$$
where $$i = 0, ..., n.$$

## Autoregressive Distributed Lag Models
Autoregressive Distributed Lag Model - $$ADL(P_Y ,P_X )$$
$$y_t+1^Q = \mu+ \alpha_1 y_t^Q+ \alpha_P y_{t-P_Y}^Q + \beta_1 x_t^Q + \beta_2 x_{t-1}^Q + \beta_3 x_{t-2}^Q +...+ \beta_n x_{t-P_X+1}^Q + u_{t+1}$$


##  Original univariate MIDAS regression model
$$y_t = \alpha + \beta midas^K (\theta) x_t^k + \epsilon_t$$
 
where $$midas^K$$ : smoothing the $$K$$ past value of the covariate $$x_t$$ by using a functional polynomial:
$$midas^K (\theta) x_t^k := \sum_{k=1}^K \frac {f_K (k,\theta)}{\sum_{l=1}^K f_K (l,\theta)} x_{t-(k-1)/k}^k $$

## An extended MIDAS regression
- $$y_t^Q$$ quarterly sampled stationary variable that we aim at predicting,
- $$x_t^M$$ a vector of $$N_M$$ stationary monthly variables,
- $$x_t^D$$ a vector of $$N_D$$ stationary daily variables.
- extended MIDAS model enabling the mixing of daily
and monthly information:
$$y_t^Q = \alpha + \phi y_{t-1}^Q + \sum_{i=1}^{N_M} (\gamma_j) midas^{K_D} (\omega_j)  x_{t,j}^M +\sum_{i=1}^{N_D} (\beta_i) midas^{K_D} (\theta_i) x_{t,i}^D + \epsilon_t$$

## Parameterization

There are several possible parameterizations of the MIDAS
polynomial weights including, for example, the U-MIDAS
(unrestricted MIDAS polynomial), normalized Beta probability
density function, normalized exponential Almon lag
polynomial, and polynomial specification with step functions.

  - The exponential Almon function $$\gamma$$ :
 $$f_K (k,\theta) = \gamma_K(k,\theta_1,\theta_2) = exp(\theta_1k +\theta_2k^2)$$
  - The "Beta-Lag" function $$\Xi$$, based on the Beta function:
 $$f_K (k,\theta) = \Xi_K(k,\theta_1,\theta_2) = (\frac{k}{K})^{\theta_1 -1} (1-\frac{k}{K})^{\theta_2 -1} \Gamma(\theta_1 +\theta_2)/(\Gamma(\theta_1)\Gamma(\theta_2))$$,
  - The restricted "Beta-Lag" function $$\Psi$$, with $$\theta_1 = 1$$  and $$\theta_2 > 1$$, of the form:
 $$f_K (k,\theta) = \Psi_K(k,\theta) = \theta (1-\frac{k}{K})^{\theta -1}$$
 



## AR-MIDAS


## U-MIDAS: MIDAS regressions with unrestricted lag polynomials


 
## FAMIDAS : Dynamic factor MIDAS 
- The choice of indicators
- Different methods to tackle with the "curse of dimensionality
 - factor models [Giannone et al., 2008]
 - bridge models [Barhoumi et al., 2012]
 - ridge regression models [Exterkate et al., 2011]
 - LASSO/LARS [Tibshirani, 1996]
 - bayesian shrinkage [De Mol et al., 2008]
- In the context of MIDAS regression, the dynamic factor MIDAS (FAMIDAS):[Frale and Monteforte, 2011].

## MS-U-MIDAS : Markov-switching MIDAS model 

