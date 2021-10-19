# 📍 Lesson 3 - Identifying Duplicate Records and Data cleaning 

* Record Counts

```sql
SELECT COUNT(*)
FROM health.user_logs;
```
* Unique Records

```sql
SELECT COUNT(DISTINCT ID) 
FROM health.user_logs;
```

* Single Column Frequency Counts By Measure

```sql
SELECT
  measure,
  COUNT(*) AS frequency,
  ROUND(
    100 * COUNT(*) / SUM(COUNT(*)) OVER (),
    2
  ) AS percentage
FROM health.user_logs
GROUP BY measure
ORDER BY frequency DESC;
```

* Single Column Frequency Counts by Id
```sql
SELECT
  id,
  COUNT(*) AS frequency,
  ROUND(
    100 * COUNT(*) / SUM(COUNT(*)) OVER (),
    2
  ) AS percentage
FROM health.user_logs
GROUP BY id
ORDER BY frequency DESC
LIMIT 10;
```

* Individual Column Distributions
    * Measure 

```sql
SELECT
  measure_value,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```

  * Systolic
```sql
SELECT
  systolic,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```
  * Diastolic
```sql
SELECT
  Diastolic
  COUNt(*) AS frequency
FROM health.user_logs
GROUP BY 1
ORDER BY 2 DESC 
LIMIT 10;
```





