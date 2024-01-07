# 📍 Lesson 5 - Distribution Functions

Cumulative distribution: The probability of any value between the minimum value of our dataset X and the value V as shown below
<img width="378" alt="image" src="https://github.com/sarathchandrikak/Serious-SQL/assets/65502906/15632de0-0e69-4199-b79f-0fbcf6508cb8">


Dividing data into 100 buckets and finding min, max, count in each bucket
```sql
WITH percentile_values AS (
  SELECT
    measure_value,
    NTILE(100) OVER (
      ORDER BY
        measure_value
    ) AS percentile
  FROM health.user_logs
  WHERE measure = 'weight'
)
SELECT
  percentile,
  MIN(measure_value) AS floor_value,
  MAX(measure_value) AS ceiling_value,
  COUNT(*) AS percentile_counts
FROM percentile_values
GROUP BY percentile
ORDER BY percentile;
```

Window functions for sorting values
```sql
WITH percentile_values AS (
  SELECT
    measure_value,
    NTILE(100) OVER (
      ORDER BY
        measure_value
    ) AS percentile
  FROM health.user_logs
  WHERE measure = 'weight'
)
SELECT
  measure_value,
  ROW_NUMBER() OVER (ORDER BY measure_value DESC) as row_number_order,
  RANK() OVER (ORDER BY measure_value DESC) as rank_order,
  DENSE_RANK() OVER (ORDER BY measure_value DESC) as dense_rank_order
FROM percentile_values
WHERE percentile = 100
ORDER BY measure_value DESC;
```


Removing Outliers
```sql
DROP TABLE IF EXISTS clean_weight_logs;
CREATE TEMP TABLE clean_weight_logs AS (
  SELECT *
  FROM health.user_logs
  WHERE measure = 'weight'
    AND measure_value > 0
    AND measure_value < 201
);
```

NTILE 

The top 10 rows when ordered by measure_value increasing

```sql
SELECT
  measure_value,
  NTILE(100) OVER (
    ORDER BY
      measure_value
  ) AS percentile
FROM
  health.user_logs
WHERE
  measure = 'weight'
ORDER BY
  measure_value
LIMIT
  10;
```

CUME_DIST 

```sql
SELECT
  measure_value,
  CUME_DIST() OVER (
    ORDER BY
      measure_value
  ) AS percentile
FROM health.user_logs
WHERE measure = 'weight'
ORDER BY measure_value
LIMIT 10;
```
