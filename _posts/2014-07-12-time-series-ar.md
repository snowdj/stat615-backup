---
layout: post
title: AR Process
category : time series
tagline: "This is a project webpage for time aggregation"
tags : [AR, Stationary, time series]
---


## AR

$$Y_t=e_t+\phi_{1}Y_{t-1}+\phi_{2}Y_{t-2}+\cdots+\phi_{p}Y_{t-p}$$


### AR(1)

$$Y_t=e_t+\phi Y_{t-1}$$


Recursive -> expand

$$Y_t=e_t+\phi e_{t-1}+\phi^2 e_{t-2}+\cdots+\phi^k e_{t-k}+\cdots$$

mean = 0


variance:
$$\sigma^2_t=Var(Y_t)=(1+\phi^2+\phi^4+\cdots+\phi^{2k}+\cdots)\sigma^2=(\lim_{n \to \infty}{\frac{1-\phi^{2n}}{1-\phi^2})\sigma^2}$$


when $$|\phi| < 1$$, converge to $$\frac{\sigma^2}{1-\phi^2}$$.


when $$|\phi| < 1$$, AR(1) is stationary.

covariance:

\[
\begin{array}{l l l}
\gamma_k & = & E((Y_{t-k}-\mu_{t-k})(Y_t-\mu_t)) \\\\
           & = & E(Y_{t-k}Y_t) \\\\
           & = & E(Y_{t-k}e_t + \phi Y_{t-k}Y_{t-1}) \\\\
           & = & E(Y_{t-k})E(e_t) + \phi E(Y_{t-k}Y_{t-1}) \\\\
           & = & \phi \gamma_{k-1}
\end{array}
\]

recursive:

covariance:

$$\gamma_k = \phi^k\gamma_0 = \phi^k\frac{\sigma^2}{1-\phi^2}$$
correlation:
$$\rho_k=\gamma_k / \gamma_0=\phi^k$$




{% highlight r linenos=table%} 
    Ye = rnorm(100, mean=0, sd=1);
    Y = c();
    Y[1] = Ye[1];
    for (i in 2:length(Ye)) {
    Y[i] = Ye[i] + 0.8 * Y[i-1];
    }
    Y = ts(Y);
    plot(Y,  main="AR(1)", type="b", col="red");
    abline(h=0)
{% endhighlight %}

![plot of chunk unnamed-chunk-5](http://jonduan.github.io/time_aggregation/figure/unnamed-chunk-5.png) 

### AR(p) characteristics function
$$1-\phi_1x-\phi_2x^2-\cdots-\phi_px^p=0$$

stationary condition: root of the func absolute value > 1.

For example, AR(1) func $1-\phi x=0$, root is $x=1/\phi$, then condition is $|1/\phi| > 1$.

AR(p) turn into a linear algebra question.