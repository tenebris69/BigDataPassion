

yum -y update
yum -y upgrade

yum install -y wget

wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
rpm -ivh epel-release-7-9.noarch.rpm
yum update -y
yum repolist

yum install -y vim wget htop ntp openssh-server openssh-clients nano bash-completion

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