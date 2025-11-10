Ansible: for software automation

Flowable is an open-source workflow engine written in Java that can execute business processes described in BPMN 2.0. It is an actively maintained fork of Activiti

Activiti is an open-source workflow engine written in Java that can execute business processes described in BPMN 2.0

Camunda is an open-source workflow and decision automation platform. Camunda Platform ships with tools for creating workflow and decision models, operating deployed models in production, and allowing users to execute workflow tasks assigned to them.

Apache Airflow - Автоматизация аналитических скриптов

Apache Camel - is an open source integration framework that empowers you to quickly and easily integrate various systems consuming or producing data.

Samza - allows you to build stateful applications that process data in real-time from multiple sources including Apache Kafka.

Apache Cordova - "Platform for building native mobile applications using HTML, CSS and JavaScript". Similar to Flutter

Redmine is a free and open source, web-based project management and issue tracking tool.

Zabbix - мониторинг сервера

Amplitude

Looker

Tableau

OCE - (Orchestrated Customer Engagement) designed by IQVIA.

Reltio MDM (master data management platform) is a next-generation multi-domain cloud MDM platform. It helps you deploy customer, organization, employee, assets, location, and product data masters within the same instance, breaking down data silos.

Marvel similar to Figma

POP App - extension for marvel

InVision design software

Origami Studio - prototyping, works as extension for Figma

Google BigQuery is a fully-managed, serverless data warehouse that enables scalable analysis over petabytes of data.

OWOX - BI combines data from your website, offline shop, Google Analytics, advertising services, call-tracking and CRM systems.

Google Data Studio - Unlock the power of your data with interactive dashboards and beautiful reports that inspire smarter business decisions.

С ботлнеками хорошо помогает такое направление DS как process mining. По логам событий из приложения можно строить пользовательские пути и смотреть где происходят лишние шаги пользователей, что изменить. Есть информация по направлению на Вики, есть бесплатные курсы, а также Сбер недавно выпустил свою библиотеку на питоне. (Информация есть на хабре в их блоге)

Visual paradigm

All-in-one UML, SysML, BPMN Modeling Platform for Agile, EA TOGAF ADM Process

Sparks Enterprise Architect

Orbus iServer - репозиторий документации

ktor - build asynchronous servers and clients in Kotlin

JOOQ - write sql on java

Gitops - ensures that a system's cloud infrastructure is immediately reproducible based on the state of a Git repository. Pull requests modify the state of the Git repository. Once approved and merged, the pull requests will automatically reconfigure and sync the live infrastructure to the state of the repository.

Iac - (Infrastructure-as-Code; Iac) — это подход для управления и описания инфраструктуры ЦОД через конфигурационные файлы, а не через ручное редактирование конфигураций на серверах или интерактивное взаимодействие.

Istio - Istio enables organizations to secure, connect, and monitor microservices, so they can modernize their enterprise apps more swiftly and securely. Istio manages traffic flows between services, enforces access policies, and aggregates telemetry data, all without requiring changes to application code.

Jaeger - Monitor and troubleshoot transactions in complex distributed systems

Prometheus - monitoring (node_exporter, grafana)

Victoriametrics - The High Performance Open Source Time Series Database & Monitoring Solution

Vault - Manage Secrets & Protect Sensitive Data. Secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API

Hortonworks - cloudera

Apache Ignite is a distributed database for high-performance computing with in-memory speed

Flower is a web based tool for monitoring and administrating Celery clusters

perf

grof

flamegraph - A flame graph visualizes a distributed request trace and represents each service call that occurred during the request's execution path with a timed, color-coded, horizontal bar. Flame graphs for distributed traces include error and latency data to help developers identify and fix bottlenecks in their applications.

callgrind - is a profiling tool that records the call history among functions in a program's run as a call-graph. By default, the collected data consists of the number of instructions executed, their relationship to source lines, the caller/callee relationship between functions, and the numbers of such calls.

