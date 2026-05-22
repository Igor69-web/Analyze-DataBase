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
## Описание таблицы
>Для того чтобы посмотреть описание созданной таблицы, можно воспользоваться SQL запросом к информационной схеме
```sql
SELECT column_name, data_type, is_nullable, column_default
FROM information_schema.columns
WHERE table_schema = current_schema() AND table_name = 'users';
```
## Проверка внешних ключей по таблице (на примере поиска связей с таблицей (справочник)
```sql
SELECT
    -- 1. Справочник
    ccu.table_name AS dictionary_table,
    ccu.column_name AS dictionary_column, -- Обычно это id
    
    -- 2. Таблица фактов, которая от него зависит
    tc.table_name AS referencing_table,
    (SELECT obj_description(c.oid, 'pg_class') 
     FROM pg_class c 
     JOIN pg_namespace n ON n.oid = c.relnamespace 
     WHERE c.relname = tc.table_name AND n.nspname = tc.table_schema) AS referencing_table_comment,
    
    -- Поле-проводник в таблице фактов
    kcu.column_name AS referencing_column,
    
    -- Примерный объем таблицы фактов (чтобы оценить масштаб)
    (SELECT reltuples::bigint 
     FROM pg_class 
     WHERE relname = tc.table_name 
       AND relnamespace = (SELECT oid FROM pg_namespace WHERE nspname = tc.table_schema)) AS referencing_table_rows

FROM information_schema.table_constraints AS tc 
JOIN information_schema.key_column_usage AS kcu
  ON tc.constraint_name = kcu.constraint_name
  AND tc.table_schema = kcu.table_schema
JOIN information_schema.constraint_column_usage AS ccu
  ON ccu.constraint_name = tc.constraint_name
WHERE tc.constraint_type = 'FOREIGN KEY' 
  -- Имя таблицы для проверки:
  AND ccu.table_name = '{table]' 
ORDER BY referencing_table;
```
