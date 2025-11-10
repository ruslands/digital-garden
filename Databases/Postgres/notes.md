## Connect to Yandex cluster

# get certificate

mkdir -p ~/.postgresql && wget "[https://storage.yandexcloud.net/cloud-certs/CA.pem](https://storage.yandexcloud.net/cloud-certs/CA.pem)" --output-document ~/.postgresql/CA.pem && chmod 0600 ~/.postgresql/CA.pem

# verify ssl

openssl s_client -connect rc1b-1vco6wbrmhvdndl0.mdb.yandexcloud.net:6432 -CAfile /Users/konovalov/.postgresql/CA.pem

# connect to cluster

psql "host=c-c9q96s5m7um2be35dfkr.rw.mdb.yandexcloud.net \

      port=6432 \

      sslmode=verify-full \

      sslrootcert=/Users/konovalov/.postgresql/RootCA.pem \

      dbname=staging \

      user=stabdteuwi \

      target_session_attrs=read-write"

# dump db

pg_dump --dbname=postgresql://stabdteuwi:VDRfetgfy65739ijeRTjdgfu876385Bjdgf@c-c9q96s5m7um2be35dfkr.rw.mdb.yandexcloud.net:6432/staging --schema=source --schema=core --schema=auth  | gzip > staging_20240228.sql.gz

######################

# restore

psql --dbname=postgresql://casas:aDai3aeshohBoh7O@dev-koti-db.co76ujrsni6t.eu-central-1.rds.amazonaws.com:5432/auth_staging -f auth_dev_20220923.sql

pgAdmin4 unlock password - KONOvalov2[dev-koti-db.kodit.io](http://dev-koti-db.kodit.io/)

psql --dbname=postgresql://auth:'xo(dJW83EBq1Pl'@koti-db.kodit.io:5432/auth_production -f auth_dev_20220923.sql

export PGDATA='/usr/local/var/postgres'

export PGHOST=localhost

psql mydb # the PostgreSQL interactive terminal program

psql -U postgres

psql -U mats -d postgres

psql -h 103.125.217.63 -p 5434 -U postgres

= psql terminal commands =

\?                                 # more psql internal commands

\du                               # list of users

\l                                  # список баз данных

\dt                                # list of tables

\c db                            # connect to db

\q                                  # quit from psql

\password postgres  #

\i basics.sql                 # reads in commands from the specified file

CREATE database new_db owner first_user;

DROP database mydb;

set role first_user;  
create table for_vk(column1 int, column2 text);  
drop table for_vk;  
create user first_user with password 'konovalov';  
grant all privileges on database rkonovalov to rkonovalov;

SELECT version();

SELECT current_date;

DROP TABLE tablename;

DROP USER username;

INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');

INSERT INTO weather (city, temp_lo, temp_hi, prcp, date) VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');

COPY weather FROM '/home/user/weather.txt';

= end =

psql --dbname=wideprint --username=wideprint --host=localhost # нужно создать UNIX пользователя mats_admin или использовать параметр --host

psql -U postgres -c 'SELECT now()' # подключение к бд.

['\dt' # структура бд | '\?' # спиосок команд | 'select * from answers' # вывести значения из бд]

Two dashes (“--”) introduce comments.

SQL is case insensitive about key words and identifiers

identifiers are double-quoted to preserve the case

[https://forums.docker.com/t/systemd-coredump-taking-ownership-of-tmp-db-directory-and-contents-in-rails-app/93609](https://forums.docker.com/t/systemd-coredump-taking-ownership-of-tmp-db-directory-and-contents-in-rails-app/93609)

2023-04-12 20:48:48.705 +03 [36040] LOG:  listening on IPv6 address "::1", port 5432

2023-04-12 20:48:48.705 +03 [36040] LOG:  listening on IPv4 address "127.0.0.1", port 5432

2023-04-12 20:48:48.706 +03 [36040] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"

2023-04-12 20:48:48.716 +03 [36041] LOG:  database system was shut down at 2023-04-12 19:08:19 +03

2023-04-12 20:48:48.722 +03 [36040] LOG:  database system is ready to accept connections

ps auxwww |grep postgres

===mats===

CREATE ROLE auth_mcs WITH LOGIN PASSWORD '12345';

ALTER ROLE auth_mcs CREATEDB;

CREATE DATABASE auth_db;

GRANT ALL PRIVILEGES ON DATABASE auth_db TO auth_mcs;

Настройка доступа к базе данных осуществляется через файл pg_hba.conf

vi /etc/postgresql/9.5/main/postgresql.conf

   listen_addresses = '*'

   service postgresql restart

vi /etc/postgresql/9.5/main/pg_hba.conf # добавить ip контайнера or 0.0.0.0/0

   host    mats            mats_admin      0.0.0.0/0           md5

   service postgresql restart

chmod 700 pg_hba.conf

chmod 700 postgresql.conf

chmod 700 .pgpass

# from docker container

chown postgres:postgres pg_hba.conf

chown postgres:postgres postgresql.conf

chown postgres:postgres .pgpass

# conf file macos

 vi /usr/local/var/postgres/pg_hba.conf

egrep 'listen|port' /usr/local/var/postgres/postgresql.conf

pg_ctl -D /usr/local/var/postgres start

pg_ctl -D /usr/local/var/postgres stop

pg_ctl -D /usr/local/var/postgres status

pg_ctl -D /usr/local/bin/postgres status

systemctl status postgresql

service postgresql restart

brew services restart postgresql

netstat | grep 5432

lsof -i :5432

kill -s 9 87385

launchctl list | grep post

## No PostgreSQL clusters exist; see "man pg_createcluster"

pg_createcluster 9.5 main

/etc/init.d/postgresql start

FTS

[https://eax.me/postgresql-full-text-search/](https://eax.me/postgresql-full-text-search/)

[https://eax.me/pg-trgm/](https://eax.me/pg-trgm/)

[https://www.postgresql.org/docs/9.6/textsearch-tables.html](https://www.postgresql.org/docs/9.6/textsearch-tables.html)

    stmt = text("""

        SELECT * FROM journal

        WHERE extract('day' FROM (current_date - last_update)) >= update_frequency LIMIT :limit;

        """)

    results = session.query(Journal).from_statement(stmt).params(limit=limit).all()

Drop all tables

DO $$  
  DECLARE  
    r RECORD;  
BEGIN  
  FOR r IN  
    (  
      SELECT table_name  
      FROM information_schema.tables  
      WHERE table_schema=current_schema()  
    )  
  LOOP  
     EXECUTE 'DROP TABLE IF EXISTS ' || quote_ident(r.table_name) || ' CASCADE';  
  END LOOP;  
END $$ ;