---
layout: post
title: Forecast Time series in R
category : Forecast
tagline: Forecast Time series in R from Rob's Forecast book
tags : [Forecast, Time series,  R]
---


Forecast Time series in R
========================================================



### Main package used


{% highlight r linenos=table %}
    library(fpp)
    ## Loading required package: forecast
    ## Loading required package: zoo
    ## 
    ## Attaching package: 'zoo'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric
    ## 
    ## Loading required package: timeDate
    ## This is forecast 5.4 
    ## 
    ## Loading required package: fma
    ## Loading required package: tseries
    ## Loading required package: expsmooth
    ## Loading required package: lmtest

    library(tseries)
    library(fma) # for more time series data
    library(expsmooth) # for more time series data
    library(lmtest) #for some regression functions
{% endhighlight %}

## Average method

* Forecast of all future values is equal to mean of historical data $${y_1,...y_T}$$.

* Forecasts :  
  $$\hat y_{T+h} = \bar y = (y_1 +...+ y_T)/T$$ 

## Naive method (for time series only)

* Forecasts equal to last observed value.
* Forecasts:   
    $$\hat y_{T+h|T} = y_T$$.
* Consequence of efficient market hypothesis.


## Seasonal naive method
* Forecasts equal to last value from same season.
* Forecasts: 
    $$\hat y_{T+h|T} = y_{T+h-km} $$ 
 
where m = seasonal period and $$k = [(h-1)/m] +1$$. ? 

And then $$km = h-1 + m$$. 

$$h - km = - m + 1$$, which has nothing with k. ?

## Drift method

* Forecasts equal to last value plus average change.
* Forecasts:

 
$$ 
\begin{array}{l l l}
\hat y_{T+h|T}  & = & Y_T + \frac{h}{T-1}\sum_{t=2} ^T (Y_T - y_1) \\\\
                & = & Y_T + \frac{h}{T-1}(Y_T - y_1). 
\end{array}
$$


* Equivalent to extrapolating a line drawn between first and last observations Forecasting.


## Some R code from forecast package


    x = beer
    Mean: meanf(x, h=20)
    Naive: naive(x, h=20) or rwf(x, h=20)
    Seasonal naive: snaive(x, h=20)
    Drift: rwf(x, drift=TRUE, h=20)


## Forecasting residuals

**Residuals in forecasting:** 

difference between observed value and its forecast based on all previous observations:  
 $$ e_t = y_t - y_{t|t-1} $$.

### Assumptions

1. $$e_t$$ uncorrelated. If they arent, then information left in residuals that should be used in computing forecasts.
2. $$e_t$$ have mean zero. If they dont, then forecasts are biased.

### Useful properties (for prediction intervals)
3. $$e_t$$ have constant variance.
4. $$e_t$$ are normally distributed.


