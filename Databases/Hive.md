/--------Hive----------/

Cредства Упрощения работы с Mapreduce и это не БД, данные хранятся на hdfs  
Hive это не про хранение, а про анализ.  
Движок, позволяющий транслировать SQL-like запросы (HiveQL) в серии Map-Reduce job-ов  
Информация хранится в обычных файлах на HDFS  
Hive не совсем подходит для update/delte/insert т.к. это инструмент для анализа тех данных, которые уже есть. Hive неполноценная БД.  
Также не стоит думать об индексах в hive, нужно думать в разрезе партиционирования  
ACID

Партиционирование делается по параметру, по которому данные приходят к примеру 'по дате'

External table - совсем необязательно, чтобы файл в hive хранились во внутреннем хранилище. Hive может запускать mapreduce на внешние файлы

Иногда делают так: из внешней таблицы загружают файлы во внутреннюю таблицу и там анализируют данные

UDF - набор функций и методов, которые расширяют возможности hive т.к. на sql некоторые вещи нельзя реализовать

/-------shell commands Hive----------/

hive> show databases;  
hive> use mydatabase;  
hive> show tables;  
hive> create table rus_table (uid bigint, url string)  
      row format delimited      # формат строки ограничен(разграничен)  
      fields terminated by '\t' # поля завершаются tab  
      stored as textfile;        # сохранить как текстовый файл  
hive> describe rus_table;  
hive> show create table rus_table; # покажет конфигурацию таблицы и где ледат данные  
hive> drop table if exists rus_table;

/-------Excercises Hive with Hue----------/

Заходим в Hue -> Hive  
create database ruslan_konovalov