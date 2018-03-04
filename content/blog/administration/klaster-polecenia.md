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