+++
author = "Radosław Szmit"
categories = ["CentOS","Administracja"]
date = "2017-05-04T21:34:44+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
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

Ręcznie dodajemy następujące wpisy do pliku /etc/sysconfig/network-scripts/ifcfg-enp0s3

~~~shell
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.172.186
NETMASK=255.255.255.0
GATEWAY=192.168.172.1
DNS1=8.8.8.8
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