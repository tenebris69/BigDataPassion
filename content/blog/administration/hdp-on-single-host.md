+++
author = "Radosław Szmit"
categories = ["Dystrybucje Big Data","Hortonworks Data Platform (HDP)","Administracja Big Data"]
date = "2017-07-29T13:18:32+02:00"
description = "Instalacja HDP 2.6 na pojedynczej maszynie z wykorzystaniem CentoOS 7 i Virtualbox"
featured = "hortonworks-logo.png"
featuredalt = ""
featuredpath = "/img/administration"
linktitle = ""
title = "Instalacja HDP 2.6 na pojedynczej maszynie"
type = "post"

+++

W tym poście pokażę jak przygotować sobie jedną wirtualną maszynę z zainstalowaną dystrybucją Hortonworks. Taką maszynę, zwaną sandbox'em możemy wykorzystywać do celów testowych i developerskich.

# Stworzenie maszyny wirtualbox i instalacja systemu operacyjnego CentOS 7

Zaczniemy od stworzenia maszyny wirtualnej i instalacji na niej systemu CentOS 7. Opis jak to zrobić można znaleźć [tutaj](/blog/virtualbox-installing-cluster-with-centos)

# Przygotowanie systemu operacyjnego CentOS 7

## Instalacja Vim

~~~shell
yum -y install vim
~~~

## Włączamy automatyczne DHCP

Aby włączyć ręcznie dhcp należy wykonać:

~~~shell
dhclient -v
~~~

W celu skonfigurowania tego na stałe warto zmienić wartość (ONBOOT=yes) w pliku /etc/sysconfig/network-scripts/ifcfg-eth0

~~~shell
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
~~~

### Włączamy logowanie root'a po haśle

Ustawiamy parametr (PermitRootLogin yes) w pliku /etc/ssh/sshd_config

~~~shell
vim /etc/ssh/sshd_config
~~~

(UWAGA nigdy tego nie róbmy na produkcyjnych serwerach, NIGDY!)

## Mapowanie portów NAT

TODO

## Logowanie po SSH

Jeśli korzystamy z systemów Linux/Mac wygodnym rozwiązaniem może okazać się konfiguracja dostępu z swojego komputer i swojej konsoli. Żeby nie musieć podawać przy logowaniu ciągle hasła, warto wrzucić na maszynę wirtualną swój klucz publiczny by logować się bez hasła.

ssh-copy-id root@localhost -p 2222
ssh root@localhost -p 2222


## Zmieniamy hostname

Jeśli podczas instalacji nie ustawiliśmy adresu naszej maszyny możemy zrobić to teraz:

~~~shell
hostnamectl set-hostname sandbox.hortonworks.com

hostnamectl status

hostname
hostname -f
~~~

Polecenie hostname i hostname -f powinny zwracać tą samą wartość.

Warto też ustawić to samo w pliku hosts:

~~~shell
vim /etc/hosts 
~~~

wartość pliku:
~~~
127.0.0.1   sandbox sandbox.hortonworks.com
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
~~~

Po tej operacji warto zrestartować system.

## Konfiguracja epel release

Dodajemy repozytorium dodatkowych narzędzi systemowych

~~~shell
yum -y install epel-release
~~~

### Instalacja przydatnych narzędzi

~~~shell
yum -y install wget vim htop ntp openssh-server openssh-clients nano bash-completion
~~~

### Konfiguracja kluczy publicznego i prywatnego

Hadoop do działania wymaga by użytkownik systemowy mógł logować się do innych maszyn bez hasła za pomocą infrastruktury klucza prywatnego i publicznego, dlatego wykonujemy poniższe polecenia:

(przy pierwszym naciskamy klawisz *Enter* kilka razy, przy drugim wybieramy *yes* i wpisujemy hasło użytkownika *root*)
~~~shell
ssh-keygen
ssh-copy-id sandbox.bigdatapassion.pl
ssh sandbox.bigdatapassion.pl
~~~

### Wyłączamy SELINUX

~~~shell
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

setenforce 0
~~~

### Wyłączamy Firewall'a

~~~shell
service iptables stop
/etc/init.d/iptables stop
chkconfig iptables off
~~~

### Włączamy serwer czasu NTP

~~~shell
yum install -y ntp
chkconfig ntpd on
~~~

### Ustawiamy zalecany domyślny umask

~~~shell
umask 0022
echo umask 0022 >> /etc/profile
~~~

Na koniec restartujemy maszynę.




# Instalacja Apache Ambari

### Dodajemy repozytorium Ambari
~~~shell
wget -nv http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.5.1.0/ambari.repo -O /etc/yum.repos.d/ambari.repo
~~~
(UWAGA w momencie tworzenia tego artykułu wersja 2.5.2.0 miała bug'a i nie dało się jej skonfigurować na Centos 6.9)

### Instalujemy Ambari
~~~shell
yum install ambari-server -y
~~~

### Konfigurujemy Ambari
(W każdym pytaniu wybieramy odpowiedź domyślną klikając klawisz *Enter* za każdrym razem)
~~~shell
ambari-server setup
~~~

### Restartujemy Ambari
~~~shell
ambari-server start
~~~

Po tej operacji można zrestartować maszynę. Przed kontynuacją ustawmy naszej maszynie przynajmniej 4 rdzenie oraz przynajmniej 8GB RAM.


# Instalacja dystrybucji Hortonworks

Jeśli wszystko poszło jak trzeba, możemy zalogować się do Ambari we własnej przeglądarce pod adresem: http://localhost:8080/

TODO



# Instalacja środowiska graficznego

### Instalacja pakietów i narzędzi
~~~shell
yum -y groupinstall "X Window System"
yum -y groupinstall "Desktop"
yum -y groupinstall "General Purpose Desktop"
yum -y groupinstall "Desktop Platform"
yum -y groupinstall "Graphical Administration Tools"
yum -y groupinstall "Fonts"
yum -y groupinstall "Internet Browser"
~~~

### Instalacja przydatnych narzędzi

~~~shell
yum -y install gnome-system-monitor firefox htop git vim mc gedit 
~~~

### Instalacja dodatków dla Virtualbox Guest

~~~shell
yum -y install kernel*
yum -y install gcc kernel-devel kernel-headers dkms make bzip2 perl
~~~

### Dodawanie użytkownika

~~~shell
adduser sages
passwd sages
~~~

Restartujemy maszynę

### Uruchomienie trybu graficznego
~~~shell
startx
~~~











# Legenda
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.1.0/bk_ambari-installation/content/ambari_repositories.html