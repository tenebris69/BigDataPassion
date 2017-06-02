+++
author = ""
categories = []
date = "2017-06-02T13:29:58+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "aws hdp"
type = "post"

+++

~~~shell
# ping
for i in {1..7..1}; do ping -c 1 hdp${i}; done

# uptime
for i in {1..7..1}; do ssh hdp${i} uptime; done

cssh centos@hdp-{1..7}

~~~


/etc/hosts:
~~~
54.149.229.39	ip-172-31-13-225.us-west-2.compute.internal	hdp1
54.149.72.85	ip-172-31-8-246.us-west-2.compute.internal	hdp2
54.244.161.97	ip-172-31-11-52.us-west-2.compute.internal	hdp3
34.209.25.141	ip-172-31-12-137.us-west-2.compute.internal	hdp4
54.201.192.34	ip-172-31-14-19.us-west-2.compute.internal	hdp5
54.200.237.19	ip-172-31-3-44.us-west-2.compute.internal	hdp6
54.191.200.16	ip-172-31-10-121.us-west-2.compute.internal	hdp7
~~~

~~~
172.31.13.225	hdp1
172.31.8.246	hdp2
172.31.11.52	hdp3
172.31.12.137	hdp4
172.31.14.19	hdp5
172.31.3.44     hdp6
172.31.10.121	hdp7
~~~

~~~
ip-172-31-13-225.us-west-2.compute.internal	hdp1
ip-172-31-8-246.us-west-2.compute.internal	hdp2
ip-172-31-11-52.us-west-2.compute.internal	hdp3
ip-172-31-12-137.us-west-2.compute.internal	hdp4
ip-172-31-14-19.us-west-2.compute.internal	hdp5
ip-172-31-3-44.us-west-2.compute.internal	hdp6
ip-172-31-10-121.us-west-2.compute.internal	hdp7
~~~

~~~shell
for i in {1..7..1}; do ssh-copy-id root@hdp{i}; done
~~~


Hosty:
~~~
ip-172-31-13-225.us-west-2.compute.internal
ip-172-31-8-246.us-west-2.compute.internal
ip-172-31-11-52.us-west-2.compute.internal
ip-172-31-12-137.us-west-2.compute.internal
ip-172-31-14-19.us-west-2.compute.internal
ip-172-31-3-44.us-west-2.compute.internal
ip-172-31-10-121.us-west-2.compute.internal
~~~

