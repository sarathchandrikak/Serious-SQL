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


# Exercises

1. Which id value has the most number of duplicate records in the health.user_logs table?
```sql
      Select id, 
      count(id) as id_count 
      from health.user_logs 
      group by id 
      order by id_count DESC 
      LIMIT 
      1;
```
  
2. Which log_date value had the most duplicate records after removing the max duplicate id value from question 1?
```sql
select
  log_date,
  count(log_date)
from
  health.user_logs
where
  id != (
    Select
      id
    from
      health.user_logs
    group by
      id
    order by
      count(id) DESC
    LIMIT
      1
  )
group by
  log_date
order by
  count(log_date) desc
limit
  1;
```

3. Which measure_value had the most occurences in the health.user_logs value when measure = 'weight'?
```sql
select
  measure_value,
  count(measure_value) as measurevalue_count
from
  health.user_logs
where
  measure = 'weight'
group by
  measure_value
order by
  measurevalue_count desc
limit
  1;
```

4. How many single duplicated rows exist when measure = 'blood_pressure' in the health.user_logs? How about the total number of duplicate records in the same table?
5. What percentage of records measure_value = 0 when measure = 'blood_pressure' in the health.user_logs table? How many records are there also for this same condition?
6. What percentage of records are duplicates in the health.user_logs table?
```sql
WITH all_measure_values AS (
  SELECT
    measure_value,
    COUNT(*) AS total_records,
    SUM(COUNT(*)) OVER () AS overall_total
  FROM
    health.user_logs
  WHERE
    measure = 'blood_pressure'
  GROUP BY
    1
)
SELECT
  ROUND(100 * total_records :: NUMERIC / overall_total, 2) AS percentage
FROM
  all_measure_values
WHERE
  measure_value = 0;
```








