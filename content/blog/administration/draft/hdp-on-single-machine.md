+++
author = "Radosław Szmit"
categories = ["HBase","Administracja"]
date = "2017-05-01T17:04:17+02:00"
description = "Rozwiązywanie problemów z bazą HBase"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Hbase troubleshooting"
type = "post"

draft = true
+++

yum -y update
yum -y upgrade

yum install -y wget

wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
rpm -ivh epel-release-7-9.noarch.rpm
yum update -y
yum repolist



sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld

systemctl disable chrony.service

systemctl enable ntpd.service
systemctl is-enabled ntpd.service
systemctl start ntpd.service
systemctl status ntpd.service

chkconfig ntpd on

echo umask 022 >> ~/.bash_profile
echo umask 022 >> /etc/profile






hostnamectl set-hostname hadoop.sages.com.pl
hostnamectl set-hostname hadoop.sages.com.pl --static
hostnamectl set-hostname hadoop.sages.com.pl --pretty
hostnamectl set-hostname hadoop.sages.com.pl --transient

hostnamectl status
hostname
hostname -f