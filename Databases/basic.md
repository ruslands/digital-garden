Databases

Все базы данных внутри похожи и хранят данные в страницах

Хранимая процедура: это скомпилированная программа SQL, состоящая один или более операторов SQL, которая хранится и выполняется на сервере базы данных, вне приложения. Она хранится как исполняемый код в реляционной базе данных. Для создания хранимой процедуры применяется команда CREATE PROCEDURE или CREATE PROC. Существует несколько типов процедур - Системные, Пользовательские, Временные

Все действия по администрированию БД выполняются через системные процедуры.

Плюсы хранимых процедур - не нужно передавать sql запрос по сети

View vs Procedure

Views should be used to store commonly-used JOIN queries and specific columns to build virtual tables of an exact set of data we want to see. Stored procedures hold the more complex logic, such as INSERT, DELETE, and UPDATE statements to automate large SQL workflows.

Optimistic and pessimistic concurrency control

The optimistic method of concurrency control is based on the hypothesis that conflicts of database operations are rare and that it is better to let transactions run to completion and only check for conflicts before they are committed. In general, optimistic concurrency is more efficient when update collisions are expected to be infrequent; pessimistic concurrency is more efficient when update collisions are expected to occur often

Миграции - применение изменений к базе данных. Миграции используют транзакции. Транзакция это механизм перевода БД из одного согласованного состояния в другое согласованное состояние

Schedules - is a process of lining the transactions and executing them one by one
![[databases-1.jpg]]
database link: is a connection between two physical database servers that allows a client to access them as one logical database.

execution plan: when you execute a query, the optimizer generates an execution plan.

An execution plan describes a recommended method of execution for a SQL statement.

The plan shows the combination of the steps Oracle Database uses to execute a SQL statement. Each step either retrieves rows of data physically from the database or prepares them for the user issuing the statement.

optimizer: contains three components: the transformer, estimator, and plan generator.

Разные уровни работы с данными

- Слой доступа к данным

- Универсальность и Оптимальность

- SQL или другой QL

- Параллелизм и Оптимальность

- Управление транзакциями

- Слой хранения

- Параллелизм и Эффективность (Конкурентный доступ)

- Сериализация

- Надежность и Эффективность (Восстановление после сбоев)

- Восстановление

- Железо

- Надежность и Эффективность

- Интеграция с ядром ОС

![[databases-2.png]]

[https://habr.com/ru/post/328792/](https://habr.com/ru/post/328792/) - CAP theorem

В CAP говорится, что в распределенной системе возможно выбрать только 2 из 3-х свойств:

- C (consistency) — согласованность. Каждое чтение даст вам самую последнюю запись.
- A (availability) — доступность. Каждый узел (не упавший) всегда успешно выполняет запросы (на чтение и запись).
- P (partition tolerance) — устойчивость к распределению. Даже если между узлами нет связи, они продолжают работать независимо друг от друга.

![[databases-3.gif]]

Database transaction models: ACID and BASE

The CAP theorem states that it is impossible to achieve both consistency and availability in a partition tolerant distributed system (i.e., a system which continues to work in cases of temporary communication breakdowns).

The fundamental difference between ACID and BASE database models is the way they deal with this limitation.

- The ACID model provides a consistent system. (PostgreSQL, MySQL)
- The BASE model provides high availability.

ACID (atomicity, consistency, isolation, durability) is a set of properties of database transactions

A database transaction, by definition, must be 

Atomicity - it must either be complete in its entirety or have no effect whatsoever)

consistent  - гарантирует, что по мере выполнения транзакций, данные переходят из одного согласованного состояния в другое, то есть транзакция не может разрушить взаимной согласованности данных.

isolated - it must not affect other transactions, локализация пользовательских процессов означает, что конкурирующие за доступ к БД транзакции физически обрабатываются последовательно, изолированно друг от друга, но для пользователей это выглядит, как будто они выполняются параллельно.

durable (it must get written to persistent storage) - если транзакция завершена успешно, то те изменения в данных, которые были ею произведены, не могут быть потеряны ни при каких обстоятельствах.

Atomicity - Transaction Manager

Consistency - Application Programmer

Isolation - Concurrency Control Manager

Durability - Recovery Manager

Механизмы одновременного доступа к данным  
Контроль за целостностью данных (ACID)  
Универсальный язык доступа к данным (SQL)

A transaction is a sequence of one or more SQL operations that are treated as a unit. Transaction supports what is known as the ACID properties

Сериализация - для корректного выполнения транзакции

BASE (Basically Available, Soft State, Eventually Consistent)

Basically Available – Rather than enforcing immediate consistency, BASE-modelled NoSQL databases will ensure availability of data by spreading and replicating it across the nodes of the database cluster.

