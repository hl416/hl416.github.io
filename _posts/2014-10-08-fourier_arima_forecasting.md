---
layout: post
title: fourier arima forecasting
description: ""
modified: 2014-10-08
category: articles
tags: [life, love]
---

{% highlight R %}
library(forecast)
library(doSNOW)
library(foreach)
library(doParallel)
{% endhighlight %}

{% highlight R %}
data <- scan(__FILE__)
dataseries <- ts(data,frequency=144)
datatest <- window(dataseries,start=c(11,1))
{% endhighlight %}

{% highlight R %}

cl<-makeCluster(40,type="SOCK")
registerDoSNOW(cl)
{% endhighlight %}
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
{% highlight R %}


 png(imgFilePath, width  = 3.25,
  height    = 2.25,
  units     = "in",
  res       = 1200,
  pointsize = 4)

plot(ls[1,],type="l",lty=1,col="red",xlab="Time (Unit: 10min)",ylab="Wait Time in Total (sec)")
lines(datatest[4:(144*5)],col="green")
legend("topright",c("Predicted","Actual"),col=c("red","green"),lty=1)
{% endhighlight %}
{% highlight R %}


stopCluster(cl)
dev.off()

{% endhighlight %}
