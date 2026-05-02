# Analyze-DataBase
## ОГЛАВЛЕНИЕ
### 📂 Структура репозитория
* `audit/` - скрипты для анализа состояния БД.
  * [Обзор БД](https://github.com/Igor69-web/Analyze-DataBase/blob/main/audit/schema_overview.md) - Общая информация Базы данных.
  * [Тяжелые таблицы](https://github.com/Igor69-web/Analyze-DataBase/blob/main/audit/top_heavy_tables.md) - где занято место.
  * [Активность](https://github.com/Igor69-web/Analyze-DataBase/blob/main/audit/table_activity.md) - что реально используется.
 * `performance/` - фишечные скрипты SQL
   * [Проверка БД](https://github.com/Igor69-web/Analyze-DataBase/blob/main/performance/active_queries.md) - если тормозит БД.

### 🛠️ Используемые инструменты
* PostgreSQL 13+
* GitHub для версионирования скриптов
* DBeaver / psql
  
### 🗺️ План работы
* Провести техническую разведку (анализ веса и активности таблиц).
* Создать Data Dictionary (текстовое описание всех сущностей).
* Выявить и устранить аномалии в данных.
* Оптимизировать производительность (индексы и процедуры).


