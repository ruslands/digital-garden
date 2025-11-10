[https://habr.com/en/company/selectel/blog/245515/](https://habr.com/en/company/selectel/blog/245515/)

Geras

Druid

RRDtool

OpenTSDB  
Axibase

Chronix

Cube

Heroic

InfluxDB

Kairosdb

Newts

Prometheus

TrailDB

Riak-TS Riak TS

Akumuli

Rhombus

Dalmatiner

Blueflood

Timely

Для хранения временных рядов сегодня всё чаще используются так называемые NoSQL базы данных — как популярные HBase и Cassandra, так и более специализированные решения — например, OpenTSDB, KairosDB и Acunu.

Все перечисленные выше БД работают на базе инфраструктуры Hadoop, и для их нормального функционирования требуется огромное количество зависимостей.

B качестве низкоуровневого хранилища пар «ключ-значение» в InfluxDB используется база данных LevelDB. Для этой цели можно также использовать RocksDB (по утверждению разработчиков InfluxDB

InfluxDB может использоваться в качестве бэкенда для Graphite. Поддерживается также возможность работы с дашбордом для метрик Grafana

Несомненным плюсом InfluxDB являются и широкие возможности интеграции с другими программными продуктами — например, инструментом для обработки логов Fluentd, демонами для сбора статистики CollectD и и StatsD, фреймворками для мониторинга Sensu и Shinken.