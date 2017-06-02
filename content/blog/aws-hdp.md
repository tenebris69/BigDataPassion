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

Żeby się zalogować bez hasła z konta root na inne należy wkleić od plików ~/.ssh/authorized_hosts zawartość pliku ~/.ssh/id_rsa.pub która w naszym przypadku wygląda tak:

~~~
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5U26cKr+/F+e88/GFs+hryyafOTzch7Sx1WVp2h+vee9/OWpySbaAZkyqT5/iNUmlAyoZ+87JD03PR7u0I4tgX8Lj6/KkSETVtAVL5ufeT1TWrJ7XuI/gcCt6/pLjc5p7gyWKbg6LKgbyaf+aey+Pkz5iI8FbDPnt5RrR8wIGY4LOzikkIwR0ZLrJUdbr0o4Mv96yvC+mkzcdIiuZ4VfH81lWMlpMbUv+3udM6qiQyjUbRNrSw7Ej0fWxU94/IoHz5P/ozuE2QNCDPLc+Ej2i9hMJKE3i2/Cqx2/JOJnAY3AHVz97MpfuYnoOmXMFhhrP7okvi+nmugKYIintbFbx root@ip-172-31-13-225.us-west-2.compute.internal
~~~


