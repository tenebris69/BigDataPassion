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

# Lista zainstalowanych pakietów

~~~shell
dpkg --get-selections | grep -v deinstall
apt list --installed
aptitude search '~i!~M'
~~~

# Usuwanie

~~~shell
apt-get purge mysql*
~~~

# Usuwanie (bazy danych)

~~~shell
service mysql stop
killall -KILL mysql mysqld_safe mysqld
apt-get --yes purge mysql-server mysql-client
apt-get --yes autoremove --purge
apt-get autoclean
deluser --remove-home mysql
delgroup mysql
~~~

# Backup and restore
~~~shell
dpkg --get-selections > list.txt
dpkg --clear-selections
sudo dpkg --set-selections < list.txt
~~~

# Aktualizacja

~~~shell
apt-get update && apt-get upgrade && apt-get dist-upgrade && apt-get autoremove && apt-get clean && apt-get autoclean && updatedb
~~~