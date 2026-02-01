# pg_stat_activity

```
datname
pid
usename
application_name
client_addr
xact_start
wait_event_type
wait_event
state
backend_xmin
query_id
query
```

## monitoring script example

```sql
select
  datname,
  pid,
  pg_blocking_pids(pid) as holder,
  substr(query, 1, 14) as query,
  state,
  extract(epoch from current_timestamp - query_start)::integer as querytime,
  extract(epoch from current_timestamp - xact_start)::integer as txntime,
  usename,
  application_name,
  wait_event_type,
  wait_event
from pg_stat_activity
where state in ('active', 'idle_in_transaction')
and pid not in (select pg_backend_pid())
order by 6 desc;
```
