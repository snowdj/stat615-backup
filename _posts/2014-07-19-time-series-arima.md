---
layout: post
title: ARMA Process
category : time series
tagline: "This is a project webpage for time aggregation"
tags : [ARMA, Stationary, time series]
---


# ARMA

ARMA(p,q):
$$Y_t=e_t+\phi_{1}Y_{t-1}+\phi_{2}Y_{t-2}+\cdots+\phi_{p}Y_{t-p}-\theta_{1}e_{t-1}-\theta_{2}e_{t-2}-\cdots-\theta_{q}e_{t-q}$$

ARMA(1,1)
$$Y_t=e_t+\phi Y_{t-1}-\theta e_{t-1}$$

mean = 0

variance:

$$
\gamma_k=\left\{
\begin{array}{l l}
\frac{1+\theta^2-2\phi\theta}{1-\phi^2}\sigma^2 & (k = 0) \\\\
\phi\gamma_0-\theta\sigma^2 & (k = 1) \\\\
\phi\gamma_{k-1} & (k \ge 2)
\end{array}
\right.
$$


correlation:

$$\rho_k=\phi^{k-1}\frac{(\phi-\theta)(1-\phi\theta)}{1+\theta^2-2\phi\theta} $$


{% highlight r linenos=table %}
    Ye = rnorm(100, mean=0, sd=1);
    Y = c();
    Y[1] = Ye[1];
    for (i in 2:length(Ye)) {
    Y[i] = Ye[i] + 0.8 * Y[i-1] - (-0.9) * Ye[i-1];
    }
    Y = ts(Y);
    plot(Y,  main="ARMA(1,1)", type="b", col="red");
    abline(h=0)
{% endhighlight %}

![plot of chunk unnamed-chunk-6](http://jonduan.github.io/time_aggregation/figure/unnamed-chunk-6.png) 

## ARIMA
$$ARIMA(p,d,q)$$

For example:

$$\begin{array}{l l l}
(ARIMA(0,0,1))(t) & = & e_t-\theta e_{t-1} \\\\
(ARIMA(0,0,2))(t) & = & e_t-\theta_1 e_{t-1}-\theta_2 e_{t-2} \\\\
(ARIMA(1,0,0))(t) & = & e_t+\phi (ARIMA(1,0,0))(t-1) \\\\
(ARIMA(1,0,2))(t) & = & e_t+\phi (ARIMA(1,0,2))(t-1)-\theta_1 e_{t-1}-\theta_2 e_{t-2} 
\end{array}$$



![ARIMA](http://blog.codinglabs.org/uploads/pictures/time-series-analysis-foundation/arima-models.png)
