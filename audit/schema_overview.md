# Описание: Статистика по количеству таблиц в разрезе схем
```sql
SELECT 
    schemaname AS schema, 
    count(*) AS tables_count
FROM pg_catalog.pg_tables
WHERE schemaname NOT IN ('pg_catalog', 'information_schema')
GROUP BY schemaname
ORDER BY tables_count DESC;
```