Soft State – Due to the lack of immediate consistency, data values may change over time. The BASE model breaks off with the concept of a database which enforces its own consistency, delegating that responsibility to developers.

Eventually Consistent – The fact that BASE does not enforce immediate consistency does not mean that it never achieves it. However, until it does, data reads are still possible (even though they might not reflect the reality).

Масштабирование: вертикальное и горизонтальное

Вертикальное - добавление ресурсов.

Горизонтальное - разбиение системы на более мелкие структурные компоненты и разнесение их по отдельным физическим машинам (или их группам)

Шардирование: техника вертикального масштабирования, разбиение данных на части на основе некого ключа шардинга и распределение по разным хранилищам(серверам).  
Репликация: данные с одного сервера базы данных постоянно копируются (реплицируются) на один или несколько других (называемые репликами). Для приложения появляется возможность использовать не один сервер для обработки всех запросов, а несколько.

Партиционирование: это разбиение таблиц, содержащих большое количество записей, на логические части по неким критериям. Горизонтальное партиционирование - нарезка построчно, вертикальное партиционирование нарезка по столбцам.

горизонтальное партиционирование — это когда все таблицы, на которые разделена основная таблица, лежат в одной и той же схеме, а когда на разных машинах — это уже шардирование.

Блокировки

Two phase locking(2pl) - транзакция берет блокировки. У этого типа блокировки возникает проблема с dead-lock. Решается с помощью dead-lock detection.

MV2PL - добавлено версионирование

WAL - write ahead log. Хранит лог записей, которые выполняются в транзакции, нужен для восстановления грязных страниц.

Checkpoint - для синхронизации грязных страниц на диск. При восстановлении идем до последнего checkpoint.

NoSQL - не имеет транзакции и блокировок. Нет параллелизма. Но некоторые noSQL бд начинают добавлять блокировки.

Нормализация

Нормализация – это процесс удаления избыточных данных.

Нормализация – это метод проектирования базы данных, который позволяет привести базу данных к минимальной избыточности.

- Устранения аномалий
- Повышения производительности
- Повышения удобства управления данными

Нормальная форма базы данных – это набор правил и критериев, которым должна отвечать база данных.

- Первая нормальная форма (1NF)
- Вторая нормальная форма (2NF)
- Третья нормальная форма (3NF)
- Четвертая нормальная форма (4NF)
- Пятая нормальная форма (5NF)

Первая нормальная форма (1NF)

- В таблице не должно быть дублирующих строк
- В каждой ячейке таблицы хранится атомарное значение (одно не составное значение)
- В столбце хранятся данные одного типа
- Отсутствуют массивы и списки в любом виде

Вторая нормальная форма (2NF)

- Таблица должна находиться в первой нормальной форме
- Таблица должна иметь ключ
- Все неключевые столбцы таблицы должны зависеть от полного ключа (в случае если он составной)

Транзитивная зависимость – это когда неключевые столбцы зависят от значений других неключевых столбцов.

Если в первой нормальной форме наше внимание было нацелено на соблюдение реляционных принципов, во второй нормальной форме в центре нашего внимания был первичный ключ, то в третьей нормальной форме все наше внимание уделено столбцам, которые не являются первичным ключом, т.е. неключевым столбцам.

Чтобы нормализовать базу данных до третьей нормальной формы, необходимо сделать так, чтобы в таблицах отсутствовали неключевые столбцы, которые зависят от других неключевых столбцов.

Иными словами, неключевые столбцы не должны пытаться играть роль ключа в таблице, т.е. они действительно должны быть неключевыми столбцами, такие столбцы не дают возможности получить данные из других столбцов, они дают возможность посмотреть на информацию, которая в них содержится, так как в этом их назначение.

SQL Dialects

SQL is a query language to operate on sets. It is more or less standardized, and used by almost all relational database management systems: SQL Server, Oracle, MySQL, PostgreSQL, DB2, Informix, etc.

PL/SQL is a proprietary procedural language used by Oracle

PL/pgSQL is a procedural language used by PostgreSQL

TSQL is a proprietary procedural language used by Microsoft in SQL Server.

О выборе SQL-баз данных

Не существует баз данных, которые подойдут абсолютно всем. Именно поэтому многие компании используют и реляционные, и нереляционные БД для решения различных задач. Хотя NoSQL-базы стали популярными благодаря быстродействию и хорошей масштабируемости, в некоторых ситуациях предпочтительными могут оказаться структурированные SQL-хранилища. Вот две причины, которые могут послужить поводом для выбора SQL-базы:

