+++
author = "Radosław Szmit"
categories = ["Dystrybucje Big Data","Hortonworks Data Platform (HDP)"]
date = "2017-05-06T10:14:20+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hdp sandbox"
type = "post"

draft = true
+++

Uruchomienie bazy (Sandbox Hortonworks)
~~~shell
[root@sandbox ~]# /etc/init.d/hbase-starter start
Starting HBase...
Starting Postgre SQL                                      [  OK  ]
Starting name node                                        [  OK  ]
Starting zookeeper nodes                                  [  OK  ]
Starting hbase master                                     [  OK  ]
Starting hbase thrift                                     [  OK  ]
Starting hbase stargate                                   [  OK  ]
Starting hbase regionservers                              [  OK  ]
[root@sandbox ~]# 
~~~


Sprawdzenie stanu bazy po włączeniu
~~~ruby
hbase(main):001:0> status
1 servers, 0 dead, 280.0000 average load
hbase(main):002:0> 
~~~