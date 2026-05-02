# Анализ активности чтения и записи
## Этот запрос покажет, к каким таблицам реально обращаются пользователи и сервисы, а какие — «заброшки».

```sql
SELECT 
    schemaname, 
    relname AS table_name,
    seq_scan + idx_scan AS total_reads,     -- Сколько раз читали
    n_tup_ins + n_tup_upd + n_tup_del AS total_writes, -- Сколько раз меняли
    last_seq_scan,                          -- Дата последнего полного чтения
    last_idx_scan                           -- Дата последнего чтения по индексу
FROM pg_stat_user_tables
WHERE (seq_scan + idx_scan) > 0             -- Смотрим только активные
ORDER BY total_reads DESC;
```
