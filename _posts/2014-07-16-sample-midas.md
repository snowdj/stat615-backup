---
layout: post
title: Common solutions to the mixed-frequency problem
category : MIDAS
tagline: "Mi(xed) Da(ta) S(ampling) regression (henceforth MIDAS regression) construct regressions combining data with different sampling frequencies"
tags : [intro, MIDAS, Data Aggregation, Econometrics]
---


## Common solutions to the mixed-frequency problem

**Temporal aggregation issue** 
- The mathematical structure commonly assumes that the underlying stochastic processes evolve in continuous time and data are collected at equi-distant discrete points in time.

- TIME AGGREGATION : 
  - Pros: admittedly easier
  - Cons: many, including
      - Loss of (past) information
      - Shocks, impulse and response mechanics mis-specified
      - No scope for real-time updating (so called ’now-casting’)
      - Econometric issues of bias and inefficiencies.

 - Making use of the disaggregate information, under general conditions, is theoretically preferable, since it can lead to more efficiency.
 

- Time Averaging
  - simple averaging is the most common method
  
- Step Weighting
 - use (normalized) weighting function: One might have the prior belief, for example, that more weight should be given to the samples of X that are more contemporaneous to the observed Y.
 - lead to parameter proliferation
 - lead to overfitting
 
- MIDAS : 
  - employ (exogenously chosen) distributed lag polynomials as weighting functions.
  
  - Benifit: 
      - preserves the timing information
      - achieve flexibility while maintaining parsimony

>
> **Explicitly modeling the flow of data (e.g., using mixed data sampling) may be more beneficial to the forecaster, especially if the forecaster is interested in constructing intra-period forecasts.**


## Features
- MIDAS involve regressors with different sampling frequencies
 - Similar to distributed lag models  
 $$Y_t = \beta_0 + B(L)X_t +\epsilon_t$$  
 Where $$B(L)$$ are some finite or infinite lag polynomial operator.

## Regional Analysis and Aggregation
- From a forecasting point of view, the pooling of disaggregated forecasts should help reduce the variance of the forecast errors, because the potential heterogeneity can be better captured.

- improvements in forecast accuracy

- the use of pooled region-specific forecasts improves upon the forecasting performance of countrywide GDP
