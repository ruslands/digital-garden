Если вам для просмотра текущих ообщений в топиках, то можно Offset Explorer использовать

akhq

Pykafka

[http://docs.confluent.io/current/clients/index.html](http://docs.confluent.io/current/clients/index.html)

Docs

[https://kafka.apache.org/intro](https://kafka.apache.org/intro)

If we want to customize any Kafka parameters, we need to add them as environment variables in docker-compose.yml

If we want to have Kafka-docker automatically create topics in Kafka during creation, a KAFKA_CREATE_TOPICS

KAFKA_CREATE_TOPICS: “Topic1:1:3,Topic2:1:1:compact”

Here, we can see Topic 1 is having 1 partition as well as 3 replicas, whereas Topic 2 is having 1 partition, 1 replica, and also a cleanup.policy which is set to compact.

Apache [Kafka](https://www.bigdataschool.ru/wiki/kafka) использует [ZooKeeper](https://www.bigdataschool.ru/wiki/zookeeper) для хранения метаданных о разделах (partitions) своих топиков (topics) и брокерах, а также для выбора брокера в качестве контроллера Кафка [2]. Проще говоря, Зукипер информирует каждого брокера [Kafka](https://www.bigdataschool.ru/wiki/kafka)о текущем состоянии кластера. Благодаря этого каждому клиенту [Kafka](https://www.bigdataschool.ru/wiki/kafka) (издателю/подписчику) нужно всего лишь подключиться к какому-либо брокеру, а обновление метаданных у него произойдет автоматически

 running the command with no arguments will display usage information documenting them in more detail.

/opt/kafka/bin

find ~/ -type f -name "postgis-2.0.0"

# get number of messages in topic

bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic RSS --time -1

# create a topic "test"

bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test

# check all topic

bin/kafka-topics.sh --list --bootstrap-server localhost:9092

# describe topics

bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic my-replicated-topic

/bin/kafka-topics.sh --create --topic topic \ --partitions 4 --zookeeper $ZK --replication-factor 2