1. Необходимость соответствия базы данных требованиям ACID (Atomicity, Consistency, Isolation, Durability — атомарность, непротиворечивость, изолированность, долговечность). Это позволяет уменьшить вероятность неожиданного поведения системы и обеспечить целостность базы данных. Достигается подобное путём жёсткого определения того, как именно транзакции взаимодействуют с базой данных. Это отличается от подхода, используемого в NoSQL-базах, которые ставят во главу угла гибкость и скорость, а не 100% целостность данных.
2. Данные, с которыми вы работаете, структурированы, при этом структура не подвержена частым изменением. Если ваша организация не находится в стадии экспоненциального роста, вероятно, не найдётся убедительных причин использовать БД, которая позволяет достаточно вольно обращаться с типами данных и нацелена на обработку огромных объёмов информации.

О выборе NoSQL-баз данных

Если есть подозрения, что база данных может стать узким местом некоего проекта, основанного на работе с большими объёмами информации, стоит посмотреть в сторону NoSQL-баз, которые позволяют то, чего не умеют реляционные БД.

Вот возможности, которые стали причиной популярности таких NoSQL баз данных, как MongoDB, CouchDB, Cassandra, HBase:

1. Хранение больших объёмов неструктурированной информации. База данных NoSQL не накладывает ограничений на типы хранимых данных. Более того, при необходимости в процессе работы можно добавлять новые типы данных.
2. Использование облачных вычислений и хранилищ. Облачные хранилища — отличное решение, но они требуют, чтобы данные можно было легко распределить между несколькими серверами для обеспечения масштабирования. Использование, для тестирования и разработки, локального оборудования, а затем перенос системы в облако, где она и работает — это именно то, для чего созданы NoSQL базы данных.
3. Быстрая разработка. Если вы разрабатываете систему, используя agile-методы, применение реляционной БД способно замедлить работу. NoSQL базы данных не нуждаются в том же объёме подготовительных действий, которые обычно нужны для реляционных баз.

Типы NoSQL-баз данных

- Хранилище «ключ-значение» — в нём есть большая хеш-таблица, содержащая ключи и значения.  
    Примеры: Riak, Amazon DynamoDB
- Документоориентированное хранилище — хранит документы, состоящие из тегированных элементов.  
    Пример: CouchDB
- Колоночное хранилище — в каждом блоке хранятся данные только из одной колонки.  
    Примеры: HBase, Cassandra
- Хранилище на основе графов — сетевая база данных, которая использует узлы и рёбра для отображения и хранения данных. Пример: Neo4J

ORM (object relational mapping) -  maps between an Object Model and a Relational Database. a technique to query or perform CRUD (Create, Read, Update, Delete) operations to the database, mainly RDBMS (Relational Databases), using an object-oriented paradigm.

ODM (object document mapping) maps between an Object Model and a Document Database

OGM (object graph mapper)

