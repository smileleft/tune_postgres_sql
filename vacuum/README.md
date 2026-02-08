# vacuum

## how to vacuum table

```sql
vacuum ({option} {table};

# example
vacuum (verbose) table1;
```

## how to disable Autovacuum

```sql
alter system set autovacuum=off;
```

## how to get dead tuple

```sql
select lp, t_xmin, t_xmax from heap_page_items(get_raw_page('{target table}', 0));
```

## parameter for vacuum/autovacuum

```bash
autovacuum_naptime
autovacuum_max_workers
autovacuum_vacuum_cost_delay
autovacuum_vacuum_cost_limit

autovacuum_analyze_scale_factor
autovacuum_analyze_threshold
autovacuum_vacuum_insert_scale_factor
autovacuum_vacuum_insert_threshold
autovacuum_vacuum_scale_factor
autovacuum_vacuum_threshold
```

## how to calculate time to create statistics data

```bash
autovacuum_analyze_threshold(50) + autovacuum_analyze_scale_factor(0.1) X reltuples
```

## how to calculate time to vacuum for inserted record

```bash
autovacuum_vacuum_insert_threshold(1000) + autovacuum_vacuum_insert_scale_factor(0.2) X reltuples
```
