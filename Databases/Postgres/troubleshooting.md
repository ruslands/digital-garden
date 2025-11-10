Administer - Jobs - Session manager / Lock Manager

SELECT pg_terminate_backend(pg_stat_activity.pid)  
FROM pg_stat_activity  
WHERE pg_stat_activity.datname ='staging' AND pid <> pg_backend_pid();

SELECTpg_terminate_backend(pg_stat_activity.pid)

FROMpg_stat_activity

WHEREpg_stat_activity.datname='staging'ANDpg_stat_activity.usename= 'stabdteuwi';

IOWAIT

Проблема с iowait в PostgreSQL может возникнуть из-за высокой загрузки диска (доли времени, которую процессор ожидает завершения операций ввода-вывода). Это может быть вызвано интенсивной активностью записи на диск или другими проблемами ввода-вывода.

LWLock:BufferIO

Deadlock

if transaction 1 has to wait for transaction 2 and transaction 2 has to wait for transaction 1

In that case, the system has two choices:

- Wait infinitely
- Abort one transaction and commit the other transaction.

|     |                                                                                                                                                                  |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | Common causes for the LWLock: BufferIO event to appear in top waits include the following:                                                                       |
|     | • Multiple backends or connections trying to access the same page that's also pending an 1/0 operation                                                           |
|     | • The ratio between the size of the shared buffer pool (defined by the shared_buffers parameter) and the number of buffers needed by the current<br><br>workload |
|     | • The size of the shared buffer pool not being well balanced with the number of pages being evicted by other operations                                          |
|     | • Large or bloated indexes that require the engine to read more pages than necessary into the shared buffer pool                                                 |
|     | • Lack of indexes that forces the DB engine to read more pages from the tables than necessary                                                                    |
|     | • Sudden spikes for database connections trying to perform operations on the same page                                                                           |