+++
author = "Radosław Szmit"
categories = ["Virtualbox","CentOS","Administracja"]
date = "2017-05-04T12:59:11+02:00"
description = "Stworzenie klastra 3 maszyn w oparciu o Virtualbox'a 5 i system CentOS 7"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Testowy klaster obliczeniowy (VirtualBox 5 i CentOS 7)"
type = "post"

+++

VirtualBox jest darmowym narzędziem firmy Oracle (poprzednio Sun Microsystems) pozwaląjącym na łatwą i wygodną wirtualizację na własnym komputerze. W bardzo szybki sposób jesteśmy w stanie stworzyć z jego pomocą klaster kilku maszyn i z powodzeniem używać go do testów.

# Instalacja VirtualBox'a

VirtualBox'a można zainstalować na systemie Windows, Linux, MacOS i Solaris ściągając instalator ze strony projektu https://www.virtualbox.org/. Użytkownicy systemów Linux mogą go też łatwo i szybko zainstalować za pomocą repozytoriów (zalecane dla początkujących użytkowników).

Po zainstalowaniu możemy uruchomić narzędzie w trybie graficznym lub korzystać z linii komend. Ta druga opcja udostępnia większe możliwości, jednak jest adresowana do bardziej doświadczonych użytkowników, dlatego skorzystamy z wersji okienkowej.

Po uruchomieniu wersji graficznej powinniśmy zobaczyć coś takiego:
![](/img/virtualbox-installing-cluster-with-centos/virtualbox1.png)


# Stworzenie maszyny w VirtualBox

Teraz tworzymy nową czystą maszynę klikając _New_.

W nowo otwartym oknie wpisujemy nazwę, np. _hadoop1_, wybieramy Typ _Linux_ i wersję _Red Hat (64-bit)_ (CentOS jest oparty o dystrubucję Red Hat Enterprise Linux).

![](/img/virtualbox-installing-cluster-with-centos/virtualbox2.png)

W kolejnym ekranie przydzielamy pamięć RAM do maszyny, np. 16 GB.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox3.png)

Tworzymy nowy wirtualny dysk dla naszej maszyny wirtualnej.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox4.png)

Wybieramy domyślny typ dysku VDI.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox5.png)

Ustawiamy dynamiczne alokowanie powierzchni dysku (nasz wirtualny dysk będzie miał od razu widoczną całkowitą powierzchnię, ale fizycznie plik na dysku komputera hosta na którym zainstalowany jest VirtualBox będzie miał wielkość proporcjonalną do wielkości zajmowanego miejsca na wirtualnym dysku).

![](/img/virtualbox-installing-cluster-with-centos/virtualbox6.png)

Ustawiamy docelową wielkość dysku (z racji dynamicznej alokacji możemy ustawić więcej niż potrzebujemy, np. 100 GB) i klimay _Create_.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox7.png)

Nasza maszyna jest już utworzona i widzimy ją na liście.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox8.png)

# Pobranie CentOS'a

Najnowszą siądmą wersję systemu CentOS (Community ENTerprise Operating System) można pobrać z https://www.centos.org/download/. Wybieramy wersję minimal ISO, resztę pobierzemy przez sieć http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1611.iso. Najszybciej będzie ściągnąć iso z serwerów ICM'u lub AGH.

# Instalacja CentOS'a

Uruchamiamy utworzoną maszynę wirtualną przyciskiem _Run_. Następnie wybieramy wcześniej ściągnięty obraz ISO systemu CentOS (klikamy ikonkę katalogu obok wybieranej listy)

![](/img/virtualbox-installing-cluster-with-centos/centos1.png)
![](/img/virtualbox-installing-cluster-with-centos/centos2.png)

Po uruchomieniu maszyny z obrazu ISO wybieramy instalację.

![](/img/virtualbox-installing-cluster-with-centos/centos3.png)

Możemy ustawić język polski dla zgodności z polskimi stronami kodowymi.

![](/img/virtualbox-installing-cluster-with-centos/centos4.png)

Instalator będzie nas prosić o potwierdzenie automatycznego partycjonowania całego dysku.

![](/img/virtualbox-installing-cluster-with-centos/centos5.png)

Podczas instalacji systemu konieczne jest ustawienie hasła dla użytkownika _root_.

![](/img/virtualbox-installing-cluster-with-centos/centos6.png)

Gdy system skończy się instalować uruchamiamy ponownie maszynę (Virtualbox uruchomi ją już automatycznie z dysku wirtualnego a nie z wskazanego obrazu ISO)

![](/img/virtualbox-installing-cluster-with-centos/centos7.png)

Po uruchomieniu maszyny możemy zalogować się do niej na użytkownika _root_ wpisując go jako login i podając wcześniej ustawione hasło.

![](/img/virtualbox-installing-cluster-with-centos/centos8.png)

# Konfiguracja sieci

Dla wyłączonej maszyny w VirtualBox w zakładce _Sieć_ ustawiamy _Mostkowana karta sieciowa (bridge)_.

Uruchamiamy maszynę i logujemy się

~~~bash
ip l
~~~

Łączymy się z siecią za pomocą DHCP i urządzenai enp03

~~~bash
dhclient -v enp0s3
~~~

Gdy mamy już przydzielone IP (ponownie uruchamiamy ip l) możemy doinstalować kilka niezbędnych pakietów:

~~~bash
yum update -y
yum install -y wget

wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm
rpm -ivh epel-release-7-9.noarch.rpm

yum update -y
yum repolist

yum install -y NetworkManager NetworkManager-tui
~~~

Before using nmtui, first set "NM_CONTROLLED=yes" in /etc/sysconfig/network-scripts/ifcfg-enp0s3.
~~~bash
yum install vim -y
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
~~~

Edytujemy ustawienia urządzeni _enp0s3_
~~~bash
nmtui edit enp0s3 
~~~

Ustawiamy statyczny adres IP
~~~bash
192.168.172.186
192.168.172.200/24
~~~

Restartujemy serwis sieciowy
~~~bash
systemctl restart network.service
~~~

# Dodatkowe maszyny

Jeśli chcemy mieć więcej maszyn możemy ten proces powtórzyć mogąc jednocześnie wybrać inne opcje konfiguracyjne lub po prostu gdy chcemy mieć identyczne maszyny sklonować już stworzoną maszynę. W tym celu klikamy prawym przyciskiem myszy na wybranej maszynie i wybieramy opcję klonowania (Ctrl+O)

Warto przy tym wybrać opcję stworzenia innego numeru MAC adresu karty sieciowej:

![](/img/virtualbox-installing-cluster-with-centos/virtualbox9.png)

oraz wybrać pełną kopię.

![](/img/virtualbox-installing-cluster-with-centos/virtualbox10.png)

W ten sposób otrzymaliśmy gotowy klaster testowy i możemy zainstalować na nim Hadoopa lub całą wybraną jego dystrybucję czemu poświęcone będą kolejne wpisy.

# Legenda
* https://www.virtualbox.org/
* https://www.centos.org/download/
* http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1611.iso
