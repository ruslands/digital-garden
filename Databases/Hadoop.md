/--------Hadoop--------/  
Так что правило такое: скрипты — локально на кластере, инпут-аутпут — на hdfs, в отдельных директориях, директорию для output не создаем!  

Hadoop является проектом верхнего уровня организации Apache Software Foundation, поэтому основным дистрибутивом и центральным репозиторием для всех наработок считается именно Apache Hadoop. Однако этот же дистрибутив является основной причиной большинства сожжённых нервных клеток при знакомстве с данным инструментом: по умолчанию установка слонёнка на кластер требует предварительной настройки машин, ручной установки пакетов, правки множества файлов конфигурации и кучи других телодвижений. При этом документация чаще всего неполна или просто устарела. Поэтому на практике чаще всего используются дистрибутивы от одной из трёх компаний:

Hortonworks Ambari  
Cloudera  
MapR

Когда мы говорим про Hadoop, то в первую очередь имеем в виду его файловую систему — HDFS (Hadoop Distributed File System).  
Самый простой способ думать про HDFS — это представить обычную файловую систему, только больше. Обычная ФС, по большому счёту, состоит из таблицы файловых дескрипторов и области данных. В HDFS вместо таблицы используется специальный сервер — сервер имён (NameNode), а данные разбросаны по серверам данных (DataNode).  

сервер имён раскрывает для всех желающих расположение блоков данных на машинах

HDFS имеет классическую unix-овскую древовидную структуру директорий, пользователей с триплетом прав, и даже схожий набор консольных комманд:  
# просмотреть корневую директорию: локально и на HDFS  
ls /  
hadoop fs -ls /

# оценить размер директории  
du -sh mydata  
hadoop fs -du -s -h mydata

# вывести на экран содержимое всех файлов в директории  
cat mydata/*  
hadoop fs -cat mydata/*

/---------Hadoop FS Shell Commnads---------/  
*See also User and Administration Commands

hadoop fs -rm -r -skipTrash /user/root/rus_temp_freq_words # Skip Trash

hadoop fs -copyFromLocal /home/training/purchases.txt hadoop/ # Add the purchases.txt file from the local directory to the hadoop directory you created in HDFS

hadoop fs -copyToLocal /user/ruslan.konovalov/result /data/home/ruslan.konovalov # скачать файл с hdfs к себе на сервер

hadoop fs -get hadoop/sample.txt /home/training/ # ‘-get’ command can be used alternaively to ‘-copyToLocal’ command  

hadoop fs -cp /labs/facetz_2015_02_17 /user/ruslan.konovalov # Копирование файла между директориями на HDFS

hadoop fs -cat /user/ruslan.konovalov/result/* # покажет содержимое файла  
   
hadoop fs -ls hadoop #  List the hadoop directory  
   
hadoop fs -ls / #  List the contents of the root directory in HDFS  
   
hadoop fs -rm hadoop/retail/customers # Delete a file ‘customers’ from the “retail” directory.  
    
hadoop fs # список команд  
   
hadoop fs -expunge # To empty the trash  
    
hadoop fs -mv hadoop apache_hadoop # Move a directory from one location to othe  
    
hadoop fs -setrep -w 2 apache_hadoop/sample.txt # change replication factor of a file,  Default replication factor to a file is 3.  
   
hadoop fs -mkdir /user/training/hadoop # create directory  
   
hadoop fs -du -s -h hadoop/retail # See how much space this directory occupies in HDFS.  
   
hadoop fs -df hdfs:/ # Report the amount of space used and available on currently mounted filesystem  

hadoop fs -count hdfs:/ # Count the number of directories,files and bytes under the paths that match the specified file pattern

hadoop fs -put data/sample.txt /user/training/hadoop # Add a sample text file from the local directory named “data” to the new directory you created in HDFS during the previous step.  

hadoop fs -put data/retail /user/training/hadoop # Add the entire local directory called “retail” to the /user/training directory in HDFS.

/--------Job------/

hadoop jar /usr/hdp/2.5.3.0-37/hadoop-mapreduce/hadoop-streaming.jar -D mapred.reduce.tasks=1 -files /home/ubuntu/data/mapper.py,/home/ubuntu/data/reducer.py -input '/users/data' -output '/users/adverts' -mapper 'python mapper.py' -reducer 'python reducer.py'

-files перечисляем файлы, которые необходимо добавить в distributed cache. Параметр –files должен идти перед другими параметрами.  
если код пишем на python3, то -mapper 'python mapper.py' -reducer 'python reducer.py'  

Map only job

hadoop jar /usr/hdp/2.5.3.0-37/hadoop-mapreduce/hadoop-streaming.jar -D mapred.reduce.tasks=0 -file /data/home/ruslan.konovalov/code/mapper.py -input '/labs/facetz_2015_02_17/*' -output '/user/ruslan.konovalov/result' -mapper 'python mapper.py'

Job with Cat|Sort

hadoop jar /usr/hdp/2.5.3.0-37/hadoop-mapreduce/hadoop-streaming.jar -D mapred.reduce.tasks=1 -input '/user/ruslan.konovalov/result/part-00000' -output '/user/ruslan.konovalov/top350' -mapper "cat" -reducer "sort -nrk2"

hadoop jar /usr/hdp/2.5.3.0-37/hadoop-mapreduce/hadoop-streaming.jar -D mapred.reduce.tasks=1 -input /users/numbers -output /users/numbers/result -mapper "cat" -reducer "uniq -c"  
Мы здесь используем простые cli-утилиты cat и uniq, считаем число уникальных строк

HDFS

доступ к данным на чтение в HDFS через Последовательное чтение всего файла с данными

/**Pros**/  
- Fault Tolerance  
- Don't need expensive servers  
- write once / read many times  
- Последовательное чтение

