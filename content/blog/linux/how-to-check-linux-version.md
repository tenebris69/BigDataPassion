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

W tym poście zbiór poleceń pozwalających sprawdzić jakiej wersji systemu używamy. Przydatne gdy chcemy doinstalować dodatkowe oprogramowanie.

# Debian / Ubuntu / Mint

Zaczynamy od najpopularniejszej według https://distrowatch.com/ gałęzi systemów opartych o dystrybucję Debian:

![](https://upload.wikimedia.org/wikipedia/commons/d/d8/Debian_family_tree_11-06.png)

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

# Red Hat / CentOS / Fedora

Teraz kolej na dystrybucje z rodziny Red Hat'a

![](https://upload.wikimedia.org/wikipedia/commons/a/a3/Redhat_family_tree_11-06.png)