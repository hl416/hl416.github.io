---
layout: post
title: Hadoop Setup
description: ""
modified: 2014-07-17
category: articles
tags: [life, love]
---

set up hadoop: 

http://tecadmin.net/steps-to-install-hadoop-on-centosrhel-6/

run wordcount.java: 

http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html#Prerequisites

run python code:

http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/

{% highlight java %}
org.apache.hadoop.security.AccessControlException: org.apache.hadoop.security.AccessControlException: Permission denied: user=hadoop, access=EXECUTE, inode="system":root:supergroup:rwx------
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
	at org.apache.hadoop.ipc.RemoteException.instantiateException(RemoteException.java:95)
	at org.apache.hadoop.ipc.RemoteException.unwrapRemoteException(RemoteException.java:57)
	at org.apache.hadoop.hdfs.DFSClient.getFileInfo(DFSClient.java:1061)
	at org.apache.hadoop.hdfs.DistributedFileSystem.getFileStatus(DistributedFileSystem.java:554)
	at org.apache.hadoop.mapred.TaskTracker.localizeJobTokenFile(TaskTracker.java:4489)
	at org.apache.hadoop.mapred.TaskTracker.initializeJob(TaskTracker.java:1285)
	at org.apache.hadoop.mapred.TaskTracker.localizeJob(TaskTracker.java:1226)
	at org.apache.hadoop.mapred.TaskTracker$5.run(TaskTracker.java:2603)
	at java.lang.Thread.run(Thread.java:744)
{% endhighlight %}
在hdfs-site.xml里加入下面的
<property> 
    <name>dfs.permissions</name> 
    <value>false</value> 
  </property>


{% highlight java %}
ERROR security.UserGroupInformation: PriviledgedActionException as:root cause:org.apache.hadoop.ipc.RemoteException: org.apache.hadoop.mapred.SafeModeException: JobTracker is in safe mode
	at org.apache.hadoop.mapred.JobTracker.checkSafeMode(JobTracker.java:5188)
	at org.apache.hadoop.mapred.JobTracker.getStagingAreaDir(JobTracker.java:3677)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:587)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:1432)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:1428)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1190)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:1426)
{% endhighlight %}

delete dfs/name/in_use.lock

java.lang.RuntimeException: java.lang.ClassNotFoundException: WordCount$Map
add the commented line below to the first line
Job job = new Job(conf, "wordcount");
//job.setJarByClass(WordCount.class);