/**Cons**/  
- Реализована Low-Latency - Высокая пропускная способность вместо быстрого доступа к данным, H-Base решает эту проблему. т.е. быстро получать данные от какого-либо ресурса и хранить в их HDFS это не получится  
- Не подходит для хранения большого количества маленьких файлов. Лучше 1 файл - 1000мб, чем 100 по 10.  
- Не подходит для многопоточной записи  
- Записывает данные в конец, поменять файл в середине нельзя.

Демоны HDFS - процессы, которые постоянно запущены на сервере  
- Secondary NameNode  
- Namenoda  
- Datanoda

Namenoda отвечает за:  
- Хранит информацию о файловой системе fsimage  
- Определяет как реплицировать блоки  
- Файловое пространство  
- Мета-информацию  
- Хранит расположение блоков файлов  
- Не хранит данные  
! - Запускается на выделенной машине, если машину теряем, то теряем всю информацию в HDFS

Datanoda:  
- Хранит информацию в блоках (64 или 128 мб)  
- Хранит и отдает блоки данных  
- Отправляет ответы о состояние Namenoda  
- Запускается на каждой машине кластера

Secondary NameNode:  
Необязательный демон, который может быть не запущен не является бэкапом. Периодически обновляет файл fsimage.  
Обычная namenoda сохраняет все данные об изменениях в файловой системе в лог файл и только после перезагрузки HDFS  
накатывает изменения в fsimage и это может занять много времени.  
Когда происходит перезагрузка Namenoda, Secondary Namenoda включается в работу пока Namenoda перезагружается.

Репликация:  
Один блок хранится на нескольких datanoda  
Фактор репликации равен 3;

Чем больше размер блока HDFS, тем больше файлов в HDFS можно хранить  
Чем больше размер блока datanode, тем меньше памяти нужно для namenoda, чтобы хранить информацию о блоках.  
Если сделать миллион маленьких блоков, то может не хватить памяти в namenoda и она откажет

Способы доступа:  
- Direct Access (с помощью нативного клиента)  
- Proxy Server (Rest, Thrift)

За что отвечает фреймворк MapReduce Hadoop:  
Синхронизация этапов Map и Reduce  
Запуск mapper и reducer  
Копирование кода к данным  
Обработка ошибок и отказов

Hadoop Streaming помогает запускать MR jobs написанные на другом языке, отличном от Java.