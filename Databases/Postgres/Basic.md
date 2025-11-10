При чтении филда больше 8 кб, происходит чтение с диска

log_statement will log only top-level statements, that is statements sent by the client. Nested statements are not logged.

user can define their own data types

There is a limit on how many columns a table can contain. Depending on the column types, it is between 250 and 1600

Backend

64-bit large objects

Large objects are now 64-bit and can now be up to 4TB where before it was up to 2GB.

[Advisory Locks](https://www.postgresql.org/docs/current/explicit-locking.html#ADVISORY-LOCKS)

PostgreSQL provides a means for creating locks that have application-defined meanings. These are called advisory locks, because the system does not enforce their use — it is up to the application to use them correctly. Advisory locks can be useful for locking strategies that are an awkward fit for the MVCC model. For example, a common use of advisory locks is to emulate pessimistic locking strategies typical of so-called “flat file” data management systems. While a flag stored in a table could be used for the same purpose, advisory locks are faster, avoid table bloat, and are automatically cleaned up by the server at the end of the session.

There are two ways to acquire an advisory lock in PostgreSQL: at session level or at transaction level.

Like all locks in PostgreSQL, a complete list of advisory locks currently held by any session can be found in the [pg_locks](https://www.postgresql.org/docs/current/view-pg-locks.html) system view.

Value expressions: allows the calculation of values from primitive parts using arithmetic, logical, set, and other operations. [https://www.postgresql.org/docs/current/sql-expressions.html](https://www.postgresql.org/docs/current/sql-expressions.html)

Calling functions:  functions that have named parameters can be called using either positional or named notation

[https://www.postgresql.org/docs/current/sql-syntax-calling-funcs.html](https://www.postgresql.org/docs/current/sql-syntax-calling-funcs.html)


[https://www.postgresql.org/docs/current](https://www.postgresql.org/docs/current)

[https://wiki.postgresql.org/wiki/Main_Page](https://wiki.postgresql.org/wiki/Main_Page)

[https://wiki.postgresql.org/wiki/Frequently_Asked_Questions](https://wiki.postgresql.org/wiki/Frequently_Asked_Questions)

![[postgres-1.jpg]]

relational database management system (RDBMS): That means it is a system for managing data stored in relations. Relation is essentially a mathematical term for table.

Each table is a named collection of rows. Each row of a given table has the same set of named columns, and each column is of a specific data type. Whereas columns have a fixed order in each row, it is important to remember that SQL does not guarantee the order of the rows within the table in any way (although they can be explicitly sorted for display).

Tables are grouped into databases, and a collection of databases managed by a single PostgreSQL server instance constitutes a database cluster. To retrieve data from a table, the table is queried

postgres: the database server program

System catalogs: are the place where a relational database management system stores schema metadata, such as information about tables and columns, and internal bookkeeping information. PostgreSQL's system catalogs are regular tables. You can drop and recreate the tables, add columns, insert and update values, and severely mess up your system that way. 

transactional integrity:

referential integrity: no one can insert rows in the table that do not have a matching entry in the other table

MVCC (multiversion concurrency control): is one of the main techniques Postgres uses to implement transactions. MVCC lets Postgres run many queries that touch the same rows simultaneously, while keeping those queries isolated from each other.

=== Transactions ===

- is the propagation of one or more changes to the database. Groups of updates

- позволяют выполнять операции быстрее и без конфликтов

- transactions should be short

- if something goes wrong partway through the operation, none of the steps executed so far will take effect

- A transaction is said to be atomic: from the point of view of other transactions, it either happens completely or not at all.

- We also want a guarantee that once a transaction is completed and acknowledged by the database system, it has indeed been permanently recorded and won't be lost even if a crash ensues shortly thereafter. A transactional database guarantees that all the updates made by a transaction are logged in permanent storage (i.e., on disk) before the transaction is reported complete.

- when multiple transactions are running concurrently, each one should not be able to see the incomplete changes made by others. 

- updates made so far by an open transaction are invisible to other transactions until the transaction completes

Уровни изоляции транзакций:

Read uncommited == Read commited; Read uncommited in PostgreSQL

Read committed (Чтение зафиксированных данных): default settings. A statement can only see rows committed before it began.

допустимы следующие особые условия чтения данных:

Неповторяемое чтение — транзакция повторно читает те же данные, что и раньше, и обнаруживает, что они были изменены другой транзакцией (которая завершилась после первого чтения).

Фантомное чтение — транзакция повторно выполняет запрос, возвращающий набор строк для некоторого условия, и обнаруживает, что набор строк, удовлетворяющих условию, изменился из-за транзакции, завершившейся за это время.

Repeatable read: new rows can be inserted into the dataset (phantom rows). So you can read the same data in the same transaction without fear of someone changing it - even though it's a rare need for doing it

Serializable: данный уровень изоляции самый строгий, и не имеет феноменов чтения данных. prevents both non-repeatable read and phantom rows (so you can't even INSERT data). That means you can READ and WRITE (SELECT, UPDATE) rows that are not included with serializable transaction, but you can't DELETE OR INSERT rows on TABLE level.

transaction is set up by surrounding the SQL commands of the transaction with BEGIN and COMMIT commands

BEGIN;  
UPDATE accounts SET balance = balance - 100.00  
    WHERE name = 'Alice';  
COMMIT;

PostgreSQL actually treats every SQL statement as being executed within a transaction. If you do not issue a BEGIN command, then each individual statement has an implicit BEGIN and (if successful) COMMIT wrapped around it. A group of statements surrounded by BEGIN and COMMIT is sometimes called a transaction block

SAVEPOINT allow you to selectively discard parts of the transaction, while committing the rest. 

================

Уровни блокировки таблиц:

Имеются различные типы блокировок в PostgreSQL:

1. Блокировка уровня строки
2. Блокировка уровня таблицы
3. Совещательная блокировка

AccessShare - которая защищает от изменений структуры таблицы. Самая слабая блокировка

RowShare -

RowExclusive -

ShareUpdateExclusive -

Share -

ShareRowExclusive -

Exclusive -

AccessExclusive - блокировка самого высокого уровня

![[postgres-2.png]]

приведем такой пример. Что произойдет, если выполнить команду CREATE INDEX?

Находим в документации, что эта команда устанавливает блокировку в режиме Share. По матрице определяем, что команда совместима сама с собой (то есть можно одновременно создавать несколько индексов) и с читающими командами. Таким образом, команды SELECT продолжат работу, а вот команды UPDATE, DELETE, INSERT будут заблокированы.

n+1 проблема orm

Одна из основных проблем разработчиков, когда они создают приложение с ORM — это N+1 запрос в их приложениях. ... Эта проблема обычно возникает, когда мы получаем список данных из базы данных без использования ленивой или жадной загрузки (lazy load, eager load)

Проблема N + 1 возникает, когда фреймворк доступа к данным выполняет N дополнительных SQL-запросов для получения тех же данных, которые можно получить при выполнении одного SQL-запроса. Чем больше значение N, тем больше запросов будет выполнено и тем больше влияние на производительность. И хотя [лог медленных запросов](https://vladmihalcea.com/hibernate-slow-query-log/) может вам помочь найти медленные запросы, но проблему N + 1 он не обнаружит, так как каждый отдельный дополнительный запрос выполняется достаточно быстро. Проблема заключается в выполнении множества дополнительных запросов, которые в сумме выполняются уже существенное время, влияющее на быстродействие.  
 

Создание индекса

Те, кто работает с PostgreSQL, наверное, знают, что такая команда блокирует всю таблицу. Но еще с очень старой версии 8.2 существует ключевое слово [CONCURRENTLY](https://www.postgresql.org/docs/current/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY), которое позволяет создавать индекс в неблокирующем режиме.У этой команды есть один нюанс. Она может завершиться ошибкой – например, при создании уникального индекса в таблице, содержащей дублирующиеся значения. Индекс при этом будет создан, но он будет помечен как невалидный и не будет использоваться в запросах. Статус индекса можно проверить при помощи следующего запроса: 

REINDEX

Стоит отдельно сказать про команду [REINDEX](https://www.postgresql.org/docs/current/sql-reindex.html), которая как раз предназначена для пересоздания индекса. До 12й версии она работает только в блокирующем режиме, что не дает возможности ее использовать. В 12й версии PostgreSQL появилась поддержка CONCURRENTLY, и теперь и ей можно пользоваться.

Добавление столбца  
такая миграция весьма дешевая и безопасная. Сама команда, хоть и захватывает блокировку самого высокого уровня (AccessExclusive), выполняется очень быстро, поскольку «под капотом» происходит лишь добавление метаинформации о новом столбце без перезаписи данных самой таблицы. В большинстве случаев это происходит незаметно.  
На время выполнения транзакции захватывается самая слабая блокировка AccessShare, которая защищает от изменений структуры таблицы.

Добавление столбца со значением по умолчанию  
Если эта команда выполняется в PostgreSQL старой версии (ниже 11), то это приводит к перезаписи всех строк в таблице. Очевидно, что если таблица большая, то это может занять много времени. А поскольку на время выполнения захватывается строгая блокировка ([AccessExclusive](https://www.postgresql.org/docs/current/explicit-locking.html#LOCKING-TABLES)), то все запросы к таблице также блокируются. 

Удаление столбца  
Здесь логика такая же, как и при добавлении столбца: данные таблицы не модифицируются, происходит только изменение метаинформации. В данном случае столбец помечается как удаленный и недоступный при запросах. Это объясняет тот факт, что при удалении столбца в PostgreSQL физически место не освобождается (если не выполнять VACUUM FULL), то есть данные старых записей по-прежнему остаются в таблице, но недоступны при обращении. Освобождение происходит постепенно при перезаписи строк в таблице.

Ограничения

Теперь пройдемся по ограничениям: NOT NULL, внешние, уникальные и первичные ключи.

Создание ограничения NOT NULL

Создание ограничения таким образом приведет к сканированию всей таблицы – все строки будут проверены на условие not null, и если таблица большая, то это может занять длительное время. Строгая блокировка, захватываемая этой командой, заблокирует все параллельные запросы до ее завершения. 

Создание внешнего ключа

При добавлении внешнего ключа все записи дочерней таблицы проверяются на наличие значения в родительской. Если таблица большая, то это сканирование будет долгим, и блокировка, которая удерживается на обеих таблицах, также будет долгой.

Создание ограничения уникальности

Как и в случае с ранее рассмотренными ограничениями, команда захватывает строгую блокировку, под которой проводит проверку всех строк в таблице на соответствие ограничению – в данном случае уникальности. 

Window functions: performs a calculation across a set of table rows that are somehow related to the current row. The OVER clause determines exactly how the rows of the query are split up for processing by the window function. The PARTITION BY clause within OVER divides the rows into groups, or partitions, that share the same values of the PARTITION BY expression(s). For each row, the window function is computed across the rows that fall into the same partition as the current row.

используется ключевой слово OVER это не то же самое, что GROUP BY. Они не уменьшают количество строк, а возвращают столько же значений, сколько получили на вход. Во-вторых, в отличие от GROUP BY, OVER может обращаться к другим строкам. И в-третьих, они могут считать скользящие средние и кумулятивные суммы. Оконные функции не изменяют выборку, а только добавляют некоторую дополнительную информацию о ней. Для простоты понимания можно считать, что SQL сначала выполняет весь запрос (кроме сортировки и limit), а уже потом считает значения окна.

[https://habr.com/ru/company/glowbyte/blog/515940/](https://habr.com/ru/company/glowbyte/blog/515940/)

Модели DWH

Data Vault

Anchor Model

Data Lakes vs Data Warehouse

Data lakes and data warehouses are both widely used for storing big data, but they are not interchangeable terms. A data lake is a vast pool of raw data, the purpose for which is not yet defined. A data warehouse is a repository for structured, filtered data that has already been processed for a specific purpose.

Indexes: Структура данных(вспомогательная таблица), которые предназначены для ускорения выполнения запросов к БД.

Индексы требуют время на обновление и хранение

Indexes types (Access Methods):

Heap -

B-Tree - Этот тип индекса используется по умолчанию и покрывает очень широкий круг задач.

С помощью B-дерева можно проиндексировать любые данные, которые могут быть отсортированы, т. е. для которых применимы операции сравнения больше/меньше/равно. Сюда можно отнести числа, строки, даты и время, логический тип и любые данные, которые можно ими закодировать.

индекс хранят в виде сбалансированного сильно ветвящегося дерева, называемого B-деревом (B-tree)

Работают с операторами: < ; <= ; = ; >= ; >

Индекс b-tree жестко привязан к семантике сравнения: поддержка операторов «больше», «меньше», «равно» — это все, на что он способен (зато способен очень хорошо!). Но в современных базах хранятся и такие типы данных, для которых эти операторы просто не имеют смысла: геоданные, текстовые документы, картинки…

Hash

GiST (generalized search tree) - index can be used vary depending on the indexing strategy (the operator class)

SP-GiST - offer an infrastructure that supports various kinds of searches.

GIN (Generalized Inverted Index) - indexes are inverted indexes which can handle values that contain more than one key, arrays for example. they can index what a normal B-tree cannot, such as JSONB data types and full text search.

BRIN (Block Range INdex)

Bitmap

Dense

Sparse

Reverse

Primary

Secondary

Hash

Postgres работает быстрее, можно хранить JSON и делать join, но сложно сделать несколько реплик. Киллер фича mongo, что ее можно реплицировать и она не сломается.

Mongo работает стабильнее, но медленее

Index can have own constraints - unique etc

CTE: Common Table Expression

Used as a temporary result set that the user can reference within another SQL statement. CTEs are temporary in the sense that they only exist during the execution of the query. CTEs are typically used to simplify complex joins and subqueries in PostgreSQL.

It is defined by using WITH statement. CTE improves readability and ease in maintenance of complex queries and sub-queries.

WITH my_cte AS (

  SELECT a, b, c

  FROM T1

)

SELECT a, c

FROM my_cte

WHERE ....

Table Variable

Local Temp Table

Foreign table: A foreign table can be used in queries just like a normal table, but a foreign table has no storage in the PostgreSQL server. Whenever it is used, PostgreSQL asks the foreign data wrapper to fetch data from the external source, or transmit data to the external source in the case of update commands.

Materialized View:  is more or less a view that is cached (or materialized) to disk

Sequence:

postgres_fdw: module provides the foreign-data wrapper postgres_fdw, which can be used to access data stored in external PostgreSQL servers.

table bloat: Due to how PostgreSQL handles transactions and concurrency, MVCC - Multi-Version Concurrency Control, you can get bloat. In PostgreSQL, when you do an UPDATE or DELETE, the row isn't actually physically deleted. For a DELETE, it simply marks the row as unavailable for future transactions, and for UPDATE, under the hood it's a combined INSERT then DELETE, where the previous version of the row is marked unavailable.

While the data is marked unavailable, it is still there, and the space cannot be used. To then mark the space as available for use by the database, a vacuum process needs to come along behind the operations, and mark that space available for the database to use. It isn't returned to the operating system, however. That only happens when there are no active rows in an entire page, which can be uncommon in some workloads. This can be a good thing for some workloads, because you can just simply update the space on the individual pages inside of the data files, without needing to add additional data files.

Problems come about with bloat when there are excessively large numbers of dead tuples versus live tuples. Walking along and checking all of the visibility flags takes time, and having more data files for a relation results in additional unnecessary IO load. Bloat is especially noticeable on indexes, which can also have many dead tuples, sometimes many more than the table. Bloat can slow index lookups and scans, which will show up in slowly increasing query times and changing query plans.

You can restore space by using pg_reorg, pg_repack, CLUSTER, VACUUM FULL. This will go through and reorganize the files, moving tuples and reorganizing to make sure that there are no dead tuples, which will eliminate the bloat. 

Bloat can also be efficiently managed by adjusting VACUUM settings per table, which marks dead tuple space available for reuse by subsequent queries.

You can use queries on the PostgreSQL Wiki related to [Show Database Bloat](https://wiki.postgresql.org/wiki/Show_database_bloat) and [Index Bloat](https://wiki.postgresql.org/wiki/Index_Maintenance#Index_Bloat) to determine how much bloat you have, and from there, do a bit of performance analysis to see if you have problems with the amount of bloat you have on your tables.

A materialized view and a table are both database objects used to store data, but they have some key differences in terms of their functionality and usage. Here are the main differences between a materialized view and a table:

1. Data Storage:

- Table: A table is a basic database object that stores data as a collection of rows and columns. It is typically used to store permanent and persistent data.
- Materialized View: A materialized view is a derived table that stores the results of a precomputed query. It is created by executing a query against one or more tables and storing the result set in a separate object. The data in a materialized view is typically a subset or aggregation of data from one or more tables.

1. Data Persistence:

- Table: The data in a table is persistent and remains intact until explicitly modified or deleted.
- Materialized View: The data in a materialized view is persistent as long as it is explicitly refreshed or updated. It represents a snapshot of the underlying data at the time of creation, and it can be refreshed periodically or on-demand to reflect changes in the source tables.

1. Query Performance:

- Table: Tables are optimized for efficient data storage and retrieval. Indexes and other optimization techniques can be applied to improve query performance.
- Materialized View: Materialized views are precomputed and optimized for specific queries. They can improve query performance by providing faster access to data. Since the data is precomputed and stored, queries against materialized views can avoid expensive joins and aggregations, resulting in faster response times.

1. Data Consistency:

- Table: The data in a table is always up-to-date, reflecting the latest changes made to the data.
- Materialized View: The data in a materialized view may not be up-to-date with the latest changes in the source tables. To ensure data consistency, materialized views need to be refreshed or updated periodically or manually.

1. Usage and Purpose:

- Table: Tables are used for storing and managing data in a database. They are commonly used for transactional operations, data persistence, and as a primary data source.
- Materialized View: Materialized views are used for improving query performance, especially for complex and time-consuming queries. They are often used in reporting and data warehousing scenarios where aggregated or summarized data is required.

In summary, a table is a basic database object used for permanent data storage, while a materialized view is a derived object that stores precomputed query results, optimized for specific queries. Materialized views can improve query performance but require periodic or manual refreshing to ensure data consistency.

FQDN -