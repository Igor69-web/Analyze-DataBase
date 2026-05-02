# Описание: Общая информация БД

## Статистика по количеству таблиц в разрезе схем
```sql
SELECT 
    schemaname AS schema, 
    count(*) AS tables_count
FROM pg_catalog.pg_tables
WHERE schemaname NOT IN ('pg_catalog', 'information_schema')
GROUP BY schemaname
ORDER BY tables_count DESC;
```
## В PostgreSQL есть стандартный механизм комментариев. Иногда разработчики их пишут, но в консоли их не видно.
> Чтобы разобраться, какая таблица что значит
```sql
SELECT 
    cols.table_name, 
    cols.column_name, 
    pd.description
FROM information_schema.columns cols
JOIN pg_catalog.pg_statio_user_tables st ON cols.table_name = st.relname
LEFT JOIN pg_catalog.pg_description pd ON pd.objoid = st.relid 
    AND pd.objsubid = cols.ordinal_position
WHERE cols.table_schema = 'public'
LIMIT 50;
```
## Карта связей
```sql
SELECT
    conname AS constraint_name,
    conrelid::regclass AS table_from,
    confrelid::regclass AS table_to
FROM pg_constraint
WHERE contype = 'f' 
  AND connamespace = 'public'::regnamespace;
```
