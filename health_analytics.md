# 📍 Health Care Analytics - Case Study


Please find the dataset used to answer the questions here - [users](https://github.com/sarathchandrikak/Serious-SQL/blob/main/health_users.csv), [user_logs](https://github.com/sarathchandrikak/Serious-SQL/blob/main/health_user_logs.csv)



1. How many unique users exist in the logs dataset? 

```sql
select count (distinct id) from health.user_logs
```

2. How many total measurements do we have per user on average?
   
```sql
WITH user_measure_count AS (
    SELECT
      id,
      COUNT(*) AS measure_count,
      COUNT(DISTINCT measure) AS unique_measures
    FROM
      health.user_logs
    GROUP BY
      id
  )
SELECT
  ROUND(AVG(measure_count))
FROM
  user_measure_count;
```

3. What about the median number of measurements per user?

```sql
WITH cte2 AS (
    SELECT
      id,
      count(measure) AS countt
    FROM
      health.user_logs
    GROUP BY
      id
  )
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (
    ORDER BY
      countt
  )
FROM
  cte2;

```

4. How many users have 3 or more measurements?

```sql
SELECT
  COUNT(id)
FROM
  (
    SELECT
      id,
      COUNT(measure) AS mct
    FROM
      health.user_logs
    GROUP BY
      id
  )
WHERE
  mct >= 3;
```

5. How many users have 1,000 or more measurements?

```sql
SELECT
  COUNT(id)
FROM
  (
    SELECT
      id,
      COUNT(measure) AS mct
    FROM
      health.user_logs
    GROUP BY
      id
  )
WHERE
  mct >= 1000;
```

6. What is the number and percentage of the active user base who have logged blood glucose measurements?

```sql
SELECT
  COUNT(DISTINCT id)
FROM
  health.user_logs
WHERE
  measure = 'blood_glucose';
SELECT
  COUNT(id)
FROM
  (
    SELECT
      id,
      COUNT(measure) AS dfdf
    FROM
      health.user_logs
    GROUP BY
      id
    HAVING
      COUNT(measure) = 3
  );
```

7. What is the number and percentage of the active user base who have at least 2 types of measurements?

```sql
WITH mcte AS (
    SELECT
      id,
      COUNT(*) AS mcount
    FROM
      health.user_logs
    GROUP BY
      id
  )
SELECT
  id,
  mcount /(
    SELECT
      SUM(mcount)
    FROM
      mcte
  ) AS num,
  (
    mcount /(
      SELECT
        SUM(mcount)
      FROM
        mcte
    )
  ) * 100 AS percentage
FROM
  mcte
WHERE
  mcount >= 2;
```

8. What is the number and percentage of the active user base who have all 3 measures - blood glucose, weight and blood pressure?

```sql
WITH mcte AS (
    SELECT
      id,
      COUNT(*) AS mcount
    FROM
      health.user_logs
    GROUP BY
      id
  )
SELECT
  id,
  mcount /(
    SELECT
      SUM(mcount)
    FROM
      mcte
  ) AS num,
  (
    mcount /(
      SELECT
        SUM(mcount)
      FROM
        mcte
    )
  ) * 100 AS percentage
FROM
  mcte
WHERE
  mcount = 3;
```

9. What is the median systolic/diastolic blood pressure values?

```sql
WITH cte2 AS (
    SELECT
      id,
      systolic,
      diastolic
    FROM
      health.user_logs
    WHERE
      measure = 'blood_pressure'
  )
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (
    ORDER BY
      systolic
  ) AS spct,
  PERCENTILE_CONT(0.5) WITHIN GROUP (
    ORDER BY
      diastolic
  ) AS dpct
FROM
  cte2;
```