GDB - project debugger, allows you to see what is going on `inside' another program while it executes -- or what another program was doing at the moment it crashed.

nettop - Network Traffic in MacOS

Dash and Bokeh uses AJAX to communicate between server and front, Bokeh uses websocket. Dash need to create the whole figure to update it, but bokeh only sends data to the client, and the data is in binary format. So Bokeh is much faster than Dash, I even created a video player in Bokeh, that can play at 20 frames per second.

Reddis  is an in-memory data structure project implementing a distributed, in-memory key-value database with optional durability. Redis supports different kinds of abstract data structures, such as strings, lists, maps, sets, sorted sets, HyperLogLogs, bitmaps, streams, and spatial indexes. Can be used as message broker

openvault

Helm is the package manager for Kubernetes. 

Rethinkdb

AVRO - Registry service. В этом проекте я использую Netflix Eureka (но есть еще Consul, Zookeeper, Etcd и другие)

Ribbon — это client-side балансировщик. По сравнению с традиционным, здесь запросы проходят напрямую по нужному адресу, что исключает лишний узел при вызове. Из коробки он интегрирован с механизмом Service Discovery, который предоставляет динамический список доступных инстансов для балансировки между ними.

Hystrix — это имплементация паттерна [Circuit Breaker](http://martinfowler.com/bliki/CircuitBreaker.html) — предохранителя, который дает контроль над задержками и ошибками при вызовах по сети. Основная идея состоит в том, чтобы остановить каскадный отказ в распределенной системе, состоящей из большого числа компонентов. Это позволяет отдавать ошибку как можно быстрее, не задерживаясь при запросе к зависшему сервису (давая ему восстановится).

Помимо контроля за размыканием цепи, Hystrix позволяет определить fallback-метод, который будет вызван при неуспешном вызове. Тем самым можно отдавать дефолтный ответ, сообщение об ошибке, и др.

На каждый запрос Hystrix генерирует набор метрик (таких как скорость выполнения, результат), что позволяет анализировать общее состоянее системы. Ниже будет рассмотрен мониторинг на основе данных метрик.

В инфраструктуре, состоящей из большого количества движущихся частей (каждая из которых может иметь по нескольку экземпляров), очень важно использовать систему централизированного сбора, обработки и анализа логов. Elasticsearch, Logstash и Kibana составляют стeк, с помощью которого можно эффективно решать такую задачу.

Готовая к запуску Docker-конфигурация ELK с Куратором и шаблонами для шипперов [доступна на гитхабе](https://github.com/sqshq/ELK-docker). Именно такая конфигурация, с небольшой кастомизацией и масштабированием, успешно работает в продакшене на моем текущем проекте для анализа логов, сетевой активности и мониторинга производительности серверов.

Sentry - monitoring. From error tracking to performance monitoring, developers can see what actually matters, solve quicker, and learn continuously about their applications - from the frontend to the backend.

Memcached

Celery

Hadoop - фреймворк для распределенных вычислений использующий парадигму MapReduce.

Что плохо в Hadoop - медленный за счет shuffle и каждая фаза map или reduce сбрасывает данные на локальный диск, а следующая заново засчитывает с диска.

SQOOP: помогает импортировать/экспортировать данные из реляционных баз данныхYARN: фреймворк для управления ресурсами кластера и менеджмента задач, в том числе включает фреймворк MapReduce.  
Hive: инструмент для SQL-like запросов над большими данными (превращает SQL-запросы в серию MapReduce–задач) Не подходит для real-time системах. Это не БД. Использует реляционную базу данных для хранения метаинформации. Фиксированная схема;  
Pig: высокоуровневая платформа, которая предоставляет высокоуровневый язык программирования PigLatin, который преобразуется в последовательности MapReduce-задач. Свободная схема;  
Hbase: распределенная Column-Based база данных;  
Cassandra: распределенная Column-Based база данных;  
ZooKeeper: сервис для синхронизации распределенных приложений;  
Mahout: библиотека и движок машинного обучения на больших данных.  
Airflow: is used for the scheduling and orchestration of data pipelines or workflows. Orchestration of data pipelines refers to the sequencing, coordination, scheduling, and managing complex data pipelines from diverse sources.  
OpenShift: is a cloud-based Kubernetes platform that helps developers build applications. It offers automated installation, upgrades, and life cycle management throughout the container stack — the operating system, Kubernetes and cluster services, and applications — on any cloud. OpenShift gives organizations the ability to build, deploy, and scale applications faster both on-premises and in the cloud. It also protects your development infrastructure at scale with enterprise-grade security.  
Ansible: is an open source IT automation engine that automates provisioning, configuration management, application deployment, orchestration, and many other IT processes.  
Rancher: Enterprise Kubernetes Management  
apache AB: утилита для нагрузочного тестирования  
Яндекс.Танк: утилита для нагрузочного тестирования  
Serverless Framework:  
Localstack  
Minio  
Openstack  
OpenTelemetry: requests tracing  
Redoc: is an open-source tool that generates API documentation from OpenAPI specifications

Zeromq: is an asynchronous messaging library, aimed at use in distributed or concurrent applications. It provides a message queue, but unlike message-oriented middleware, a ZeroMQ system can run without a dedicated message broker; the zero in the name is for zero broker.

Sensu - [https://sensu.io](https://sensu.io)

Shinken - [http://www.shinken-monitoring.org](http://www.shinken-monitoring.org)

Fluentd - [https://www.fluentd.org](https://www.fluentd.org)

Haproxy:

supervisor: Supervisor is a client/server system that allows its users to control a number of processes on UNIX-like operating systems.

/----Для потоковой обработки данных--------/  
Storm  
Spark  
Kafka

Puppet, Chef, Ansible, SaltStack - Кроссплатформенное клиент-серверное приложение, которое позволяет централизованно управлять конфигурацией операционных систем и программ, установленных на нескольких компьютерах. Написано на языке программирования Ruby. Наряду с Chef отмечается как одно из самых актуальных средств конфигурационного управления по состоянию на 2013 год.

AVRO - это система сериализации данных, созданная в рамках проекта Hadoop. Данные сериализуются бинарный формат с помощью предварительно созданной json-схемы при чем для десериализации также требуется схема (возможно — другая).

TypeSystem -

[https://www.encode.io/](https://www.encode.io/)

 is a comprehensive data validation library that gives you:

- Data validation.
- Object serialization & deserialization.
- Form rendering.
- Marshaling validators to/from JSON schema.
- Tokenizing JSON or YAML to provide positional error messages.
- 100% type annotated codebase.
- 100% test coverage.
- Zero hard dependencies.

F5 - load balancer

Circuit breakers - The circuit breaker pattern is one of those patterns, widely adopted in microservices architectures there are solutions available that help make applications resilient and fault tolerant – one such framework is Hystrix.The Hystrix framework library helps to control the interaction between services by providing fault tolerance and latency tolerance. It improves overall resilience of the system by isolating the failing services and stopping the cascading effect of failures.

Istio - is a service mesh, a configurable infrastructure layer for a Microservices application. It makes communication between service instances flexible, reliable, and fast, and provides service discovery, load balancing, encryption, authentication and authorization, support for the circuit breaker pattern, and other capabilities.

Fluentd - open source решение для обеспечения единого слоя логирования приложения. Позволяет собирать логи с разных слоёв приложения, а потом транслировать их в единый источник. 

Мы также исследовали, как будет вести себя fluentd, если у него не будет связи с Elasticsearch. В течение двух суток в шлюз непрерывно шли запросы, во fluentd отправлялись логи, но у fluentd был забанен IP Elastic. После восстановления связи fluentd отлично перегнал абсолютно все логи в Elastic.

ELB - Elastic load balancing

ZUUL