Raw SQL vs Query builder vs ORM [https://lyz-code.github.io/blue-book/architecture/orm_builder_query_or_raw_sql/](https://lyz-code.github.io/blue-book/architecture/orm_builder_query_or_raw_sql/)

Query builders are libraries which are written in the programming language you use and use native classes and functions to build SQL queries. Query builders typically have a fluent interface, so the queries are built by an object-oriented interface which uses method chaining.

Pros

- Flexibility and performance same as Raw SQL

Cons

- SQL injections, change management same as RAW SQL

fluent interface: is an object-oriented API whose design relies extensively on method chaining/cascading

ORMs create an object for each database table and allows you to interact between related objects

Cons

- Flexibility, Performance

data lake:

data warehouse:

/--------HDFS--------/

Hue - hadoop user experience, представляет GUI интрефейс для программ из стека hadoop

РМД - отношение, атрибут, кортеж

Отношение обычно имеет простую графическую интерпретацию в виде таблицы,  
 столбцы которой соответствуют атрибутам, а строки — кортежам, а в  
«ячейках» находятся значения атрибутов в кортежах. Тем не менее, в  
строгой реляционной модели отношение не является таблицей, кортеж — это не строка, а атрибут — это не столбец[2][3].  
 Термины «таблица», «строка», «столбец» могут использоваться только в  
неформальном контексте, при условии полного понимания, что эти более  
«дружественные» термины являются всего лишь приближением и не дают точного представления о сути обозначаемых понятий[2][4].  
  
Какими недостатками обладает архитектура Master / Slave, применяемая при масштабировании Реляционных Баз Данных?  
Не подходит в случае большого потока запросов на изменение данных  
Тяжело гарантировать согласованность данных  
Master является узким местом - точка отказа (На самом деле да, все больше стараются избавится от такой зависимости от  
 мастера. В последних версиях узнать расположение региона можно без master (через zookeeper). Кроме того, появилась поддержка бекапа этих мастеров.)

Какими недостатками обладает Sharding, применяемый при масштабировании Реляционных Баз Данных?  
Тяжелая операция переразбиения на шарды       
Проблемы с выполнением операции Join

Варианты маштабируемости  
- Master-Slave  
- Sharding  
 

РБД минусы  
- Проблемы с маштабируемостью (sharding, master-slave)  
- Тяжело хранить иерархические структуры

NoSQL - это не реализация конкртеной базы данных, а принципы по которым можно реализовать БД.  
главная черта NoSQL бд  
- масштабируемость  
- многопроцессорность  
- различные типы данных  
- Нефиксированная схема БД

типы данных NoSQL:  
key/value - модель данных хэш-таблица (Amazon S3, Voldemort)  
column based - разряженная матрица (HBase, Cassandra)  
Document Based - модель данных дерево для хранения иерархической информации (MongoDB, OrientDB)  
Graph Based - модель данных граф (Allegro, InfiniteGraph)

Данный БД работают на кластере, расположены на нескольких серверах

CAP Theorem - БД может иметь только два из этих трех свойств.  
- Consistency - непротиворечивость данных  
- Availability - доступность данных           
- Partitionability - разделяемость данных на изолированные части.

NoSQL свойства BASE вместо ACID  
- Basically Available - базовая доступность  
- Soft State - гибкое состояние  
- Eventually Consistent - согласованность в конечном счете

HBase  
Распределенная БД  
Работате на кластере  
Легко масштабируется горизонтально за счет sharding  
NoSQL БД  
Не предоставляет SQL-доступ  
Не предоставляет реляционной модели  
Column-oriented хранилище данных  
Плохо подходит для доступа к данным на основе текстовых запросов (LIKE)

Большой минус HDFS - нельзя получить произвольный доступ к середине файла. HBase решает эту проблему.

Использовать РБД вместо NoSQL  
1. Присутсвует транзакционность  
2. Реляцтлнная аналитика (group by, join ..)  
3. Данные вмещаются на одном сервере

Доступ к значению  
Value = Table + RowKey + Family + Column + TimeStamp

ColumnFamily задает  
- Сжатие  
- Количество версий данных  
- Time to live  
- Хранение в памяти In-Memory  
- Храниться в отдельном файле        

Regions - sharding        

HFile - файл в HBase, в котором хранятся данные таблицы.

Cassandra  
Нет единой точкой отказа как HBase (HMaster)  
Распределенная  
Поддерживает репликацию  
Маштабируема  
Устойчива к сбоям  
Консистентность в конечном счете  
Поддержка MapReduce Hadoop  
В отличие от HBase имеет SQL(CQL) подобный язык для доступа к данным  
Имеет программируемый read/write consistency

MapReduce недостатки  
Использует локальный диск для промежуточных опреаций  
Необходимость применять шаблон MapReduce

YARN Yet Another Resource Negotiator  
Для распределения ресурсов  
Может запускать различные задачи (не только MR)  
Нет жесткого разделения ресурсов как MapReduce v1 100 mapper 50 reducer

Компоненты(Demon) Yarn:  
- Resource Manager (Аналог JobTracker)  
  Запущен на отдельном сервере  
  Управляет глобальным распределением ресурсов  
  Разрешает конфликты между конкурирующими приложениями  
- Node Manager (Аналог TaskTracker)  
  Запущен на каждой ноде кластера  
  Взаимодействует с RM  
- Container  
  Создается по запросу RM  
  Захватывает определенное число ресурсов на ноде  
  В последствии в container запускается приложение  
- Application Master  
  Один для каждого приложения  
  Зависит от типа задач  
  Запускается в container  
  Общается с Container

Всегда было интересно, а как в монге с консистентностью? Если там eventual из коробки (иначе довольно сложно масштабироваться) то получается что инсталляция минимум 3 сервера должна быть, а лучше 5+ ?

Можно развернуть на одном, так-то. А вообще да, как везде: нечётное число реплик. Шардов можно иметь чётное число.

Да, но если ты пытаешься выжить в split brain, то consistency не получается, нужен арбитр который все видит. Кажется spanner строит на этом свою модель, они тупо говорят у нас network настолько надежный что split brain не будет и мы считаем что мы CAP с SLA больше пяти девяток

В обычном мире или CA или AP если серверов больше одного. А если 1 - нет сети, нет проблем с network partition :)

Монгу проще скейлить из коробки особенно если там schemaless-данные, это правда. Не надо тюнить индексы и прочий гемор. но за это приходится расплачиваться хромым acid

А чего кракена? Ну вот я пет-проджект предыдущий на Постгре сделал. Миллион инсертов в сутки оно ещё худо-бедно держало, а как пришла пора читать это всё - начала крякать и тормозить. Для агрегации вообще пришлось асинхронный скрипт делать с предрасчётами (чем с materialized views разбираться). В Монге я бы поднял ещё 2-3 шарда - и горя не знал.