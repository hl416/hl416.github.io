---
layout: post
title: tbats forecasting
description: ""
modified: 2014-10-08
category: articles
tags: [life, love]
---

Suppose we have an value varying with time, we want to use collected data to forecast the value in the future. In our case, the data has the seasonal characteristics, so that a seasonal ARIMA seems to be our first choice. However, the frequency of our data is 144 (we sample the data every 10 min, so 6 points an hour and 6 x 24 points every day), and arima() function can only process relatively small frequency. ets() is similar.


First, lets import the libraries.
{% highlight R %}
library(forecast)
library(doSNOW)
library(foreach)
library(doParallel)
{% endhighlight %}

Then lets read the data and set the time frequency.
{% highlight R %}
data <- scan(PATH_OF_DATA_FILE)
dataseries <- ts(data,frequency=144)
datatest <- window(dataseries,start=c(11,1))
{% endhighlight %}

Next, we use the tbats algorithm to forecast the value with 3 steps ahead (here 30 min as each time window is 10 min). In fact, we do not need to use the for loop here. The reason I use it is I found sometimes the code will never finish, and when i check the processes, i found the states of these processes are "poll_schedule_timeout". I guess maybe the cause is there are too many processes running for too long time. So that I devide the forecasting task into 60 subtasks. I seems better, but still sometimes cannot finish.

{% highlight R %}
lsAll <- c()
for(index in 0:59)
{
        cl<-makeCluster(40,type="SOCK")
        registerDoSNOW(cl)

        ls<-foreach(s=(index*12+1):((index+1)*12),.combine=cbind,.packages='forecast') %dopar% {
                x = 11 + floor((s-1)/144)
                y = (s-1) %% 144 + 1
                datatrain <- window(dataseries,end=c(x,y))
                fc2 <- forecast(tbats(datatrain), h=3)
                max(0,fc2$mean[3])
            }
        lsAll = c(lsAll,ls[1,])
        stopCluster(cl)

}
{% endhighlight %}

Finally, we do visualization and stop the parallel processing.
{% highlight R %}
png("figs/data/tbats_sum_data_30_min_ahead_positive.png",width     = 3.25,
  height    = 2.25,
  units     = "in",
  res       = 1200,
  pointsize = 4)
plot(lsAll,type="l",lty=1,col="red",xlab="Time (Unit: 10min)",ylab="Wait Time in Total (sec)")
lines(datatest[4:(144*5)],col="green")
legend("topright",c("Predicted","Actual"),col=c("red","green"),lty=1)
dev.off()

{% endhighlight %}


