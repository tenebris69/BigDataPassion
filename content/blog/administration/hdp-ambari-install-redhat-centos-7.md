+++
author = "Radosław Szmit"
categories = ["Dystrybucje Big Data","Hortonworks Data Platform (HDP)","Administracja Big Data"]
date = "2017-05-05T19:24:48+02:00"
description = "Instalacja HDP 2.6 za pomocą Ambari 2.5 na systemie Red Hat 7.3 lub CentOS 7.3"
featured = "hortonworks-logo.png"
featuredalt = ""
featuredpath = "/img/administration"
linktitle = ""
title = "Instalacja dystrybucji Hortonworks Data Platform za pomocą Apache Ambari"
type = "post"

+++

Instrukcja instalacji dystrybucji Hortonworks Data Platform HDP-2.6.2.0 na maszynach z systemem Red Hat 7.3 lub CentOS 7.3. Do instalacji zostanie użyte Apache Ambari w wersji 2.5.1.0.

# Klaster ssh

Jeśli chcemy wykonywać wybrane polecenia jednocześnie na wszystkich maszynach możemy użyć polecenia _cssh_.
~~~shell
cssh root@hdp1 root@hdp2 root@hdp3
~~~

Jeśli wolimy klasyczne ssh, logujemy się na każdą maszynę oddzielnie:
~~~shell
ssh root@hdp1
~~~

UWAGA: Wszystkie dalesze polecenia wykonujemy zalogowani jako użytkownik *root*

# Aktualizacja systemu

~~~shell
yum update -y
~~~

# Instalacja przydatnych narzędzi

~~~shell
yum install -y wget
~~~

# Instalacja epel release

CentOS:
~~~shell
yum install -y epel-release
~~~

Red Hat:
~~~shell
cd /tmp
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y epel-release-latest-7.noarch.rpm
yum repolist
~~~

# Instalacja kolejnych przydatnych narzędzi

~~~shell
yum -y install wget vim htop ntp openssh-server openssh-clients nano bash-completion tree
~~~

# Zmieniamy hostname (opcjonalnie)

Na każdej z maszyn ustawiamy inny hostname, na przykład:
~~~shell
hostnamectl set-hostname hdp1.hortonworks.com --static
hostnamectl set-hostname hdp2.hortonworks.com --static
hostnamectl set-hostname hdp3.hortonworks.com --static
~~~

Status możemy sprawdzić
~~~shell
hostnamectl status
~~~

# Konfigurujemy hostów

Żeby maszyny się "widziały" musimy każdej ustawić odpowiedni hostname:

~~~shell
192.168.172.201 hdp1 hdp1.hortonworks.com
192.168.172.202 hdp2 hdp2.hortonworks.com
192.168.172.203 hdp3 hdp3.hortonworks.com
~~~

Oczywiście lista zależy od liczby maszyn (w tym przypadku 3) oraz adresach ip które można sprawdzić za pomocą polecenia "ip a". Trzeba także pamiętać że "swój" hostname dodaliśmy w korku poprzednim, więc musimy go pominąć.


# Konfigurujemy logowanie SSH bez hasła

Na każdem maszynie generujemy zestaw kluczy:
~~~shell
ssh-keygen
~~~

Na maszynie hdp1 wyświetlamy sobie klucz publiczny:
~~~shell
cat .ssh/id_rsa.pub
~~~

Pobrany klucz maszyny *hdp1* wklejami na każdej maszynie (także *hdp1*) do pliku:
~~~shell
vim .ssh/authorized_keys
~~~

Jeśli wszystko poszło jak trzeba z maszyny *hdp1* możemy teraz zalogować się po SSH do każdej innej maszyny:
~~~shell
ssh hdp3
~~~

# Wyłączamy selinux

Edytujemy plik /etc/sysconfig/selinux ustawiając SELINUX na disabled.

~~~shell
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

sestatus
~~~

### Wyłączamy Firewall'a

~~~shell
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld

systemctl disable firewalld.service
systemctl stop firewalld.service
systemctl status firewalld.service
~~~

### Włączamy serwer czasu NTP

~~~shell
systemctl disable chrony.service

systemctl enable ntpd.service
systemctl is-enabled ntpd.service
systemctl start ntpd.service
systemctl status ntpd.service
~~~

# Umask

~~~shell
echo umask 022 >> ~/.bash_profile
echo umask 022 >> /etc/profile
~~~

# Restart maszyn

Teraz warto wykonać restart wszystkich maszyn poleceniem *reboot*

# Instalacja Apache Ambari

UWAGA: Od teraz wszystkie operacje wykonujemy tylko na maszynie *hdp1*

~~~shell
wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.5.1.0/ambari.repo -O /etc/yum.repos.d/ambari.repo
yum install ambari-server -y
ambari-server setup
~~~

W przypadku ambari-server setup upewniamy się, że nie ma żadnych ostrzeżeń, zaś na wszystkie pytania odpowiadamy wartościami domyślnymi klikając enter:

~~~shell
[root@hdp1 ~]# ambari-server setup
Using python  /usr/bin/python
Setup ambari-server
Checking SELinux...
SELinux status is 'disabled'
Customize user account for ambari-server daemon [y/n] (n)? 
Adjusting ambari-server permissions and ownership...
Checking firewall status...
Checking JDK...
[1] Oracle JDK 1.8 + Java Cryptography Extension (JCE) Policy Files 8
[2] Oracle JDK 1.7 + Java Cryptography Extension (JCE) Policy Files 7
[3] Custom JDK
==============================================================================
Enter choice (1): 
To download the Oracle JDK and the Java Cryptography Extension (JCE) Policy Files you must accept the license terms found at http://www.oracle.com/technetwork/java/javase/terms/license/index.html and not accepting will cancel the Ambari Server setup and you must install the JDK and JCE files manually.
Do you accept the Oracle Binary Code License Agreement [y/n] (y)? 
Downloading JDK from http://public-repo-1.hortonworks.com/ARTIFACTS/jdk-8u112-linux-x64.tar.gz to /var/lib/ambari-server/resources/jdk-8u112-linux-x64.tar.gz
jdk-8u112-linux-x64.tar.gz... 100% (174.7 MB of 174.7 MB)
Successfully downloaded JDK distribution to /var/lib/ambari-server/resources/jdk-8u112-linux-x64.tar.gz
Installing JDK to /usr/jdk64/
Successfully installed JDK to /usr/jdk64/
Downloading JCE Policy archive from http://public-repo-1.hortonworks.com/ARTIFACTS/jce_policy-8.zip to /var/lib/ambari-server/resources/jce_policy-8.zip

Successfully downloaded JCE Policy archive to /var/lib/ambari-server/resources/jce_policy-8.zip
Installing JCE policy...
Completing setup...
Configuring database...
Enter advanced database configuration [y/n] (n)? 
Configuring database...
Default properties detected. Using built-in database.
Configuring ambari database...
Checking PostgreSQL...
Running initdb: This may take up to a minute.
Initializing database ... OK


About to start PostgreSQL
Configuring local database...
Configuring PostgreSQL...
Restarting PostgreSQL
Creating schema and user...
done.
Creating tables...
done.
Extracting system views...
ambari-admin-2.5.0.3.7.jar
...........
Adjusting ambari-server permissions and ownership...
Ambari Server 'setup' completed successfully.
~~~

# Uruchamiamy Ambari

~~~shell
ambari-server start
~~~

# Konfiguracja klastra HDP

Następnie wchodzimy na adres: http://hdp1:8080 i całą instalację kontynuujemy w trybie graficznym. Logujemy się jako admin / admin.

Następnie klikamy przycisk _Launch Install Wizard_

![](/img/hdp-ambari-install/hdp-lanuch-install.png)

Ustawiamy nazwę naszego klastra

![](/img/hdp-ambari-install/hdp-cluster-name.png)

Wybieramy wersję dystrybucji którą chcemy zainstalować

![](/img/hdp-ambari-install/hdp-select-version.png)

W kolejnym ekranie w polu _Target Hosts_ wskazujemy pełne adresy wybranych maszyn na których chcemy postawić dystrybucję

~~~shell
hdp1.hortonworks.com
hdp2.hortonworks.com
hdp3.hortonworks.com
~~~

oraz wklejamy zawartość pliku /root/.ssh/id_rsa maszyny na której zainstalowaliśmy Ambari

![](/img/hdp-ambari-install/hdp-install-options.png)

Po kliknięciu _Register and Confirm_ Ambari przygotowuje nasze maszyny do instalacji

![](/img/hdp-ambari-install/hdp-confirm-hosts.png)

Po zakończonym procesie powinniśmy dostać informację o udanym przygotowaniu maszyn

![](/img/hdp-ambari-install/hdp-confirm-hosts-success.png)

Jeśli jednak mamy informację o ostrzeżeniach, należy przejrzeć ich listę i rozwiązać problemy

![](/img/hdp-ambari-install/hdp-confirm-hosts-error.png)

Następnie wybieramy narzędzia które chcemy zainstalować (ilość i rodzaj według uznania, np. HDFS, MapReduce2, Tez, Hive, Pig, HBase, Oozie, Flume, Ambari Metrics)

![](/img/hdp-ambari-install/hdp-choose-services.png)

Wskazujemy na których maszynach mają być zainstalowane usługi zarządzające (Masters) (można zostawić na początek proponowane rozmieszczenie)

![](/img/hdp-ambari-install/hdp-assign-masters.png)

oraz usługi robocze (Slaves) (wybieramy wszystkie 3 maszyny dla każdej z usług)

![](/img/hdp-ambari-install/hdp-assign-slaves.png)

Konfigurujemy nasze serwisy, pola wymagane są na czerwono (najczęściej konieczność ustawienia hasła do bazy danych), pozostałe ustawienia na ten moment możemy pozostawić w ich wartościach domyślnych

![](/img/hdp-ambari-install/hdp-configure.png)

Akceptujemy ostrzeżenia i dostajemy podsumowanie instalacji

![](/img/hdp-ambari-install/hdp-review.png)

Na koniec Ambari zaczyna instalować wszystkie usługi zgodnie z naszymi wytycznymi

![](/img/hdp-ambari-install/hdp-install.png)

Po zakończonej instalacji możemy korzystać już w pełni wdrożonego klastra.

Gdyby w trakcie instalacji wystąpiły jakieś błędy podczas uruchamiania usług, możemy spróbować zatrzymać wszystkie serwisy a następnie ponownie je włączyć. W tym celu w lewym pasku serwisów w menu _Actions_ wybieramy _Stop All_ a następnie _Start All_.

# Legenda
* https://hortonworks.com/products/data-center/hdp/
* https://ambari.apache.org/
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.1.0/bk_ambari-installation/content/download_the_ambari_repo_lnx7.html
* https://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-installation/content/ambari_repositories.html
* https://cwiki.apache.org/confluence/display/AMBARI/Installation+Guide+for+Ambari+2.5.0