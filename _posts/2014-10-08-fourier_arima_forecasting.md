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
waittime <- scan("logs/sum_waittime_four_weeks.csv")
waittimeseries <- ts(waittime,frequency=144)
#waittimetrain <- window(waittimeseries,end=c(11,1))
waittimetest <- window(waittimeseries,start=c(11,1))
#waittimepredicted = list(rep(0,144))
#number of iterations
iters<-142
#setup parallel backend to use 8 processors
cl<-makeCluster(40,type="SOCK")
#registerDoParallel(cl)
registerDoSNOW(cl)

#loop
#ls<-foreach(s=1:(141+144*4),.combine=cbind,.packages='forecast') %dopar% {
ls<-foreach(s=1:(141+144*4),.combine=cbind,.packages='forecast') %dopar% {
        x = 11 + floor((s-1)/144)
        y = (s-1) %% 144 + 1
        waittimetrain <- window(waittimeseries,end=c(x,y))
        bestfit <- list(aicc=Inf)
        bestRegs <- 1
        for(i in 1:20)
        {
          fit <- auto.arima(waittimetrain, xreg=fourier(waittimetrain, K=i), seasonal=FALSE)
          if(fit$aicc < bestfit$aicc)
                {
                        bestfit <- fit
                        bestRegs = i
                }
          else break;
        }
        fc <- forecast(bestfit, xreg=fourierf(waittimetrain, K=bestRegs, h=3))
        max(0,fc$mean[3])
#       a <- accuracy(fc,waittimetest[3])
#       print(a[2,"RMSE"])
}
imgFile = "figs/waittime/fourier_sum_waittime_30_min_ahead_positive.png"
png(imgFile, width  = 3.25,
  height    = 2.25,
  units     = "in",
  res       = 1200,
  pointsize = 4)

#postscript(imgFile)
#pdf("fourier_sum_waittime_30_min_ahead_positive.pdf")
plot(ls[1,],type="l",lty=1,col="red",xlab="Time (Unit: 10min)",ylab="Wait Time in Total (sec)")
lines(waittimetest[4:(144*5)],col="green")
legend("topright",c("Predicted","Actual"),col=c("red","green"),lty=1)
accuracy(ls[1,],waittimetest[4:(144*5)])
stopCluster(cl)
dev.off()

