/--------HBase-------/

РСУБД превосходят почти по всем параметрам если данных мало, очень плохо масштабируются

HDFS и MapReduce — мощные инструменты для выполнения пакетных операций с большими наборами данных, но они не предоставляют средств для эффективного чтения или сохранения отдельных записей, HBase помогает заполнить этот пробел.  

Распределенная столбцово-ориентированная база данных, построенная на основе HDFS.  
Это приложение Hadoop, используемое в ситуациях, когда вам необходимо организовать произвольный доступ для чтения/записи к очень большим наборам данных в реальном времени.

Использование HBase в качестве источника и(или) приемника данных в заданиях MapReduce

Бинарный поиск - делит массив пополам и сравнением ищет число.  
Использовать не более 3 Column Family  
RowKey - генерируем сами (url, id user и т.д.)

/--------Упражнение HBase--------/

Войти в Shell HBase  
        # hbase shell  
Вывести список всех таблиц базы данных  
        # list  
Создать таблицу с единственной column family 'fam1'  
        # create 'tralala.Ruslan', 'fam1'  
        # create 'tralala.Ruslan', {NAME=>'fam1'}  
Вывести список таблиц, название которых начинается с T  
        # list 'T.*'  
Показать структуру таблицы  
        # describe 'tralala.Ruslan'  
Установить в column 'fam1' сжатие gz  
        # alter 'tralala.Ruslan', {NAME=>'fam1', COMPRESSION=>'gz'}  
Добавить в таблицу column family 'a'  
        # alter 'tralala.Ruslan', {NAME=>'a'}  
Отключить таблицу  
        # disable 'tralala.Ruslan'  
Удалить таблицу  
        # drop 'tralala.Ruslan'  
Создать таблицу 'test', c column family 'a' и 'b'  
        # create 'test', {NAME=>'a'}, {NAME=>'b'}  
Добавить запись с ключом 'key1' и значениями 'v1'  в колонкее 'a:a' и 'v2' в 'b:a'  
        # put 'test', 'key1', 'a:a', 'v1'  
        # put 'test', 'key1', 'b:a', 'v2'  
Просканировать все значения  
        # scan 'test'  
Просканировать все значения в CF 'a'  
        # scan 'test', {COLUMNS=>['a']}  
Удалить значение 'v1'  
        # delete 'test', 'key1', 'a:a'  
Перезаписать значение v2 на v5  
        # put 'test', 'key1', 'b:a', 'v500'  
Выводит команды по категориям.  
        # help  
Справку по конкретной команде с примерами использования.  
        # help your command

Создать таблицу users с CF user_profile  
        # create 'users', {NAME => 'user_profile', VERSIONS => 5},

Положить в таблицу users по rowkey id1  в columnname - user_profile в column name значение alexander  
        # put 'users', 'id1', 'user_profile:name', 'alexander'  
Тоже самое только column name - second_name  
        # put 'users', 'id1', 'user_profile:second_name', 'alexander'

get 'users', 'id1'

put 'users', 'id1', 'user_profile:second_name', 'petrov'

get 'users', 'id1'

get 'users', 'id1', {COLUMN => 'user_profile:second_name', VERSIONS => 5}

put 'users', 'id2', 'user_profile:name', 'vasiliy'

put 'users', 'id2', 'user_profile:second_name', 'ivanov'

scan 'users', {COLUMN => 'user_profile:second_name', VERSIONS => 5}

delete 'users', 'id1', 'user_profile:second_name'

get 'users', 'id1'

/------------Python HappyBase-----------/

#reducer для lab  
import happybase  
connection = happybase.Connection('sometable')  
print(connection.tables()) # выведет доступные таблицы  
table = connection.table('mytable') # создаем переменную ссылку на подключение таблицы

row = table.row(b'row-key')  
print(row[b'cf1:col1']) # напечатает значения в семейства cf1 столбец col1

/----happybase task-----/

Подключить библиотеку и установить соединение с hbase по адресу node2.newprolab.com  
Создать таблицу со своим названием с одним column family fam1 и максимальным числом версий  
Записать в нее 100 значений с ключом от 1 до 100, значания - квадраты этих чисел в строковом виде  
Просканировать эти значения, удаляя все четные ключи больше

import happybase  
connection = happybase.Connection('node2.newprolab.com')  
connection.create_table('tralala.ruslan', {'fam1': dict(max_versions=10)})  
table = happybase.Table('tralala.ruslan', connection)

for i in range(1, 101):  
        table.put(str(i), {'fam1': str(i*i)}

for k, d in table.scan():  
        if int(k) > 50 and int(k) % 2 == 0:  
                table.delete(k)