1. docker exec -it influxdb influx # run influx client
2. CREATE USER admin WITH PASSWORD 'admin' WITH ALL PRIVILEGES

CREATE DATABASE queue_1

SHOW DATABASES

USE queue_1

curl "[http://localhost:8086/query](http://localhost:8086/query)" --data-urlencode "q=CREATE USER admin WITH PASSWORD 'admin' WITH ALL PRIVILEGES"

Разница между field и tag в том, что по tag эффективнее формируются WHERE запросы, но минус в том, что он обязательно string.

В field хранятся данные, ктоторые обычно не нужно фильтровать, а нужно обрабатывать. is_night и value из таких данных.

============InfluxdDB============

curl -sL -I localhost:8086/ping

sudo docker network create influxdb

  influxdb :

    image: influxdb:1.7.6

    restart: always

    container_name: influxdb

    volumes:

      - /home/rus/influxdb:/var/lib/influxdb   

    ports:

      - "8086:8086"

      - "8083:8083"

docker run --name=influxdb_test --restart=always -d -p 8087:8086 -p 8084:8083 -p 2002:2003 \

                        -v /db/influx_test:/var/lib/influxdb \

                        -e INFLUXDB_GRAPHITE_ENABLED=true \

                        -e INFLUXDB_ADMIN_ENABLED=true \

                        influxdb

docker run --name=influxdb_test --restart=always --net=influxdb -d -p 8086:8086 -p 8083:8083 -p 2003:2003 \

                        -v /home/rus/influxdb:/var/lib/influxdb \

                        -e INFLUXDB_GRAPHITE_ENABLED=true \

                        -e INFLUXDB_ADMIN_ENABLED=true \

                        influxdb

sudo docker run -d -p 8888:8888 \

      --name=chronograf \

      --net=influxdb \

      -v $PWD:/var/lib/chronograf \

      chronograf --influxdb-url=http://185.87.193.141:8086

8086 HTTP API port

8083 Administrator interface port, if it is enabled

2003 Graphite support, if it is enabled

Make table for each queue

One table include 16 ID

Tags are indexed and fields are not indexed. This means that queries on tags are more performant than those on fields.

1. Send data to influxDB. 2 options

 - mqttwarn -> influxDB

 - pika + python_influxDB client

2. Grafana get data from influxdb

 - [https://grafana.com/docs/features/datasources/influxdb/](https://grafana.com/docs/features/datasources/influxdb/)

pip3 install influxdb

docker run --name=influxdb_new --restart=always -d -p 8010:8086 -p 8020:8083 -p 2000:2003 \

                        -v /db_test/ifx_new:/var/lib/influxdb \

                        -e INFLUXDB_GRAPHITE_ENABLED=true \

                        -e INFLUXDB_ADMIN_ENABLED=true \

                        influxdb