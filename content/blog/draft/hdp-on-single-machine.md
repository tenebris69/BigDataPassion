

yum update -y

yum install -y wget

wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
rpm -ivh epel-release-7-9.noarch.rpm
yum update -y
yum repolist

yum install -y vim htop ntp openssh-server openssh-clients nano bash-completion

hostnamectl set-hostname hadoop.sages.com.pl --static

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

cd /etc/yum.repos.d/
wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.5.1.0/ambari.repo
yum install ambari-server -y
