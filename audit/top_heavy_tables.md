# Описание: Топ-20 самых тяжелых таблиц базы (данные + индексы)

```sql
SELECT 
    schemaname AS schema,
    relname AS table_name,
    pg_size_pretty(pg_total_relation_size(relid)) AS human_size,
    pg_stat_get_live_tuples(relid) AS estimated_rows
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(relid) DESC
LIMIT 20;
```
