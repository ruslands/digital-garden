select
pid as process_id,
usename as username,
datname as database_name,
client_addr as client_address,
application_name,
backend_start,
state,
state_change from pg_stat_activity;

Wait Event:

- ClientRead
- CheckpointerMain
- BgWriterHibernate
- WalWriterMain
- AutoVacuumMain
- LogicalLauncherMain

ClientRead

A session that waits for ClientRead is done processing the last query and waits for the client to send the next request.