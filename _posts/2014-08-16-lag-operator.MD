---
layout: post
title: Lag Operator
category : Time series
tagline: Lag Operator in time series
tags : [Lag Operator, Time series,  R]
---



Lag Operator in time series
========================================================


From Wikipedia

In time series analysis, the lag operator or backshift operator operates on an element of a time series to produce the previous element. 

    $$X= \{X_1, X_2, \dots \}\,$$ 

then


    $$\, L X_t = X_{t-1}  \; for all \; t > 1\,$$ 

and 

    $$\, X_t = L X_{t+1} for all \; t \geq 1\, $$


where L is the lag operator. Sometimes the symbol B for backshift is used instead. 
Note that the lag operator can be raised to arbitrary integer powers 

that

    $$\, L^{-1} X_{t} = X_{t+1}\,$$

and

    $$ \, L^k X_{t} = X_{t-k}.\, $$

## Lag polynomials


- polynomials of the lag operator can be used. 

specifies an AR(p) model:

    $$\varepsilon_t = X_t - \sum_{i=1}^p \varphi_i X_{t-i} = \left(1 - \sum_{i=1}^p \varphi_i L^i\right) X_t\, $$

- A polynomial of **lag operators** is called a lag polynomial 

the ARMA model can be concisely specified as

    $$\varphi (L) X_t = \theta (L) \varepsilon_t\,$$


     where $$\varphi (L)$$ and $$\theta (L)$$ respectively represent the lag polynomials

    $$\varphi (L) = 1 - \sum_{i=1}^p \varphi_i L^i\,$$

and

    $$\theta (L)= 1 + \sum_{i=1}^q \theta_i L^i.\,$$ 


- Polynomials of lag operators follow similar rules of multiplication and division as do numbers and polynomials of variables. For example,

    $$X_t = \frac{\theta (L) }{\varphi (L)}\varepsilon_t,$$ 


rantional lag polynomial 

using long division,

$$\frac{1}{1-\theta B}  = 1  + \theta B +  \theta^2 B^2+ ...$$

## Difference operator



In time series analysis, the first difference operator Δ is a special case of lag polynomial

    $$\begin{array}{lcr} 
    \Delta X_t & = X_t - X_{t-1} \\ 
    \Delta X_t & = (1-L)X_t ~. 
    \end{array}$$

Similarly, the second difference operator works as follows:

    $$\begin{align}
     \Delta ( \Delta X_t ) & = \Delta X_t - \Delta X_{t-1} \\ 
     \Delta^2 X_t & = (1-L)\Delta X_t \\ 
     \Delta^2 X_t & = (1-L)(1-L)X_t \\ 
     \Delta^2 X_t & = (1-L)^2 X_t ~. 
     \end{align}$$ 

The above approach generalises to the ith difference operator
 $$\Delta ^i X_t = (1-L)^i X_t \ .$$



We can use a **difference equation** to describe a dynamic pattern of a linear time series.





## Conditional expectation

It is common in stochastic processes to care about the expected value of a variable given a previous information set. Let $$\Omega_t$$ be all information that is common knowledge at time t (this is often subscripted below the expectation operator); then the expected value of the realisation of X, j time-steps in the future, can be written equivalently as:

    $$E [ X_{t+j} | \Omega_t] = E_t [ X_{t+j} ] \,.$$

With these time-dependent conditional expectations, there is the need to distinguish between the backshift operator (B) that only adjusts the date of the forecasted variable and the Lag operator (L) that adjusts equally the date of the forecasted variable and the information set:

    $$L^n E_t [ X_{t+j} ] = E_{t-n} [ X_{t+j-n} ] \, ,$$
    $$B^n E_t [ X_{t+j} ] = E_t [ X_{t+j-n} ] \, .$$ 