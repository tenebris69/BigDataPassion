+++
author = "Radosław Szmit"
categories = ["Apache Spark"]
date = "2017-07-29T13:18:32+02:00"
description = "Instalacja HDP 2.6 na pojedynczej maszynie z wykorzystaniem CentoOS 6.9 i Virtualbox 5.1"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Instalacja HDP 2.6 na pojedynczej maszynie"
type = "post"

draft = true
+++

CentOS 6.9

1. automatyczne dhcp

dhclient -v

/etc/sysconfig/network-scripts/ifcfg-eth0
(ONBOOT=yes)

2. logowanie roota po haśle

/etc/ssh/sshd_config
PerminRootLogin yes

## Mapowanie portów NAT

## Logowanie po SSH

ssh-copy-id root@localhost -p 2222
ssh root@localhost -p 2222
 

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

yum -y install epel-release

5.

yum install -y wget vim htop ntp openssh-server openssh-clients nano bash-completion

5.

ssh-keygen



ssh-copy-id sandbox.bigdatapassion.pl



ssh sandbox.bigdatapassion.pl

6.

sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

setenforce 0

7. 

service iptables stop
/etc/init.d/iptables stop
chkconfig iptables off

8.

yum install -y ntp
chkconfig ntpd on

9.

umask 0022
echo umask 0022 >> /etc/profile

10.

cd /etc/yum.repos.d/
wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0/ambari.repo

yum install ambari-server -y

ambari-server setup

ambari-server start


# Legenda
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.1.0/bk_ambari-installation/content/ambari_repositories.html