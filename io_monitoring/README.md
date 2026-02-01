# IO performance index

```bash
reads/read_time
writes/write_time
writebacks/writeback_time
extends/extend_time
hits
evictions
reuses
fsyncs/fsync_time
```

## query example for IO monitoring(from pg_stat_io)

```sql
select
  backend_type,
  object,
  content,
  round((rtime + wtime) / 1000, 1) as iotime_s,
  round((rtime + wtime) / sum(rtime + wtime) over() * 100, 1) as "pct(%)",
  round(rtime / 1000, 1) as rtime_s,
  round(wtime / 1000, 1) as wtime_s,
  (reads + writes) as totio,
  round((reads + writes) / sum(reads + writes) over() * 100, 1) as "pct(%)",
  reads,
  writes
from (
  select
    backend_type,
    object,
    context,
    coalesce(read_time::numeric, 0) as rtime,
    coalesce(write_time::numeric, 0) as wtime,
    coalesce(reads, 0) as reads,
    coalesce(writes, 0) as writes
  from pg_stat_io
) t
order by 4 desc;
```
