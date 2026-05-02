# Список запросов, которые выполняются дольше 5 секунд
```sql
SELECT 
    pid, 
    now() - xact_start AS duration, 
    query, 
    state,
    usename AS user
FROM pg_stat_activity
WHERE state != 'idle' 
  AND (now() - xact_start) > interval '5 seconds'
ORDER BY duration DESC;
```
