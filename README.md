# tune_postgres_sql
postgreSQL query tunning

## how to enable pg_stat_statements

```bash

# postgresql.conf
shared_preload_libraries = 'pg_stat_statements'
```
