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

## extract(epoch from)

```sql
select
  query_start,
  current_timestamp,
  current_timestamp - query_start as elapsed_time,
  extract(epoch from current_timestamp - query_start) as elapsed_time_sec
from pg_stat_activity
where pid = {your pid};
```

## Example: How to check slow query

```java
String fromDate = "20260130";
String toDate = "20260131";
String sql = "SELECT COUNT(*) FROM t_log_r WHERE log_date BETWEEN ? and ?";
Connection conn = DriverManager.getConnection(url, user, password);
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, fromdate);
pstmt.setString(2, toDate);
```

## Use: auto_explain

```bash
# edit config
shared_preload_libraries = 'pg_stat_statements, auto_explain'
auto_explain.log_min_duration = 60s # for batch: 60s, for online: 10s
auto_explain.log_analyze = on
auto_explain.log_buffers = on

# restart postgreSQL
pg_ctl reload # or postgres service restart
```

## check slow query

```bash
# configure pg_statements
shared_preload_libraries = 'pg_stat_statements,auto_explain'
pg_stat_statements.max = 10000
pg_stat_statements.track = all
track_activity_query_size = 64kb
```

```sql
select pg_stat_statememts_result(); -- pg_stat_statements init
select pg_sleep(1) from generate_series(1,1) i;
select pg_sleep(2) from generate_series(2,2) i
```
