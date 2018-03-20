+++
author = "Radosław Szmit"
categories = ["Administracja", "Linux", "Mint"]
date = "2018-03-20T21:28:14+01:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Linux Mint 18.x - instalacja przydatnych narzędzi"
type = "post"

+++

Do pracy, zwłaszcza tej komfortowej, potrzebujemy odpowiedniego oprogramowania które nam w tym pomaga. Od kilkunastu miesięcy moją dystrybucją numer jeden w świecie Linuxa stał się Mint, dlatego poniżej lista części moich narzędzi z których bardzo często korzystam wraz z instruckją instalacji. Lista dla Linux'a Mint 18.x bazującym na Ubuntu Xenial 16.04 LTS.

# Podstawowe pakiety

~~~shell
apt install -y alacarte
apt install -y bmon
apt install -y byobu
apt install -y clusterssh
apt install -y gedit
apt install -y git
apt install -y gource
apt install -y htop
apt install -y iptraf
apt install -y keepassx
apt install -y konsole
apt install -y meld
apt install -y multitail
apt install -y ncdu
apt install -y openssh-server
apt install -y openvpn
apt install -y ssh
apt install -y tree
apt install -y vim
apt install -y vim-addon-manager
apt install -y wget
~~~





# Dropbox
~~~shell
apt install -y dropbox
~~~

# Next Cloud
~~~shell
apt install -y nextcloud-client
~~~

# Spotify
~~~shell
echo "deb [arch=amd64] http://repository.spotify.com stable non-free" > /etc/apt/sources.list.d/spotify.list
apt install spotify-client
~~~

# Google Chrome
~~~shell
echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -

apt-get update
apt-get install google-chrome-stable
~~~

# Slack
~~~shell
echo "deb https://packagecloud.io/slacktechnologies/slack/debian/ jessie main" > /etc/apt/sources.list.d/slack.list
wget -O - https://packagecloud.io/slacktechnologies/slack/gpgkey  | sudo apt-key add -

apt-get update
apt-get install slack-desktop
~~~

# Virtualbox
~~~shell
echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian xenial contrib" > /etc/apt/sources.list.d/virtualbox.list
wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -

apt-get update
apt-get install virtualbox-5.2
~~~

# Docker
~~~shell
#apt-get remove docker docker-engine docker.io

apt-get update
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
    
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian xenial contrib" > /etc/apt/sources.list.d/virtualbox.list
apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'
apt update

~~~


# Aktualizacja systemu
~~~shell
apt-get update && apt-get upgrade && apt-get dist-upgrade && apt-get autoremove && apt-get clean && apt-get autoclean && updatedb
~~~