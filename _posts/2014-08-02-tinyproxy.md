---
layout: post
title: TinyProxy
description: ""
modified: 2014-08-02
category: articles
tags: [life, love]
---

I am setting up the cloudera-manager in a local network which does not have internet access. To use the features of "parcels" provided by cloudera-manager, i need to setup a HTTP proxy in my laptop which can redirect the traffic to the internet. I searched for a while, and found tinyproxy might be a good candidate.

I run tinyproxy but it does not show anything. I cannot find the corresponding process by running "ps". Also, i cannot find any log produced by tinyproxy.

We need to run "tinyproxy -d", then you can see the error msg.

tinyproxy: Could not create file @localstatedir@/log/tinyproxy/tinyproxy.log: No such file or directory

Search in Google and download a conf file.

