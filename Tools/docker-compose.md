truncate-s0/var/lib/docker/containers/*/*-json.log # delete logs

# inspect container network

docker inspect allpapers-proxy -f "{{json .NetworkSettings.Networks }}"

docker images -a | grep none | awk '{ print $3; }' | xargs docker rmi

docker-compose up -d --build # сборка проекта. Параметр build используется для того, чтобы заставить compose пересоздавать контейнеры. если изменений в коде не было, то сборка будет моментальной, так что не переживай по поводу пересборки всех образов

docker-compose up -d --build vpakete-vue

docker-compose up -d --scale allpapers-parser=3

docker rm -f $(docker ps -a -q) # remove all containers

sudo docker-compose up # interactive mode

sudo docker-compose up -d # demon mode , --build

sudo docker-compose ps # to see what is currently running

sudo docker-compose down -v

sudo docker ps -a

sudo docker exec -it 123db299f492 bash

sudo docker logs 3a8fbc749328 #

sudo docker inspect 3a8fbc749328 #

# Проверка работы

docker-compose run composer ls /app

docker-compose run php ls /opt/php

sudo docker system prune -a # clean system

# check docker

du -h -d 1  ~/Library/Containers/com.docker.docker

docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-

docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Пакетный менеджер (по аналогии с composer и npm, только у docker — контейнеры), позволяющий описывать необходимую структуру в одном файле (конфиге).

Каждый проект в обязательном порядке имеет docker-compose.yml и директорию src.

Также для каждого контейнера должна быть своя директория (совпадающая с названием контейнера), где будет храниться вся информация, необходимая для работы контейнера (конфиги, данные, ...).

Dockerfile:

is a simple text file that contains the commands a user could call to assemble an image.

Docker Compose:

- is a tool for defining and running multi-container Docker applications.

- define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.

- get an app running in one command by just running  docker-compose up

In my workflow, I add a Dockerfile for each part of my system and configure it that each part could run individually. Then I add a docker-compose.yml to bring them together and link them.

docker-compose up --detach --build

docker-compose build

docker-compose up

docker-compose restart worker