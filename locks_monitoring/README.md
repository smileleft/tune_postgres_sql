# pg_locks monitoring

## sql for view of pg_locks

```sql
select
  locktype,
  relation::regclass,
  virtualxid as vxid,
  transactionid as txid,
  virtualtransaction as vtxid,
  pid,
  mode,
  granted
from pg_locks
where pid in ({your pids})
order by pid, locktype;
```
