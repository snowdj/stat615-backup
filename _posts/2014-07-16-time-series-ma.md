---
layout: post
title: MA Process
category : time series
tagline: "This is a project webpage for time aggregation"
tags : [AR, Stationary, time series]
---


# MA

$$Y_t=e_t-\theta_{1}e_{t-1}-\theta_{2}e_{t-2}-\cdots-\theta_{q}e_{t-q}$$

### MA(1)
$$Y_t=e_t-\theta e_{t-1}$$

$$\theta$$ is -0.8

{% highlight r linenos=table%} 
    Ye = rnorm(100, mean=0, sd=1);
    Y = c();
    Y[1] = Ye[1];
    for (i in 2:length(Ye)) {
    Y[i] = Ye[i] - (-0.8) * Ye[i-1];
    }
    Y = ts(Y);
    plot(Y, main="MA(1)", type="b", col="red");
    abline(h=0)
{% endhighlight %}

![plot of chunk unnamed-chunk-3](http://jonduan.github.io/time_aggregation/figure/unnamed-chunk-3.png) 


mean:
$$\mu_t=E(Y_t)=E(e_t)-\theta E(e_{t-1})=0$$

variance:
$$\sigma^2_t=Var(Y_t)=Var(e_t)+\theta^2 Var(e_{t-1})=(1+\theta^2)\sigma^2$$

correlatioin:

$$\gamma_{t,s}=Cov(e_t-\theta e_{t-1},e_s-\theta e_{s-1})=Cov(e_t,e_s)-\theta Cov(e_t,e_{s-1})-\theta Cov(e_{t-1},e_s)+\theta^2 Cov(e_{t-1},e_{s-1})$$



covariance
as a function of lag k 

$$
\gamma_k=\left\{
\\begin{array}{l l}
(1+\theta^2)\sigma^2 & (k=0) \\\\
-\theta \sigma^2     & (k=1) \\\\
0                      & (k>1)
\\end{array}\right.
$$

Correlation:

as a function of lag k :


$$
\rho_k=\left\{
\\begin{array}{l l}
1                          & (k=0) \\\\
(-\theta) / (1+\theta^2) & (k=1) \\\\
0                          & (k>1)
\\end{array}\right.
$$


### MA(2)


$$Y_t=e_t-\theta_1 e_{t-1}-\theta_2 e_{t-2}$$


{% highlight r linenos=table%} 
    Ye = rnorm(100, mean=0, sd=1);
    Y = c();
    Y[1] = Ye[1];
    Y[2] = Ye[2] - (-0.8) * Ye[1];
    for (i in 3:length(Ye)) {
    Y[i] = Ye[i] - (-0.8) * Ye[i-1] - (-0.9) * Ye[i-2];
    }
    Y = ts(Y);
    plot(Y, main="MA(2)", type="b", col="red");
    abline(h=0)
{% endhighlight %}

![plot of chunk unnamed-chunk-4](http://jonduan.github.io/time_aggregation/figure/unnamed-chunk-4.png) 


? MA(q) is stationary, after lag > p, no correlation 