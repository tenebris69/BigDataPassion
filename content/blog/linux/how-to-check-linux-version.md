+++
author = "Radosław Szmit"
categories = ["Administracja", "Linux"]
date = "2018-03-20T21:27:14+01:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Jak sprawdzić jakiej wersji Linux'a używamy"
type = "post"

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
