# tune_postgres_sql
postgreSQL query tunning

## how to enable pg_stat_statements

```bash

# edit postgresql.conf
shared_preload_libraries = 'pg_stat_statements'

# restart db and create extension
CREATE EXTENSION pg_stat_statements;
```
