E — [Elasticsearch](https://www.elastic.co/products/elasticsearch), L — [Logstash](https://www.elastic.co/products/logstash), K — [Kibana](https://www.elastic.co/products/kibana)

Filebeat

Filebeat capture and ship file logs --> Logstash parse logs into documents --> Elasticsearch store/index documents --> Kibana visualize/aggregate

Мы собираем логи файлбитом. Он не умеет парсить, данные в логе прилетают как текст,

То есть 100 это текст, сложить текст и текст нельзя.

Для этого надо впиливать логстеш, который может распарсить текст в цифры, и соответственно положить в эластик, а кибана в свою очередь аггрегировать

Убрать filebeat?

Его нельзя убрать)) он отправляет логи, его можно заменить на fluentd, тогда сможем убрать логстеш, но нагрузка на боевом сервере вырастет и саппортить его так себе удовольствие

Filebeat

выполнить две основные задачи: указать, какие файлы брать и куда отправлять результат. В установках по умолчанию Filebeat собирает все файлы в пути /var/log/*.log, это означает, что Filebeat соберет все файлы в каталоге /var/log/, заканчивающиеся на .log.

Logstash

[https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns](https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns)

logstash.conf has 3 sections -- input / filter / output, simple enough, right?

Input plugins

[https://www.elastic.co/guide/en/logstash/current/input-plugins.html](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)

Filter plugins

[https://www.elastic.co/guide/en/logstash/current/filter-plugins.html](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)

Dev tools

DELETE cart_mcs-2020.09.

input {

beats {

port => 5044

}

}

## Add your filters / logstash plugins configuration here

filter {

if [container][name] in ["cart_mcs","user_mcs","warehouse_mcs"] and "stderr" in [stream] {

mutate {

remove_field => ["@version","tags","agent","log","input","host","ecs"]

}

grok {

match => {

"message" => [

'DEBUG: datetime: %{TIMESTAMP_ISO8601:datetime} function_name: %{WORD:function} data: %{GREEDYDATA:data}'

]

}

}

}

if [container][name] not in ["cart_mcs","user_mcs","warehouse_mcs"] or "stderr" not in [stream] {

drop { }

}

}

output {

##Debug

stdout { codec => rubydebug }

# elasticsearch {

# hosts => "elasticsearch:9200"

# user => "elastic"

# password => "w6JzZu69CnSR2c"

# index => "%{[container][name]}-%{+YYYY.MM.dd}"

# }

}