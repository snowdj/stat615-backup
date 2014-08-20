---
layout: post
title: INTRODUCTION TO TIME SERIES
category : time series
tagline: "This is a project webpage for time_aggregation"
tags : [intro, beginner, time series]
---


TIME SERIES
========================================================


An example

$\{0, 1, 2, \ldots , 9\}$

variable:
$\{Y_t|t=0, \pm 1, \pm 2, \cdots \}$

mean:
$\mu_t=E(Y_t)$

variance:
$$\sigma^2_t=E((Y_t-\mu_t)^2)$$

covariance:
$$\gamma_{t,s}=Cov(Y_t,Y_s)=E((Y_t-\mu_t)(Y_s-\mu_s))$$

correlation:
$$\rho_{t,s}=\frac{\gamma_{t,s}}{\sqrt{\gamma_{t,t}\gamma_{s,s}}}$$



White noise

$$\{e_1,e_2,\ldots,e_t,\ldots\}$$


```r
  Y = ts(rnorm(100, mean=0, sd=1));
    plot(Y, main="white noise", type="b", col="red");
    abline(h=0)
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1.png) 
  
  
Random walk

  $$\begin{array}{l l l}
Y_1 & = & e_1 \\
Y_2 & = & e_1+e_2 \\
& \vdots & \\
Y_t & = & e_1+e_2+\cdots +e_t \\
& \vdots &
\end{array}$$
  

```r
    Y = ts(rnorm(100, mean=0, sd=1));
    for (i in 2:length(Y)) {
    Y[i] = Y[i] + Y[i-1];
    }
    plot(Y, main="random walk", type="b", col="red");
    abline(h=0)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

mean:
$$\mu_t=E(e_1 + \cdots + e_t)=E(e_1) + \cdots + E(e_t)=0$$


variance:
$$\sigma^2_t=Var(e_1 + \cdots + e_t)=Var(e_1) + \cdots + Var(e_t)=t\sigma^2$$

covariance

$$Cov(\sum_{i=1}^m{c_iY_i}, \sum_{j=1}^n{d_jY_j})=\sum_{i=1}^m{\sum_{j=1}^n{c_i d_j Cov(Y_i, Y_j)}}$$


if t < s, since only when i=j, $Cov(Y_i, Y_j)=\sigma^2$, so 

$$\gamma_{t,s}=t\sigma^2$$

$$\rho_{t,s}=\frac{t\sigma^2}{\sqrt{ts\sigma^4}}=\sqrt{\frac{t}{s}}$$

t and s different 
$\rho_{1,2}=0.707$
$\rho_{1,4}=0.500$
$\rho_{2,3}=0.816$
$\rho_{3,4}=0.866$