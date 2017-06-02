+++
author = ""
categories = []
date = "2017-06-01T21:43:17+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "hdp sandbox 2.6"
type = "post"

+++

# Pobranie Sandbox'a 2.6
http://hortonworks.com/products/hortonworks-sandbox/#install
/home/radek/Pobrane/HDP_2.6_virtualbox_05_05_2017_14_46_00_hdp.ova

# Instalacja w virtualbox'ie

# Połączenie ssh

# Mapowanie portów


# Zmiany na maszynie

### Przydatne narzędzia
~~~shell
yum -y install gnome-system-monitor firefox htop git vim mc gedit wget
~~~

### Pulpit graficzny

~~~shell
yum grouplist

yum groupinfo "Desktop"

yum -y groupinstall "Desktop"

adduser sages
passwd sages

startx
~~~

Inne centos 6
~~~shell

yum groupinfo "X Window System"
yum groupinfo "Desktop"
yum groupinfo "General Purpose Desktop"
yum groupinfo "Desktop Platform"
yum groupinfo "Graphical Administration Tools"
yum groupinfo "Fonts"
yum groupinfo "Internet Browser"

yum -y groupinstall "X Window System"
yum -y groupinstall "Desktop"
yum -y groupinstall "General Purpose Desktop"
yum -y groupinstall "Desktop Platform"
yum -y groupinstall "Graphical Administration Tools"
yum -y groupinstall "Fonts"
yum -y groupinstall "Internet Browser"
~~~

centos 7
~~~shell
yum groupinfo "Server with GUI"
yum -y groupinstall "Server with GUI"


yum groupinfo "GNOME Desktop"
yum -y groupinstall "GNOME Desktop"
~~~