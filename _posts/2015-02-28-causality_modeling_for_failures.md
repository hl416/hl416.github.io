---
layout: post
title: causality modeling for failures
description: ""
modified: 2015-02-28
category: articles
tags: [life, love]
---










A quick try in the begining by using AdaBoost and 4 fields as the features. We have a total of 237 samples with 9 diff categories: 200 for training and 37 for testing. If we use the component ranking the first as the estimated failure cause, we have an estimation precision of 75%. If we use the components ranking the first and the second as the candidates for failure causes, we can cover 83.2% of test cases.

The next day, i got 582 samples, i use 500 for training and 82 for testing, the precision became to around 20%. I then found there are 119 categories, compared with only 9 for the 237 cases on the first day. So that for each category, there is little samples to train the algorithm.
