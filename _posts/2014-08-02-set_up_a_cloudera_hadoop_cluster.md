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

When adding host to cluster, i encounter similar errors which could be fixed by similar methods.


I also disabled SeLinux and iptables. but not sure whether we really need to do that.


Impala fail to start: 
from its log, the error is: locale::facet::_S_create_c_locale name not valid

I searched for a long time and tried to the env "LC_ALL=C" as well as other options. but all of these approaches failed. 

in ssh, if i exec "perl", there would be similar locale error.
In ssh, i set "LC_ALL=C", and then exec "perl", there would be no errors.

I then realize impala did not use the env varies i set. because we not are in the same shell session.

I read the doc of cloudera, and found that if i manage impala using the cloudera manager, i have to set the env varies in the website. Finally, i successfllly set it in the webiste :services->impala->configureation->service-wide->advances->Impala Service Environment Advanced Configuration Snippet (Safety Valve)


ozzie fail to start:
In the log file /var/log/oozie/oozie-cmf-oozie-OOZIE_SERVER-hostname.log.out, i note there is an error like:
Could not load service classes, Cannot create PoolableConnectionFactory (Schema 'SA' does not exist)

Solution:CM->oozie->actions->create databases (http://community.cloudera.com/t5/Batch-Processing-and-Workflow/Oozie-schema-SA-does-not-exist/td-p/13592)


"Install parcels.." hangs for hours. need to "add host to cluster" using "managed hosts", also look at the cloudera-scm-agent.log to see what happend.

ntp related problems: remove all restrits in /etc/ntp.conf, and then service ntpd restart

Impala: "Could not connect to meta store using any of the URIs provided"

在各个节点上运行：

    hive --service metastore &
     hive --service hiveserver &
再运行

service impala-server restart
service impala-state-store restart
service impala-catalog restart


server file system:
fdisk /dev/sda
mkfs.ext4 /dev/sda1


run "blkid" to get a list of disk devices including UUID

then the corresponding info into /etc/fstab, and run "mount -a" to make it effetive immediately.  

hive:
org.apache.thrift.transport.TTransportException: Could not create ServerSocket on address 0.0.0.0/0.0.0.0

hive metastore进程 已经有启动过了，应该讲相关的进程kill 掉。
ps -ef |grep  hive 
将hive 相关进程kill 掉。然后重新启动：
./hive --service metastore
同理
   如果 运行 ./hive  --service hiveserver2 出现  0.0.0.0/0.0.0.0:10000端口的错误，也可能是已经存在了相关的进程，将其杀掉重新启动就能解决。


The website cannot query any data, the cloudera manager cannot start. First start the service shown in the page, then start another shown after the first service finished.

Also, i removed all clusters and management services, and then add again.

zookeeper: cannot start another server cos there are existing services. So restart existing services and then start the new one.

hdfs: cannot add dev-70 to the data node role, but the std_err.log do not show any error. I then found that in fact in this log file, each line is a command. So i exec the last command in this file, and then fix the problem. Problems: including: 1) wrong relative path --> change hdfs.sh to direct to the absolute path 2) memlock limit: change the memlock for hdfs user: /etc/security/limit.d/dfs.conf 3) permission: cannot write to /opt/dfs/dn, so chmod 777..


"the server declined access to the page or resource" cmf/process/num/logs. add records to /etc/hosts, cos cannot resolve dns.


hdfs missing blocks: hdfs fsck / -delete (need to first set ssl.client.truststore.location


All hosts need to have the same agent version as the server monitor, otherwise "the agent version of the client cannot be determined". we can do upgrade to clients in the management website.

remember to stop iptables, a lot of problems relate to it. "Quorum membership status could not be detected for the last 3 minute(s). The last connection attempt to the ZooKeeper server to determine the quorum membership status failed."

if a server is in safe mode, then it cannot write to any directory. so leave safe mode, if you see some errors like "cannot create dir /tmp/..", namenode is safe mode


[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -ls "/"
Found 4 items
drwxr-xr-x   - hbase hbase               0 2014-08-05 14:31 /hbase
drwxrwxr-x   - solr  solr                0 2014-08-03 03:10 /solr
drwxrwxrwt   - hdfs  supergroup          0 2014-08-03 15:50 /tmp
drwxr-xr-x   - hdfs  supergroup          0 2014-08-06 17:12 /user
[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -chmod 777 "/user"
[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -ls "/"
Found 4 items
drwxr-xr-x   - hbase hbase               0 2014-08-05 14:31 /hbase
drwxrwxr-x   - solr  solr                0 2014-08-03 03:10 /solr
drwxrwxrwt   - hdfs  supergroup          0 2014-08-03 15:50 /tmp
drwxrwxrwx   - hdfs  supergroup          0 2014-08-06 17:12 /user

[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -ls "/"
Found 4 items
drwxr-xr-x   - hbase hbase               0 2014-08-05 14:31 /hbase
drwxrwxr-x   - solr  solr                0 2014-08-03 03:10 /solr
drwxrwxrwt   - hdfs  supergroup          0 2014-08-03 15:50 /tmp
drwxr-xr-x   - hdfs  supergroup          0 2014-08-06 17:12 /user
[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -chmod 777 "/user"
[hdfs@crdc-c240-15 haolliu]$ hdfs dfs -ls "/"
Found 4 items
drwxr-xr-x   - hbase hbase               0 2014-08-05 14:31 /hbase
drwxrwxr-x   - solr  solr                0 2014-08-03 03:10 /solr
drwxrwxrwt   - hdfs  supergroup          0 2014-08-03 15:50 /tmp
drwxrwxrwx   - hdfs  supergroup          0 2014-08-06 17:12 /user

impala-shell need to run in the hdfs user role.


install hbase:

2014-08-12 20:54:15,024 WARN org.apache.hadoop.hbase.zookeeper.RecoverableZooKeeper: Node /hbase/rs/0.0.0.0,60020,1407891253501 already deleted, retry=false
2014-08-12 20:54:15,024 WARN org.apache.hadoop.hbase.regionserver.HRegionServer: Failed deleting my ephemeral node
org.apache.zookeeper.KeeperException$NoNodeException: KeeperErrorCode = NoNode for /hbase/rs/0.0.0.0,60020,1407891253501

I then found the cause is:

INFO org.apache.hadoop.hbase.regionserver.HRegionServer: STOPPED: Unhandled: org.apache.hadoop.hbase.ClockOutOfSyncException: Server crdc-c240-10,60020,1407891253501 has been rejected; Reported time is too far out of sync with master.  Time difference of 53040ms > max allowed of 30000ms

One solution is to set the ntp server for these servers, but our servers cannot connect to Internet. I then directly set the time of my two servers by using the "date" command in shell.

Today, i noted that when one instance of a service cannot successfully start. Then instead of only checking the stderr.log in the web GUI, i can also check the role log in the same page. In particular, i can check the same role path in the actual machine, then i can find more useful info.

