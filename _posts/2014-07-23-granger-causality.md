---
layout: post
title: Granger causality
category : Granger causality
tagline: The **Granger causality test** is a statistical hypothesis test for determining whether one time series is useful in forecasting another.
tags : [intro, Granger causality]
---


Granger causality
============================================
_From Wikipedia, the free encyclopedia_

The **Granger causality test** is a statistical hypothesis test for determining whether one time series is useful in forecasting another.[1] Ordinarily, regressions reflect "mere" correlations, but Clive Granger argued that causality in economics could be reflected by measuring the ability of predicting the future values of a time series using past values of another time series. Since the question of "true causality" is deeply philosophical, econometricians assert that the Granger test finds only "predictive causality".

A time series X is said to Granger-cause Y if it can be shown, usually through a series of _t-tests and F-tests_ on lagged values of X (and with lagged values of Y also included), that those X values provide statistically significant information about future values of Y.

### Intuitive explanation

Granger defined the causality relationship based on two principles:

    1.The cause happens prior to its effect.
    2.The cause has unique information about the future values of its effect.