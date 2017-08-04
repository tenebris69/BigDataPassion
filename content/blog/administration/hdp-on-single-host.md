+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-29T13:18:32+02:00"
description = "Instalacja HDP 2.6 na pojedynczej maszynie z wykorzystaniem CentoOS 6.9 i Virtualbox 5.1"
featured = "Apache_Spark_logo.svg.png"
featuredalt = ""
featuredpath = "/img/featured"
linktitle = ""
title = "Instalacja HDP 2.6 na pojedynczej maszynie"
type = "post"

+++

CentOS 6.9

1. automatyczne dhcp
2. logowanie roota po haśle
3. hostname

vi /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=newHostName
and: a good idea if you have any applications that need to resolve the IP of the hostname)

vi /etc/hosts 
127.0.0.1 newHostName
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
and then

 rebooting the system

4. epel release

yum install epel-release

5.

yum install -y vim htop ntp openssh-server openssh-clients nano bash-completion

ssh-keygen
ssh-copy-id sandbox.bigdatapassion.pl
ssh sandbox.bigdatapassion.pl

6.

sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

7. 

service iptables stop
chkconfig iptables off

8.

yum -y install ntp
chkconfig ntpd on

9.

