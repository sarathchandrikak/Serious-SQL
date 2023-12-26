# 📍 Lesson 4 - Summary Statistics

Mean using avg function 
```sql
SELECT
  AVG(measure_value)
FROM health.user_logs;
```

Average/Mean for each measure
```sql
SELECT
  measure,
  ROUND(AVG(measure_value), 2) AS average,
  COUNT(*) AS counts
FROM health.user_logs
GROUP BY measure
ORDER BY counts DESC;
```

Median Algorithm

```
  Sort all N values from smallest to largest
  Inspect the central values of the sorted set:
  if N is odd:
    the median is the value in the N+12 th position
  else if N is even:
    the median is the average of values in the (N/2)th and 1+(N/2)th positions
```

Mode Algorithm

```
    Calculate the tally of values similar to a GROUP BY and COUNT
    The mode is the values with the highest number of occurences
```

Calculate mean, median, mode 
```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_value) AS median_value,
  MODE() WITHIN GROUP (ORDER BY measure_value) AS mode_value,
  AVG(measure_value) as mean_value
FROM health.user_logs
WHERE measure = 'weight';
```

## Spread of the Data

Min, Max, Range

```sql
SELECT
  MIN(measure_value) AS minimum_value,
  MAX(measure_value) AS maximum_value,
  MAX(measure_value) - MIN(measure_value) AS range_value
FROM health.user_logs
WHERE measure = 'weight';
```

EXPLAIN ANALYZE in front of each query, gives the Execution Time values.

```sql
EXPLAIN ANALYZE
WITH min_max_values AS (
  SELECT
    MIN(measure_value) AS minimum_value,
    MAX(measure_value) AS maximum_value
  FROM health.user_logs
  WHERE measure = 'weight'
)
SELECT
  minimum_value,
  maximum_value,
  maximum_value - minimum_value AS range_value
FROM min_max_values;
```

Variance for a sample data can be calculated as 
```sql
WITH sample_data (example_values) AS (
 VALUES
 (82), (51), (144), (84), (120), (148), (148), (108), (160), (86)
)
SELECT
  ROUND(VARIANCE(example_values), 2) AS variance_value,
  ROUND(STDDEV(example_values), 2) AS standard_dev_value,
  ROUND(AVG(example_values), 2) AS mean_value,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY example_values) AS median_value,
  MODE() WITHIN GROUP (ORDER BY example_values) AS mode_value
FROM sample_data;
```

Calculating all summary statistics 
```sql
SELECT
  'weight' AS measure,
  ROUND(MIN(measure_value), 2) AS minimum_value,
  ROUND(MAX(measure_value), 2) AS maximum_value,
  ROUND(AVG(measure_value), 2) AS mean_value,
  ROUND(
    CAST(PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_value) AS NUMERIC),
    2
  ) AS median_value,
  ROUND(
    MODE() WITHIN GROUP (ORDER BY measure_value),
    2
  ) AS mode_value,
  ROUND(STDDEV(measure_value), 2) AS standard_deviation,
  ROUND(VARIANCE(measure_value), 2) AS variance_value
FROM health.user_logs
WHERE measure = 'weight';
```

Excercises 

1. What is the average, median and mode values of blood glucose values to 2 decimal places?

```sql
SELECT
  ROUND(AVG(measure_value), 2),
  ROUND(
    CAST(
      PERCENTILE_CONT(0.5) WITHIN GROUP (
        ORDER BY
          measure_value
      ) AS NUMERIC
    ),
    2
  ) AS median_value,
  ROUND(
    MODE() WITHIN GROUP (
      ORDER BY
        measure_value
    ),
    2
  ) AS mode_value
from
  health.user_logs
where
  measure = 'blood_glucose';
```

2. What is the most frequently occuring measure_value value for all blood glucose measurements?

```sql
select MODE() WITHIN GROUP (order by measure_value) from health.user_logs where measure='blood_glucose';
SELECT
  measure_value,
  COUNT(*) as frequency
FROM
  health.user_logs
WHERE
  measure = 'blood_glucose'
GROUP BY
  measure_value
ORDER BY
  frequency DESC
LIMIT 10;
```

3. Calculate the 2 Pearson Coefficient of Skewness for blood glucose measures given the following formulas:
<img width="454" alt="image" src="https://github.com/sarathchandrikak/Serious-SQL/assets/65502906/4afea8b0-9795-4cbb-9169-841b6afd6c57">

```sql

WITH cte_blood_glucose_stats AS (
  SELECT
    AVG(measure_value) AS mean_value,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_value) AS median_value,
    MODE() WITHIN GROUP (ORDER BY measure_value) AS mode_value,
    STDDEV(measure_value) AS stddev_value
  FROM health.user_logs
  WHERE measure = 'blood_glucose'
)
SELECT
  ( mean_value - mode_value ) / stddev_value AS pearson_corr_1,
  3 * ( mean_value - median_value ) / stddev_value AS pearson_corr_2
FROM cte_blood_glucose_stats;

```
