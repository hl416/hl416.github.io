---
layout: post
title: Time Series Forecasting
description: ""
modified: 2014-10-07
category: articles
tags: [life, love]
---

Moving Average

In [[time series analysis]], the '''moving-average''' ('''MA''') model is a common approach for modeling [[univariate]] time series models. The notation MA(''q'') refers to the moving average model of order ''q'':

$$ X_t = \mu + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \cdots + \theta_q \varepsilon_{t-q} \$$

where μ is the mean of the series, the $$\theta_1$$, ..., $$\theta_q$$ are the parameters of the model and the $$ε_t$$, ..., $$ε_{t-1}$$ are [[white noise]] error terms. The value of ''q'' is called the order of the MA model. 

The following image shows the case of MA(1) and corresponding ACF. We can see that in the ACF, only $$r_{1}$$ is larger than 0. The ACF for MA(2) will similarly have $$r_{1}$$ and $$r_{2}$$ larger than 0.


The defition and related calculations are as follows.
![My helpful screenshot]({{ site.url }}/images/moving_average_1.png)
![My helpful screenshot]({{ site.url }}/images/moving_average_2.png)
![My helpful screenshot]({{ site.url }}/images/moving_average_3.png)
![My helpful screenshot]({{ site.url }}/images/moving_average_4.png)

Therefore, according to the following definition of weekly stationary process, MA(1) is a weekly stationary process.
![My helpful screenshot]({{ site.url }}/images/week_stationary_process.png)

Also, according to the above images, for MA(1), if we calcuate its ACF, then only ACF(1) > 0, BUT for others, ACF(2) to ACF(n) == 0. In this case, when we consider to use MA to analyze a time series data, we need first to make sure that this series is a weekly stationary process. 

Auto Regression:

AR(1) is a weekly stationary process, and the corresponding ACF is becoming smaller and smaller with the increase of time difference: ACF(1) > ACF(2) > ...> . And the lim is 0 when the time difference becomes extremely large.  Therefore, just like the case in moving average,  we can also look at the characteristics of ACF to say whether to use AR to analyze the data. But ACF would be difficult to decide whether to use AR(1) or AR(2) or AR(i). Then people propose PACF: partial auto-correlation function. In this case, in the AR(1) case, only PACF(1) > 0, and PACF(j) == 0 for j > 1. 
![My helpful screenshot]({{ site.url }}/images/autoregressive_1)
![My helpful screenshot]({{ site.url }}/images/autoregressive_2)
![My helpful screenshot]({{ site.url }}/images/autoregressive_3)






























