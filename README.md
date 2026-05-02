# Analyze-DataBase
## ОГЛАВЛЕНИЕ
[Обзор схем](https://github.com/Igor69-web/Analyze-DataBase/blob/main/audit/schema_overview.md) — сколько у нас таблиц.


## 🚀 Анализ активности таблиц
Этот скрипт помогает найти топ-50 самых читаемых таблиц в PostgreSQL.
> Примечание: Требуются права доступа к pg_stat_user_tables

```sql
SELECT 
    schemaname, 
    relname as table_name, 
    n_tup_ins + n_tup_upd + n_tup_del as write_activity, 
    seq_scan + idx_scan as read_activity,               
    last_vacuum, 
    last_analyze
FROM pg_stat_user_tables
ORDER BY read_activity DESC
LIMIT 50;
```
[Вернуться к оглавлению](#оглавление)
