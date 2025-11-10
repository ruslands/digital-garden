/usr/local/Cellar/postgresql@14/14.7/bin

| Tool Name         | Description                                                        | Example of Command                      |
| ----------------- | ------------------------------------------------------------------ | --------------------------------------- |
| clusterdb         | Clusters a PostgreSQL database                                     |                                         |
| ecpg              | Embedded SQL C preprocessor                                        |                                         |
| pg_basebackup     | Takes a base backup of a PostgreSQL server                         |                                         |
| pg_dump           | Exports a database into a script file or archive                   |                                         |
| pg_resetwal       | Resets the write-ahead log (WAL) of a PostgreSQL server            |                                         |
| pg_upgrade        | Upgrades a PostgreSQL server to a newer version                    |                                         |
| postmaster        | Legacy name for the PostgreSQL server process                      |                                         |
| createdb          | Creates a new PostgreSQL database                                  | `createdb mydb`                         |
| initdb            | Initializes a PostgreSQL data directory                            |                                         |
| pg_checksums      | Verifies or enables checksums in a PostgreSQL cluster              |                                         |
| pg_dumpall        | Exports all databases in a PostgreSQL cluster                      |                                         |
| pg_restore        | Restores a PostgreSQL database from an archive                     |                                         |
| pg_verifybackup   | Verifies the integrity of a PostgreSQL backup                      |                                         |
| psql              | Interactive terminal for PostgreSQL                                | `psql -h localhost -p 5432 -U postgres` |
| createuser        | Creates a new PostgreSQL role                                      | `createuser -s postgres`                |
| oid2name          | Resolves OIDs to names in a PostgreSQL database                    |                                         |
| pg_config         | Displays PostgreSQL configuration information                      |                                         |
| pg_isready        | Checks the connection status of a PostgreSQL server                |                                         |
| pg_rewind         | Synchronizes a PostgreSQL data directory after failover            |                                         |
| pg_waldump        | Displays contents of write-ahead log (WAL) files                   |                                         |
| reindexdb         | Rebuilds indexes in a PostgreSQL database                          |                                         |
| dropdb            | Drops a PostgreSQL database                                        | `dropdb mydb`                           |
| pg_amcheck        | Checks for corruption in PostgreSQL databases                      |                                         |
| pg_controldata    | Displays control information of a PostgreSQL cluster               |                                         |
| pg_receivewal     | Streams write-ahead log (WAL) from a PostgreSQL server             |                                         |
| pg_test_fsync     | Tests disk sync performance for PostgreSQL                         |                                         |
| pgbench           | Runs a benchmark test on PostgreSQL                                |                                         |
| vacuumdb          | Cleans and analyzes a PostgreSQL database                          |                                         |
| dropuser          | Removes a PostgreSQL role                                          |                                         |
| pg_archivecleanup | Cleans up PostgreSQL archive logs                                  |                                         |
| pg_ctl            | Utility to initialize, start, stop, or control a PostgreSQL server | `pg_ctl -D <path-to-bin> stop`          |
|                   |                                                                    | `pg_ctl -D <path-to-bin> start`         |
| pg_recvlogical    | Receives logical replication changes from PostgreSQL               |                                         |
| pg_test_timing    | Tests timing overhead for PostgreSQL                               |                                         |
| postgres          | The PostgreSQL database server process                             |                                         |
| vacuumlo          | Removes orphaned large objects from a PostgreSQL database          |                                         |

