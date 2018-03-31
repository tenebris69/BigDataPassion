+++
author = "Radosław Szmit"
categories = ["Administracja", "Linux"]
date = "2018-03-20T21:27:14+01:00"
description = ""
featured = "Tux.png"
featuredalt = ""
featuredpath = "/img/linux"
linktitle = ""
title = "Jak sprawdzić jakiej wersji Linux'a używamy"
type = "post"

+++

W tym poście zbiór poleceń pozwalających sprawdzić jakiej wersji systemu używamy. Przydatne gdy chcemy doinstalować dodatkowe oprogramowanie.

# Debian / Ubuntu / Mint

Zaczynamy od najpopularniejszej według https://distrowatch.com/ gałęzi systemów opartych o dystrybucję Debian:

![](https://upload.wikimedia.org/wikipedia/commons/d/d8/Debian_family_tree_11-06.png)

* Wydania Debiana: https://wiki.debian.org/DebianReleases
* Wydania Ubuntu: https://wiki.ubuntu.com/Releases
* Wydania Mint'a: https://linuxmint.com/download_all.php

~~~shell
cat /etc/debian_version
cat /etc/issue

cat /etc/lsb-release
cat /etc/os-release

cat /etc/upstream-release/lsb-release
cat /etc/linuxmint/info

uname -a
lsb_release -a
lsb_release -cs
lsb_release -d
~~~

# Red Hat / CentOS / Fedora

Teraz kolej na dystrybucje z rodziny Red Hat'a

![](https://upload.wikimedia.org/wikipedia/commons/a/a3/Redhat_family_tree_11-06.png)

~~~shell
cat /etc/centos-release
cat /etc/os-release
cat /etc/redhat-release
cat /etc/system-release

hostnamectl
rpm --query centos-release

uname -a
lsb_release -a
lsb_release -cs
lsb_release -d
~~~