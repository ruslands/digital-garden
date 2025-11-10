# remove none images

docker rmi $(docker images -f dangling=true -q)

# clean all logs

sh -c 'truncate -s 0 /var/lib/docker/containers/*/*-json.log'

if you use docker volumes ( -v volumename:/container/path) instead of binds (-v /host/path:/container/path), the default behavior is to copy pre-existing files from the container target folder back into the volume

Тут хранятся volumes

/var/lib/docker/volumes/

docker run --name nginxX5 -d -p 2222:80  -v /x5/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx

/var/lib/docker/overlay2 # clean this folder

docker system prune

docker system prune -a -f

Scale containers

docker-compose - [https://docs.docker.com/compose/reference/scale/](https://docs.docker.com/compose/reference/scale/)

docker swarm - [https://docs.docker.com/engine/reference/commandli...](https://docs.docker.com/engine/reference/commandline/service_scale/)

docker-compose up -d --scale tdmd=3

docker logs test > output.log

docker logs 1d6a0a77a29b --tail 100 2>&1 | tee SomeFile.txt

docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

pr

docker logs --follow <container ID>

Error 137

docker logs mycontainer_or_id --since 60m

sudo docker-compose up -d --remove-orphans --build dumper_ivlab

sudo docker run -it --rm --network socket_containers python:3.7

sudo docker run -it --rm -v $(pwd)/client_container/src:/src --network socket_containers --name client_dumper client_dumper

# copy file from container to host

docker cp <containerId>:/file/path/within/container /host/path/target

docker cp 11c5d5d82f09:/usr/src/app/cards_card.json .

If you want your containers to join a pre-existing network, use the external option

bridge type of network, it means that we can access to containers only by ip

host type of network share localhost "namespace"

ports:

-5432:5432

HOST_PORT:CONTAINER_PORT

Networked service-to-service communication use the CONTAINER_PORT

For example my app can connect to postgres db using container name and container port

postgres://db:5432

alias dp="sudo docker ps -a"

alias di="sudo docker images"

| - передает выход программы на вход следующей

Docker works only on KVM servers

===========Ports vs Expose=====================================

ports: 3376:8000 — пробрасываем порты между docker и нашей машиной, т.е. все запросы которые будут идти на 80 порт нашей машины, будут транслироваться на 80 порт docker.

Ports: 0.0.0.0:32769->3306/tcp

Expose ports. Either specify both ports (HOST:CONTAINER), or just the container port (a random host port will be chosen).

- Ports mentioned in docker-compose.yml will be shared among different services started by the docker-compose.

- Ports will be exposed to the host machine to a random port or a given port.

Expose: 3306/tcp

Expose ports without publishing them to the host machine - they’ll only be accessible to linked services. Only the internal port can be specified.

===========run multiple containers in same server===========

1. Create network for containers

   docker network create nginx-proxy

   docker network inspect nginx-proxy

   docker network ls

   docker network rm <network_id>

2. Specify network in containers

3. We have bridge type of network, it means that we can access to containers only by ip

4. ifconfig -> eth0 inet addr my public address, docker network address br-network_id inet

addr

5. docker inspect container_id , get IP address of container  "IPAddress": "172.26.0.3".

"Gateway": "172.26.0.1" IP address of host machine.

6. запустить два контейнера с проектами и nginx, каждый nginx's слушают разные порты. Определить порты в docker-compose.yml

Режимы запуска контейнеров:

- Контейнер жив пока работает программа (--rm)

- Запуск container in demon mode (-d)

- Интерактивный режим (-it)

Docker volume create kong-vol

Docker никогда не удаляет data volumes, даже если контейнеры, которые их создали, удалены.

Для того чтобы посмотреть список осиротевших томов, используйте команду:

docker volume ls -qf dangling=true

Для удаления таких томов:

docker volume rm $(docker volume ls -qf dangling=true)

ifconfig - покажет сети контейнеров

/bin/true/

/bin/bash/ # по умолчанию

Контейнеры – это абстракция, они реализуются благодаря возможностям ядра Linux: namespaces и cgroups.

Существует большое количество реализаций контейнеров: Docker, Rocket, LXC, OpenVZ и другие.

Команда images это alias для команды image ls(docker images)

Для запуска контейнера из образа можно использовать как имя образа, так и его ID

Может существовать образ, для которого нет ни одного контейнера

Операции записи в контейнере приводят к увеличению размера контейнера

Если слои файловой системы нескольких образов совпадают, они являются общими для этих образов

Узнать, какой storage driver используется на вашей системе, можно с помощью команды docker info

Благодаря использованию copy-on-write невозможно изменение слоев файловой системы, входящих в образ контейнера

Использование layered-file-system не увеличивает производительность записи из контейнера на диск

Запуск приложения в контейнере позволяет защитить операционную систему от внесения изменений

Файлы машины, в которой запускается контейнер, без дополнительной настройки не доступны из контейнера

Один контейнер - один процесс/приложение.

/----------------------Commands-----------------------/

[https://habr.com/en/company/flant/blog/336654/](https://habr.com/en/company/flant/blog/336654/) # список команд

docker pull ubuntu:14.04

docker images

docker run -it --name <container_name> ubuntu:14.04 /bin/true # запустит контейнер с именем <container_name>, из образа ubuntu:14.04

docker ps -a

docker commit --change='CMD ["usr/bin/python3"]' create-image-from-me myimage # CMD or ENTRYPOINT поменяет точку входа

docker run -it --rm --name new_container myimage # запустит контейнер с именем new_container из образа myimage

docker stop <container_id>

docker stop $(docker ps -a -q)

docker rm $(docker ps -a -q)

docker rename <container_name> <new_container_name>

docker rm <container_id> # remove container

Docker rmi <image_id> # remove images

docker images -a | grep "none" | awk '{print $3}' | xargs docker rmi # remove images by pattern

docker rmi $(docker images -f "dangling=true" -q) # alternative command

docker run my_image -it bash # запустит контейнер, а затем запустит в нём bash

docker run -it --name <container_name> ubuntu:14.04 /bin/bash # run bash command line

docker commit <container_name> <image_name> # create image from container

docker history <image_name> # покажет историю операций при создании образа

sudo docker exec -it <container-name> bash

sudo docker inspect <container-name>

docker logs <container-name>

 docker logs --follow <container-name> # чтобы следить за логами работающей программы

docker volume ls  —  показывает список томов, которые являются предпочитаемым механизмом для сохранения данных, генерируемых и используемых контейнерами Docker.

docker port <container> - Публичные порты

systemctl daemon-reload

systemctl enable docker

systemctl start docker

systemctl status docker

dockerd -D

/---Connection between containers(network)---/

ifconfig # покажет конфигурацию сетей

Взаимодействия между контейнерами

В Docker предустановлены три сети: none, bridge и host

Если сеть не задана явно, контейнер попадает в сеть bridge

docker network ls # покажет доступные сети

-bridge - по умолчанию контейнеры используют этот тип взаимодействия. Обращаться к контейнерам только через IP

-host - сетевое устройство хоста переносится напрямую в контейнер.

-none - у контейнера не будет сетевых интерфейсов, кроме Loopback

на первом контейнере устанавливаем прослушивание по порту

nc -l 12345

На втором контейнере отправляем сообщение

nc ip_first_container 12345

"Hello"

Недостатки Bridge:

Обращение по IP-адресам.

/---Создание сетей---/

Для изоляции от других контейнеров, ест доступ в интернет, возможность обращаться к контейнерам по имени

docker network create net_name # create net

docker network inspect net_name # check parameters

docker run -it --rm --name one --network=net_name ubuntu:14.04 #

docker run -it --rm --name two --network=custom ubuntu:14.04 #

ping one

/---CMD/ENTRYPOINT/RUN---/

ENTRYPOINT определяет команду которая будет выполнятся при старте контейнера.

CMD определяет аргументы которые будут переданы в ENTRYPOINT

The ENTRYPOINT of an image is similar to a COMMAND because it specifies what executable to run when the container starts, but it is (purposely) more difficult to override

Монтировать файл сокета докера внутрь проверяющего контейнера.

RUN - считайте, что он работает только внутри dockerfile, во время создания образа, имеет отношение к аргументам при билде (--build-arg) образа, но не имеет к аргументам при запуске контейнера - это вотчина CMD и ENTRYPOINT.

RUN, ENV, ARG работают при сборке образа, а вам нужно забрать и использовать аргумент во время старта контейнера.

Both CMD and ENTRYPOINT instructions define what command gets executed when running a container. There are few rules that describe their cooperation.

Dockerfile should specify at least one of CMD or ENTRYPOINT commands.

ENTRYPOINT should be defined when using the container as an executable.

CMD should be used as a way of defining default arguments for an ENTRYPOINT command or for executing an ad-hoc command in a container.

CMD will be overridden when running the container with alternative arguments.

ENTRYPOINT следует определять при использовании контейнера в качестве исполняемого файла.

ENTRYPOINT указывает команду, которая всегда будет выполняться при запуске контейнера.

Основной целью CMD является предоставление значений по умолчанию для исполняемого контейнера.

CMD указывает аргументы, которые будут переданы в ENTRYPOINT.

Если вы используете режим shell для ENTRYPOINT, CMD игнорируется.

При использовании режима exec для ENTRYPOINT аргументы CMD добавляются в конце.

ENTRYPOINT ["/bin/echo", "Hello, $name"]

All three instructions RUN, CMD and ENTRYPOINT support two different forms: the shell form and the exec for.

В формате exec не получается, если судить по справке по другому передать аргумент нельзя.

If CMD is defined from the base image, setting ENTRYPOINT will reset CMD to an empty value. In this scenario, CMD must be defined in the current image to have a value.

Note: Unlike the shell form, the exec form does not invoke a command shell. This means that normal shell processing does not happen.

Если не завязываться на shell-скрипт, то можно сделать через ﻿`shell form + if-else + $0`

Trying to use the shell form, or mixing-and-matching the shell and exec forms will almost never give you the result you want.

Вы хотите создать образ, соответствующий ubuntu:14.04, но изменить его так, чтобы в качестве точки входа испoльзовался python3. У вас в системе присутствует контейнер с именем create-image-from-me, созданный из ubuntu:14.04 командой:

docker run -it --name create-image-from-me ubuntu:14.04 /bin/true

docker commit --change='CMD ["python3"]' create-image-from-me myimage

В Dockerfile избегайте команд  apt-get upgrade и  apt-get dist-upgrade, оптимальный синтаксис команды для установки пакетов выглядит так:

RUN apt-get update && apt-get install -y \

    bzr \

    cvs \

    git \

    mercurial \

    subversion

==================tdmd===================

docker build --tag tdmd . # create image

docker run -d -v $(pwd):/opt --name tdmd_1 tdmd

==================dumper===================

FROM python:3.7

ENV PYTHONUNBUFFERED 1

WORKDIR /opt

COPY dumper.py /opt/dumper.py

ADD requirements.txt /opt/requirements.txt

RUN pip3 install -r /opt/requirements.txt && apt-get clean

RUN apt-get update && apt-get install -y vim

CMD python3 /opt/dumper.py

sudo docker build --tag dumper . # create image

sudo docker run -d -v $(pwd):/opt --name dumper_<name> dumper

# or add to docker-compose

  dumper_ivlab:

    build:

      context: /home/rus/webapps/dumper

    restart: always

    container_name: dumper_ivlab

    volumes:

      - /home/rus/webapps/dumper:/opt

    expose:

      - "8210"

    ports:

      - "8210:8210"

FROM python:3.7

ENV PYTHONUNBUFFERED 1

WORKDIR /opt

COPY dumper.py /opt/dumper.py

ADD requirements.txt /opt/requirements.txt

RUN pip3 install -r /opt/requirements.txt && apt-get clean

  dumper_<name>:

    build:

      context: /home/rus/webapps/dumper

    restart: always

    container_name: dumper_<name>

    command: sh -c "python3 /opt/dumper.py rack_6 rack_6 IVLAB 8210"

    volumes:

      - /home/rus/webapps/dumper:/opt

    expose:

      - "8210"

    ports:

      - "8210:8210"

===================== dockerfile examples===============================

FROM ubuntu:16.04

MAINTAINER Rus

RUN apt-get update && apt-get install -y --no-install-recommends \

              python3.5 \

           python3-pip \

   python-setuptools \

           && \

    apt-get clean && \

    rm -rf /var/lib/apt/lists/*

WORKDIR /opt

COPY ack.py /opt/ack.py

COPY requirements.txt /opt/requirements.txt

RUN pip3 install -r requirements.txt

CMD python3 ack.py

FROM python:3.7

ENV PYTHONUNBUFFERED 1

RUN mkdir /config

ADD /config/requirements.txt /config/

WORKDIR /src

COPY ./ ./

RUN pip3 install -r /config/requirements.txt && apt-get clean

sudo docker build --tag ack_messages .

sudo docker run -d -v $(pwd):/opt --name tdmd tdmd

FROM ubuntu:14.04 #

LABEL maintainer = "Rus"

ENTRYPOINT if [ "$0" = "/bin/sh" ]; then echo "Hello World!"; elif [ $0 != "/bin/sh" ]; then echo "Hello" $0"!"; fi

#ENTRYPOINT ["/bin/echo", "Hello"]

#CMD ["World!"]

======================RSS==========================

FROM ubuntu:16.04

MAINTAINER Rus

RUN apt-get update && apt-get install -y --no-install-recommends \

   firefox \

   build-essential \

   libssl-dev \

           libffi-dev \

   libxml2-dev \

           libxslt1-dev \

   python3-dev \

   python3-lxml \

   zlib1g-dev \

   cron \

            rsyslog \

              python3.5 \

           python3-pip \

           libxslt-dev \

   lib32ncurses5-dev \

   python-setuptools \

           && \

    apt-get clean && \

    rm -rf /var/lib/apt/lists/*

WORKDIR /opt

COPY mc.py /opt/mc.py

COPY requirements.txt /opt/requirements.txt

COPY geckodriver /opt/geckodriver

RUN chmod +x /opt/geckodriver

RUN mv /opt/geckodriver /usr/local/bin/

RUN export PATH=$PATH:/usr/local/bin/geckodriver

RUN pip3 install -r requirements.txt

# Copy cron file to the cron.d directory

COPY rss-cron /etc/cron.d/rss-cron

# Give execution rights on the cron job

RUN chmod 0644 /etc/cron.d/rss-cron

# Apply cron job

RUN crontab /etc/cron.d/rss-cron

CMD service rsyslog start && service cron start && tail -f /var/log/syslog

#CMD ["cron", "-f"]

sudo docker build --tag rss_cron .

sudo docker run -it -v $(pwd):/opt --name rss_cron --network=nginx-proxy rss_cron

sudo docker run -d -v $(pwd):/opt --name rss_cron --network=nginx-proxy rss_cron

sudo docker exec -it container bash

sudo docker run -d -p 8003:80 -v /home/rus/RSS:/opt --network=nginx-proxy --name nginx_rss nginx:latest

# cron file

50 0 * * * root python3 /opt/mc.py >/dev/null 2>&1

# @daily python3 /opt/mc.py

/etc/init.d/cron reload

crontab -e

crontab -l

/etc/cron.d/

grep CRON /var/log/syslog

(CRON) info (No MTA installed, discarding output)

>/dev/null 2>&1 # добавить к каждому заданию

Нужно было указать путь сохранения файлов т.к. я запускал скрипт от root, то все файлы сохранялись в папку root