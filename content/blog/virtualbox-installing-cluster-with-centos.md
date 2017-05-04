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
![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox1.png)


# Stworzenie czystej maszyny w VirtualBox

Teraz tworzymy nową czystą maszynę klikając _New_.

W nowo otwartym oknie wpisujemy nazwę, np. _hadoop1_, wybieramy Typ _Linux_ i wersję _Red Hat (64-bit)_ (CentOS jest oparty o dystrubucję Red Hat Enterprise Linux).

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox2.png)

W kolejnym ekranie przydzielamy pamięć RAM do maszyny, np. 16 GB.

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox3.png)

Tworzymy nowy wirtualny dysk dla naszej maszyny wirtualnej.

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox4.png)

Wybieramy domyślny typ dysku VDI.

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox5.png)

Ustawiamy dynamiczne alokowanie powierzchni dysku (nasz wirtualny dysk będzie miał od razu widoczną całkowitą powierzchnię, ale fizycznie plik na dysku komputera hosta na którym zainstalowany jest VirtualBox będzie miał wielkość proporcjonalną do wielkości zajmowanego miejsca na wirtualnym dysku).

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox6.png)

Ustawiamy docelową wielkość dysku (z racji dynamicznej alokacji możemy ustawić więcej niż potrzebujemy, np. 100 GB) i klimay _Create_.

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox7.png)

Nasza maszyna jest już utworzona i widzimy ją na liście.

![Example image](/img/virtualbox-installing-cluster-with-centos/virtualbox8.png)

# Pobranie CentOS'a

Najnowszą siądmą wersję systemu CentOS (Community ENTerprise Operating System) można pobrać z https://www.centos.org/download/. Wybieramy wersję minimal ISO, resztę pobierzemy przez sieć http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1611.iso. Najszybciej będzie ściągnąć iso z serwerów ICM'u lub AGH.

# Instalacja CentOS'a

TODO

# Legenda
* https://www.virtualbox.org/
* https://www.centos.org/download/
* http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1611.iso
