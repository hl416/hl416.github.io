---
layout: post
title: Set Up a Cloudera Hadoop Cluster
description: ""
modified: 2014-08-02
category: articles
tags: [life, love]
---

Postgresql server startup failed: look at pgstartup.log

Cloudera-scm-agent start fail: look at its logs "/opt/cloudera-manager/cm-5.1.1/log/cloudera-scm-agent/". I searched a while and almost everyone says the logs are in "/var/log/", but in fact not...

For the error of "cannot determine hostname or ip address", we need to set the hostname of the machine and corresponding records in the /etc/hosts. Here the hostname are got by execute "hostname" in shell.


I also disabled SeLinux and iptables. but not sure whether we really need to do that.



