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
