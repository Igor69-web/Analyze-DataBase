# Поиск таблицы по названию колонки
```sql
SELECT 
    table_schema, 
    table_name
FROM information_schema.columns
WHERE column_name ILIKE '%inn_customer%' -- Замените на часть названия
ORDER BY table_schema, table_name;
```
