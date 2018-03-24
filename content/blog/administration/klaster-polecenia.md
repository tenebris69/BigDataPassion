+++
author = ""
categories = []
date = "2017-06-01T21:43:17+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hdp sandbox 2.6"
type = "post"

draft = true
+++

# Uptime
~~~shell
for i in {1..5..1}; do ssh hdp${i} uptime; done
for i in {1..5..1}; do ssh aws${i} uptime; done
~~~

# Usuwanie poprzednich wpisów

~~~shell
for i in {1..5..1}; do ssh-keygen -f "/home/radek/.ssh/known_hosts" -R hdp${i}; done
for i in {1..5..1}; do ssh-keygen -f "/home/radek/.ssh/known_hosts" -R aws${i}; done
~~~

# Połączenie się do maszyn

~~~shell
cssh hdp{1..5}
cssh aws{1..5}
~~~

# Lista użytkowników
~~~shell
for i in `cat /tmp/users.txt`; do echo ${i}; done
~~~

# Tworzenie użytkownika
~~~shell
for i in `cat /tmp/users.txt`; do adduser ${i} ; done
for i in `cat /tmp/users.txt`; do echo "${i}:${i}" | chpasswd ; done
~~~

# Usuwanie użytkowników
~~~shell
for i in `cat /tmp/users.txt`; do userdel -r  ${i} ; done
~~~

# HDFS
~~~shell
for i in `cat /tmp/users.txt`; do hdfs dfs -mkdir /user/${i}; done
for i in `cat /tmp/users.txt`; do hdfs dfs -chown ${i} /user/${i}; done

for i in `cat /tmp/users.txt`; do hdfs dfsadmin -allowsnapshot /user/${i}/snap; done

for i in `cat /tmp/users.txt`; do hdfs dfs -rm -r -skipTrash /user/${i}; done
~~~
