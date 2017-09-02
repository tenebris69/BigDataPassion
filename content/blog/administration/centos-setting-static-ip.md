+++
author = "Radosław Szmit"
categories = ["CentOS","Administracja Big Data"]
date = "2017-04-02T21:34:44+02:00"
description = ""
featured = "centos-logo.png"
featuredalt = ""
featuredpath = "/img/administration"
linktitle = ""
title = "Konfiguracja sieci w systemie CentOS 7"
type = "post"

+++

Trzy sposoby konfiguracji sieci w systemie CentOS 7.

# Dynamiczne IP za pomocą DHCP

Sprawdzamy interfejsy sieciowe i urządzenia
~~~shell
ip l
~~~

Łączymy się z siecią za pomocą DHCP i interfejsem enp03
~~~shell
dhclient -v enp0s3
~~~

# Statyczne IP ustawiane ręcznie

Wyłączamy NetworkManager'a

~~~shell
systemctl disable NetworkManager.service
systemctl stop NetworkManager.service
~~~

Ręcznie dodajemy następujące wpisy do pliku /etc/sysconfig/network-scripts/ifcfg-enp0s3

~~~shell
BOOTPROTO="static"
ONBOOT="yes"
IPADDR=192.168.172.186
NETMASK=255.255.255.0
GATEWAY=192.168.172.1
DNS1=8.8.8.8
~~~

Ewentualnie można zamiast edytora vi użyć sed'a:
~~~shell
sed -i 's/ONBOOT=no/ONBOOT=yes/g'  /etc/sysconfig/network-scripts/ifcfg-enp0s3
sed -i 's/BOOTPROTO=dhcp/BOOTPROTO=static/g'  /etc/sysconfig/network-scripts/ifcfg-enp0s3
~~~

Restartujemy serwis sieciowy
~~~shell
systemctl restart network.service
~~~

# Statyczne IP ustawiane za pomocą Network Manageraip

Zanim zaczniemy używać narzędzia nmtui, musimy ustawić "NM_CONTROLLED=yes" w pliku /etc/sysconfig/network-scripts/ifcfg-enp0s3.

~~~shell
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
~~~

Edytujemy ustawienia urządzeni _enp0s3_
~~~shell
nmtui edit enp0s3 
~~~

Ustawiamy statyczny adres IP
~~~shell
192.168.172.186/24
~~~
oraz zaznaczamy opcje:
* Wymagane adresowanie IPv4 dla tego połączenia
* Łączenie automatyczne

Restartujemy serwis sieciowy
~~~shell
systemctl restart network.service
~~~
