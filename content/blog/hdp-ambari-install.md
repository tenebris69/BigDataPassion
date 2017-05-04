+++
author = "Radosław Szmit"
categories = ["HDP","Hadoop dystrybucje","Administracja"]
date = "2017-05-04T19:24:48+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Instalacja HDP za pomocą Ambari 2.5"
type = "post"

+++

Instrukcja instalacji dystrybucji Hortonworks na maszynach z systemem CentOS 7. Do instalacji zostanie użyte Apache Ambari.

# Aktualizacja systemu

~~~shell
yum update -y
~~~

# Instalacja przydatnych narzędzi

~~~shell
yum install -y wget
~~~

# Instalacja epel release

~~~shell
wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
rpm -ivh epel-release-7-9.noarch.rpm
yum update -y
yum repolist
~~~

# Konfigurujemy hosty

~~~shell
192.168.172.201 hadoop1 hadoop1.bigdatapassion.pl
192.168.172.202 hadoop2 hadoop2.bigdatapassion.pl
192.168.172.203 hadoop3 hadoop3.bigdatapassion.pl
~~~

# Klaster ssh

Jeśli chcemy wykonywać wybrane polecenia jednocześnie na wszystkich maszynach możemy użyć polecenia _cssh_.
~~~shell
cssh root@hadoop1 root@hadoop2 root@hadoop3
~~~

# Legenda
* 