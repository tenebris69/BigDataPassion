+++
author = ""
categories = ["Administracja", "Docker"]
date = "2018-03-21T21:43:17+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Docker wprowadzenie"
type = "post"

draft=true
+++

Docker jest obecnie jednym z najpopularniejszych systemów konteneryzacji. O kontenerze możemy myśleć jak o czymś pomiędzy maszyną wirtualną która jest w pełni odizolowanym środowiskiem a aplikacjami instalowanymi w jednym wspólnym serwerze. 

# Info
docker info
docker --version

# Obrazy
docker images
docker rmi centos
docker rmi `docker images -q`
docker rmi -f `docker images -q`

# Kontenery
docker ps
docker ps -a

# Usunięcie kontenerów
~~~shell
docker rm `docker ps -aq`
docker rm -f `docker ps -aq`
~~~

# Podgląd
docker inspect centos

# Uruchomienie
docker run [--name=NAZWA] OBRAZ [POLECENIE]
docker run centos echo "Witaj świeci"

# Logi
docker logs [nazwa]

# Bash
docker exec -it NAZWA_URUCHOMIONEGO_KONTENERA bash

# Networking
docker network rm `docker network ls -q`

# Kopiowanie plików

docker cp kataloglokalny/plik.txt container:/tmp