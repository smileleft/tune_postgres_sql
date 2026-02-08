# Slow Query and Vacuum

## how to check slow query

```sql
select pg_current_xact_id()::text::bigint - min(backend_xmin::text::bigint) from pg_stat_activity;
```
