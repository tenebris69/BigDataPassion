+++
author = ""
categories = []
date = "2017-06-01T21:43:17+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Docker wprowadzenie"
type = "post"

draft = true
+++

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
docker rm `docker ps -aq`
docker rm -f `docker ps -aq`

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