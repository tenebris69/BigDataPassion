+++
author = "Radosław Szmit"
categories = ["Linux"]
date = "2017-10-14T07:14:15+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Jak sprawdzić jakiej wersji Linux'a używamy"
type = "post"

draft = true
+++

# Debian

~~~shell
cat /etc/debian_version
~~~

~~~shell
uname -a
~~~

https://wiki.debian.org/DebianReleases

# Ubuntu

Jak w Debian oraz dodatkowo:

~~~shell
cat /etc/issue
~~~

~~~shell
lsb_release -a
~~~

~~~shell
cat /etc/*release
cat /etc/lsb-release
~~~

# Linux Mint

Jak w Debian i Ubuntu oraz dodatkowo:

~~~shell
cat /etc/linuxmint/info
~~~

wersję Ubuntu dostaniemy:
~~~shell
cat /etc/upstream-release/lsb-release
~~~

# CentOS
# RedHat
