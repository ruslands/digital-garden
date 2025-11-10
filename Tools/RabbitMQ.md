sudo docker run -d --hostname rabbit-mqtt-test --name rabbit_test -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro -v /home/rus/rabbitmq_test/data:/var/lib/rabbitmq -p 8041:15672 -p 8051:5672 -p 8090:1883 rabbitmq:3-management

Ports:

8080-15672 - admin

8051-5672  - amqp for dumper

8090:1883  - mqtt for esp32

sudo docker cp rabbitmq_message_timestamp-20170830-3.7.x.ez some-rabbit:opt/rabbitmq/plugins

docker exec -it some-rabbit bash

rabbitmq-plugins enable rabbitmq_message_timestamp

rabbitmq-plugins enable rabbitmq_mqtt

rabbitmq-plugins list

rabbitmqctl add_user mqtt-test mqtt-test

rabbitmqctl set_permissions -p / mqtt-test ".*" ".*" ".*"

rabbitmqctl set_user_tags mqtt-test administrator

curl -i -u mqtt-test:mqtt-test [http://185.87.193.141:8041/api/whoami](http://185.87.193.141:8041/api/whoami)

Also I have editted iptables.like -> iptables -A INPUT  -m tcp -p tcp --dport 8080 -j ACCEPT

docker logs some-rabbit # check logs

 -v /etc/timezone:/etc/timezone:ro

 -v /etc/localtime:/etc/localtime:ro

==new info==

RabbitMQ Web MQTT Plugin [https://www.rabbitmq.com/web-mqtt.html](https://www.rabbitmq.com/web-mqtt.html)

/etc/rabbitmq/rabbitmq.conf add timeout/keepalive

[https://www.rabbitmq.com/networking.html#tcp-keepalives](https://www.rabbitmq.com/networking.html#tcp-keepalives)

[https://stackoverflow.com/questions/41274754/rabbitmq-keepalive-timeout-error](https://stackoverflow.com/questions/41274754/rabbitmq-keepalive-timeout-error)

# Chronograf

============Grafana============

sudo docker run -d --name=grafana -p 3000:3000 grafana/grafana

============RabbitMQ============

При этом никакой rabbitmqctl force_boot и тому подобное не поможет. Делаем следующее. Идём в /var/lib/rabbitmq/mnesia и копируем базу оттуда в сухое, тёплое место. Старую базу в mnesia/ удаляем. Далее, выполняем: service rabbitmq-server restart. Именно перезапуск, а не старт и не останов. Иначе не поднимается демон. Далее, проверяем ps-ом, что кролик стартанул. После чего делаем: systemctl rabbitmq-server stop. Далее, возвращаем базу на место, удалив то, что RabbitMQ сгенерил в каталоге mnesia/ при повторном перезапуске. Далее, как обычно: service rabbitmq-server start. Если всё сделано верно, то нода в рабочем состоянии появится в management-консоли.

RabbitMQ - полноценный сервер очередей, имеющий под капотом "свою" базу данных. Redis - база данных, над которой можно построить сервер очередей. Строить сервер очередей над Redis имеет смысл, имхо, если полноценный сервер не нужен, а Redis уже используется как база данных.

RabbitMQ поддерживает MQTT сразу после установки, существуют различия в публикуемых сообщениях при их издании через MQTT и AMQP, которые подчёркивают имеющиеся значения предложений каждого из протоколов. Будучи протоколом с малым весом, MQTT лучше приспособлен для ограниченного оборудования без надёжных сетевых соединений, в то время как AMQP разработан для того чтобы быть более гибким, но при этом требует более устойчивых и надёжных сетевых сред.

Разные типы exchange(fanout, direct, topic, consistent-hash) имеют разную скорость обработки. Average 35.000 mps(messages per second).

Fanout (разветвляющий). Направляет во все очереди и обменники, имеющие привязку к exchange’у Стандартная подмодель Pub.

Direct (прямой). Маршрутизирует сообщения на основе ключа маршрутизации, который несет с собой сообщение, задается паблишером. Ключ маршрутизации — короткая строка. Прямые обменники отправляют сообщения в очереди/exchange’и, у которых есть ключ сопряжения, который точно соответствует ключу маршрутизации.

Topic (тематический). Маршрутизирует сообщения на основе ключа маршрутизации, но позволяет использовать неполное соответствие (wildcard).

Header (заголовочный). RabbitMQ позволяет добавлять к сообщениям заголовки получателей. Заголовочные exchange’и передают сообщения в соответствии с этими значениями заголовка. Каждая привязка включает в себя точное соответствие значений заголовка. К привязке можно добавить несколько значений с ЛЮБЫМИ или ВСЕМИ значениями, необходимыми для соответствия.

Consistent Hashing (консистентное хэширование). Это обменник, который хэширует либо ключ маршрутизации, либо заголовок сообщения, и отправляет только в одну очередь. Это полезно, когда вам нужно соблюсти гарантии порядка обработки и при этом иметь возможность масштабировать получателей.

Префиксные деревья и недетерминированные конечные автоматы

Нет пакетной обработки, но можно реализовать самостоятельно

Есть пакетная обработка Kafka,zeromq

большинство из нас не имеет дел с масштабами, при которых у RabbitMQ появляются проблемы.

экстремальная масштабируемость (и это прерогатива Kafka)

RabbitMQ реализует AMQP 0.9.1 и предлагает другие протоколы, такие как STOMP, MQTT и HTTP

Паблишеры (publishers) отправляют сообщения на exchange’и

Exchange’и отправляют сообщения в очереди и в другие exchange’и

RabbitMQ отправляет подтверждения паблишерам при получении сообщения

Получатели (consumers) поддерживают постоянные TCP-соединения с RabbitMQ и объявляют, какую очередь(-и) они получают

RabbitMQ проталкивает (push) сообщения получателям

Получатели отправляют подтверждения успеха/ошибки

После успешного получения, сообщения удаляются из очередей

Независимо от того, сколько у вас получателей, RabbitMQ обеспечит доставку сообщений только одному получателю.

RabbitMQ проталкивает (push) сообщения получателям (существует также API для выгрузки (pull) сообщений из RabbitMQ, но в настоящий момент эта функциональность устарела). Это может переполнить получателей, если сообщения прибудут в очередь быстрее, чем получатели могут их обработать. Чтобы этого избежать, каждый получатель может настроить предел предварительной выборки (также известный как предел QoS). По сути, предел QoS это ограничение на количество скопившихся неподтвержденных получателем сообщений. Это действует как предохранитель, когда получатель начинает отставать.

Rabbit MQTT не имеет концепции очередей или обменов... просто иерархическая структура темы. Плагин публикует сообщения MQTT на тему обмена (по умолчанию amq.topic), а затем пользователи Rabbit читают сообщения из очередей, привязанных к обмену. Обратите внимание, что плагин будет конвертировать между MQTT / разделителем тем с помощью Rabbit . разделитель.

If you want a consumer using the Pika library to receive MQTT messages that consumer must subscribe to the appropriate queue to which the MQTT messages are being published. Comprehensive documentation of how MQTT and AMQP can interoperate is available here.

You then say "I want to publish the same message to my own exchange". If you wish to use your own exchange instead of amq.topic, please see the "Custom Exchanges" section of this document. You must specify the name of the exchange in the rabbitmq.config file and create the exchange prior to publishing any messages. Note that this custom exchange must be a topic exchange.

The RabbitMQ documentation is a good resource and I suggest searching there when you have questions.

Routing key это Topic

=====================Kafka======================

Kafka — это распределенный реплицированный журнал фиксации изменений (commit log). У Kafka’и нет концепции очередей, что сначала может показаться странным, учитывая, что его используют в качестве системы обмена сообщениями.

Вместо того, чтобы помещать сообщения в очередь FIFO и отслеживать статус этого сообщения в очереди, как это делает RabbitMQ, Kafka просто добавляет его в журнал, и на этом все.

Сообщение остается, вне зависимости от того, будет ли оно получено один или несколько раз. Удаляется оно в соответствии с политикой удерживания данных (retention policy, также называемый window time period)

Каждый получатель отслеживает, где она находится в журнале: имеется указатель на последнее полученное сообщение и этот указатель называется адресом смещения. Получатели поддерживают этот адрес через клиентские библиотеки, и в зависимости от версии Kafka адрес сохраняется либо в ZooKeeper, либо в самой Kafka’е.

Отличительная особенность модели журналирования в том, что она мгновенно устраняет множество сложностей, касающихся состояния доставки сообщений и, что более важно для получателей, позволяет им перематывать назад, возвращаться и получать сообщения по предыдущему относительному адресу. Например, представьте, что вы разворачиваете сервис, который выставляет счета, учитывающие заказы, размещаемые клиентами. У службы случилась ошибка, и она неправильно рассчитывает все счета за 24 часа. С RabbitMQ в лучшем случае вам нужно будет как-то переопубликовать эти заказы только на сервисе счетов. Но с Kafka вы просто перемещаете относительный адрес для этого получателя на 24 часа назад.

===========Install 3rd-party plugin==============

otp/lib/rabbitmq/plugins

docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-

docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

sudo docker cp rabbitmq_message_timestamp-20170830-3.7.x.ez rabbit_mqtt:opt/rabbitmq/plugins

rabbitmq-plugins enable rabbitmq_message_timestamp

=========================

apt-get update

apt-get install systemd

systemctl status rabbitmq

docker pull rabbitmq

/etc/rabbitmq/rabbitmq.conf

nmap -p 8080 185.87.193.141

====================================

sudo docker logs rabbit_mqtt

 home dir       : /var/lib/rabbitmq

 config file(s) : /etc/rabbitmq/rabbitmq.conf

 cookie hash    : UC2EBU3knikwmtSKXqYRYQ==

 log(s)         : <stdout>

 database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit-mqtt

====================================

One of the important things to note about RabbitMQ is that it stores data based on what it calls the "Node Name", which defaults to the hostname. What this means for usage in Docker is that we should specify -h/--hostname explicitly for each daemon so that we don't get a random hostname and can keep track of our data:

Need to create the advanced.config file and set the queue_index_max_journal_entries to the needs.

#mount directory

-v /home/rus/rabbitmq/data:/var/lib/rabbitmq

#set time zone

volumes:

- "/etc/timezone:/etc/timezone:ro"

- "/etc/localtime:/etc/localtime:ro"

or

-v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro

sudo docker run -d --hostname rabbit-mqtt --name rabbit -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro -v /home/rus/rabbitmq/data:/var/lib/rabbitmq -p 8041:15672 -p 8051:5672 -p 8090:1883 rabbitmq:3-management

sudo docker cp rabbitmq_message_timestamp-20170830-3.7.x.ez rabbit:opt/rabbitmq/plugins

sudo docker exec -it rabbit bash

rabbitmq-plugins enable rabbitmq_mqtt

rabbitmq-plugins enable rabbitmq_message_timestamp

rabbitmq-plugins enable rabbitmq_management

rabbitmq-plugins list

# username and password are both "mqtt-test"

rabbitmqctl add_user mqtt-test mqtt-test

rabbitmqctl set_permissions -p / mqtt-test ".*" ".*" ".*"

rabbitmqctl set_user_tags mqtt-test management

# rabbitmqctl set_user_tags mqtt-test administrator

rabbitmqctl start_app

rabbitmqctl set_policy Lazy "^johnny_depp$" '{"queue-mode":"lazy"}' --apply-to queues

===============================

iptables -A INPUT  -m tcp -p tcp --dport 8089 -j ACCEPT

iptables -A INPUT  -m tcp -p tcp --dport 8080 -j ACCEPT # неуверен что это нужно делать

iptables -A INPUT  -m tcp -p tcp --dport 8082 -j ACCEPT # неуверен что это нужно делать

iptables -A INPUT  -m tcp -p tcp --dport 15672 -j ACCEPT # неуверен что это нужно делать

iptables -A INPUT  -m tcp -p tcp --dport 1883 -j ACCEPT # неуверен что это нужно делать

vi /etc/rabbitmq/rabbitmq.conf

mqtt.exchange = mice.exchange # change default exchanger for mqtt

curl -i -u mqtt-test:mqtt-test [http://185.87.193.141:8045/api/whoami](http://185.87.193.141:8045/api/whoami)

===============================

rabbitmqctl list_bindings

rabbitmqctl list_queues # what queues RabbitMQ has and how many messages are in them

rabbitmqctl list_exchanges # To list the exchanges on the server

sudo service rabbitmq-server restart

Offline change; changes will take effect at broker restart.

Queues and exchanges needs to be configured as durable in order to survive a broker restart. A durable queue only means that the queue definition will survive a server restart, not the messages in it.

Set message delivery mode to persistent:

Making a queue durable is not the same as making the messages on it persistent. Messages can be published either having a delivery mode set to persistent or transient. You need to set delivery mode to persistent when publishing your message, if you would like it to remain in your durable queue during restart.

=============RabbitMQ Management HTTP API==================

==========================MQTT=============================

Birth Message - которое сообщает миру что "я родился и живой"

w

Last Will and Testament (LWT) которое сообщает что после этого сообщения считать меня мертвым.

Keep Alive сообщения, которые сообщают брокеру что "я все еще живой" и стандартно посылаются каждые 60 секунд.

Quality Of Service

QoS=0 означает что мы один раз публикуем сообщение "мама я покакала" и нам пофиг услышали нас или нет

QoS=1 означает что мы будем публиковать сообщение "мама я покакала" до посинения, до тех пор пока нам не скажут что нас услышали и можно заткнуться уже.

QoS=2 означает что мы один раз сказали "мама я покакала", получили от мамы подтверждение того что она нас услышала, сообщили маме что мы узнали о том что мама нас улышала, а мама подтвердила, что она поняла что мы узнали о том что она услышала

Retain (0|1)

rabbit_mqtt_retained_msg_store_ets for RAM-based

rabbit_mqtt_retained_msg_store_dets for disk-based