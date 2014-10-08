---
layout: post
title: Time Series Forecasting
description: ""
modified: 2014-10-07
category: articles
tags: [life, love]
---


In [[time series analysis]], the '''moving-average''' ('''MA''') model is a common approach for modeling [[univariate]] time series models. The notation MA(''q'') refers to the moving average model of order ''q'':

$$ X_t = \mu + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \cdots + \theta_q \varepsilon_{t-q} \$$

where μ is the mean of the series, the $$\theta_1$$, ..., $$\theta_q$$ are the parameters of the model and the $$ε_t$$, ..., $$ε_{t-1}$$ are [[white noise]] error terms. The value of ''q'' is called the order of the MA model. 


The following image shows the case of MA(1) and corresponding ACF. We can see that in the ACF, only $$r_{1}$$ is larger than 0. The ACF for MA(2) will similarly have $$r_{1}$$ and $$r_{2}$$ larger than 0.



