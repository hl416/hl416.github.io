---
layout: post
title: fourier arima forecasting
description: ""
modified: 2014-10-08
category: articles
tags: [life, love]
---


Suppose we have an value varying with time, we want to use collected data to forecast the value in the future. In our case, the data has the seasonal characteristics, so that a seasonal ARIMA seems to be our first choice. However, the frequency of our data is 144 (we sample the data every 10 min, so 6 points an hour and 6 x 24 points every day), and arima() function can only process relatively small frequency. STE is similar. So, here we try to first use Fourier to model the seasonal pattern and use ARIMA to describe the dynamics. In fact, there is a good blog about this method:http://robjhyndman.com/hyndsight/longseasonality/.

First, lets load the libraries.
{% highlight R %}
library(forecast)
library(doSNOW)
library(foreach)
library(doParallel)
{% endhighlight %}


Then, read data from file and set the frequency.
{% highlight R %}
data <- scan(__FILE__)
dataseries <- ts(data,frequency=144)
datatest <- window(dataseries,start=c(11,1))
{% endhighlight %}


We need to do parallel processing otherwise it would be very slow given the scale of my dataset.
{% highlight R %}
cl<-makeCluster(40,type="SOCK")
registerDoSNOW(cl)
{% endhighlight %}


We use the previous data to estimate the value of the next 3 windows. More precisely, we want to estimate the value of 30 min later or we say always to be 3 steps ahead. If we want to forecast the variation in the next day, then it's eaiser, and we do not need to use for loop.
{% highlight R %}
ls<-foreach(s=1:(141+144*4),.combine=cbind,.packages='forecast') %dopar% {
        x = 11 + floor((s-1)/144)
        y = (s-1) %% 144 + 1
        datatrain <- window(dataseries,end=c(x,y))
        bestfit <- list(aicc=Inf)
        bestRegs <- 1
        for(i in 1:20) # try to find the best num of components
        {
          fit <- auto.arima(datatrain, xreg=fourier(datatrain, K=i), seasonal=FALSE)
          if(fit$aicc < bestfit$aicc)
                {
                        bestfit <- fit
                        bestRegs = i
                }
          else break;
        }
        fc <- forecast(bestfit, xreg=fourierf(datatrain, K=bestRegs, h=3))
        max(0,fc$mean[3]) # get the estimated value 30 min later (10*3)
}
{% endhighlight %}

Finally, we visualize the forecasting and stop the parallel processing.
{% highlight R %}
 png(imgFilePath, width  = 3.25,
  height    = 2.25,
  units     = "in",
  res       = 1200,
  pointsize = 4)
plot(ls[1,],type="l",lty=1,col="red",xlab="Time (Unit: 10min)",ylab="Wait Time in Total (sec)")
lines(datatest[4:(144*5)],col="green")
legend("topright",c("Predicted","Actual"),col=c("red","green"),lty=1)
stopCluster(cl)
dev.off()

{% endhighlight %}
