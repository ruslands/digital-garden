mongoShell

use core;

db.user.deleteMany({}) # clean database

db.journal.updateMany({}, {"$set" : {"status": true}}, upsert=false, multi=true)

1. Start mongo without --auth
2. use admin  
    db.createUser(  
      {  
        user: "admin",  
        pwd: "SUNENNERUURU848245ufnjrf",  
        roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]  
      }  
    )
3. db.updateUser("admin",{roles : ["userAdminAnyDatabase","userAdmin","readWrite","dbAdmin","clusterAdmin","readWriteAnyDatabase","dbAdminAnyDatabase"]});
4. mongodb://admin:SUNENNERUURU848245ufnjrf@46.243.183.115:3033/admin ## create databases from compas
5. From container run python3 schema-validator.py

CLI

## Create new database

use <new-database-name>

## Drop database

use mydb;  
db.dropDatabase()

docker exec allpapers-mongo sh -c 'exec mongodump --db core  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --gzip --archive' > dump_`date "+%Y-%m-%d"`.gz

docker exec allpapers-mongo sh -c 'mongorestore --db core --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --gzip --archive=/data/dump/dump_2022-08-07.gz'

docker exec allpapers-mongo sh -c 'exec mongodump --db core  --collection journal --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --gzip --archive' > dump_`date "+%Y-%m-%d"`.gz

mongodump --db allpapers-core --out - | gzip > dbdump.gz

mongodump -d allpapers-core -o backup/dbdump

mongodump --host allpapers-mongo --port 27017 --db allpapers-core --collection journal --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --out ./backup

# Сделать Бэкап

mongodump --host allpapers-mongo --port 27017 --db allpapers-core --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --out ./backup

# Импорт бэкапов

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection user backup/allpapers-core/user.bson

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection journal backup/allpapers-core/journal.bson

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection paper backup/allpapers-core/paper.bson

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection tag backup/allpapers-core/tag.bson

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection publisher backup/allpapers-core/publisher.bson

mongorestore --host allpapers-mongo --port 27017  --authenticationDatabase admin --username allpapers --password 537nfrufhUYGUED --db allpapers-core --collection definitions backup/allpapers-core/definitions.bson

[https://www.shellhacks.com/mongodb-create-user-database-admin-root/](https://www.shellhacks.com/mongodb-create-user-database-admin-root/)

Flow to set authentication : 

1. Start MongoDB without access control.

vpakete-mongo:

image: mongo:latest

container_name: vpakete-mongo

hostname: vpakete-mongo

restart: always

ports:

- "3033:27017"

# expose:

# - 27017

# environment:

# - MONGO_INITDB_ROOT_USERNAME=admin

# - MONGO_INITDB_ROOT_PASSWORD=XEYNHHSEOFxsJLysNHASlnR2WGs6iYW4

volumes:

- "./vpakete-mongo/data:/data/db"

- "./vpakete-mongo/setup:/setup"

- "./vpakete-mongo/import:/import"

command: mongod # --auth

networks:

- default

1. Create the user administrator.

use admin  
    db.createUser(  
      {  
        user: "9CeKMhEKeLBJyOx3UUWbuBlNCBaE6LD6",  
        pwd: "XEYNHHSEOFxsJLysNHASlnR2WGs6iYW4",  
        roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]  
      }  
    )

use admin

db.createUser(  
  {  
    user: "user",  
    pwd: "537nfrufhUYGUED",

    roles: [ { role: "readWrite", db: "vpakete-user" } ]  
  }  
)

db.createUser(  
  {  
    user: "picker",  
    pwd: "537nfrufhUYGUED",

    roles: [ { role: "readWrite", db: "vpakete-picker-db" } ]  
  }  
)

db.createUser(  
  {  
    user: "courier",  
    pwd: "537nfrufhUYGUED",

    roles: [ { role: "readWrite", db: "vpakete-courier-db" } ]  
  }  
)

db.createUser(  
  {  
    user: "crm",  
    pwd: "537nfrufhUYGUED",

    roles: [ { role: "readWrite", db: "vpakete-crm-db" } ]  
  }  
)

1. Re-start the MongoDB instance with access control.

image: mongo:latest

container_name: vpakete-mongo

hostname: vpakete-mongo

restart: always

ports:

- "3033:27017"

# expose:

# - 27017

environment:

- MONGO_INITDB_ROOT_USERNAME=admin

- MONGO_INITDB_ROOT_PASSWORD=XEYNHHSEOFxsJLysNHASlnR2WGs6iYW4

volumes:

- "./vpakete-mongo/data:/data/db"

- "./vpakete-mongo/setup:/setup"

- "./vpakete-mongo/import:/import"

command: mongod --auth

networks:

- default

1. Connect to DB

mongodb://9CeKMhEKeLBJyOx6:XEYNHHSEOFxsJLysNGs6iYW4@185.87.194.174:3033/admin

mongodb://9CeKMhEKeLBJyOx3UUWbuBlNCBaE6LD6:XEYNHHSEOFxsJLysNHASlnR2WGs6iYW4@46.243.183.115:3033/admin

MongoDB stores all user information, including name, password, and the user's authentication database, in the system.users collection in the admin database.

More Details : [https://docs.mongodb.com/manual/tutorial/enable-authentication/](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

db.createUser(  
  {  
    user: "lg",  
    pwd: "537nfrufhUYGUED",

    roles: [ { role: "readWrite", db: "lg-core" } ]  
  }  
)

db.auth('9CeKMhEKeLBJyOx3UUWbuBlNCBaE6LD6','XEYNHHSEOFxsJLysNHASlnR2WGs6iYW4')

db.auth( <username>, <password> )

db.createUser({ user:"admin", pwd: "nrfehuyFYUTTF8734578", roles: [{role: "userAdminAnyDatabase", db: "admin"}] })

use admin  
db.createUser(  
  {  
    user: 'admin',  
    pwd: 'nrfehuyFYUTTF8734578',  
    roles: [ { role: 'root', db: 'admin' } ]  
  }  
);

pymongo.MongoClient('mongodb://admin:537nfrufhUYGUED@vpakete-mongo:27017/vpakete-crm-db?authSource=vpakete-crm-db')

mongoimport --db vpakete-crm-db --collection user --file user.json --jsonArray

mongoimport --db vpakete-crm-db --collection user --file user.bson

mongorestore -d vpakete-crm-db -c user user.bson

mongodump --db vpakete-crm-db --collection user --out=import/

mongodump --db allpapers-core --collection user --out=import/

db = db.getSiblingDB('vpakete-crm-db')

## show users

db.getUsers()

show users

db.dropUser('vpakete-crm-user-admin')

## show databases

show databases  
show dbs

## show collections

show collections  
show tables  
db.getCollectionNames()

db.role.find()

## create user

|   |
|---|
|use products|
|db.createUser( { user: "accountAdmin01",|
|pwd: passwordPrompt(), // Or "<cleartext password>"|
|customData: { employeeId: 12345 },|
|roles: [ { role: "clusterAdmin", db: "admin" },|
|{ role: "readAnyDatabase", db: "admin" },|
|"readWrite"] },|
|{ w: "majority" , wtimeout: 5000 } )|

Databases -> Databases

Collections -> Tables

Documents -> Rows

Embedding

Referencing

[https://habr.com/ru/post/476590/](https://habr.com/ru/post/476590/)

![[mongo-1.png]]

