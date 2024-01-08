# Data and Table exploration


1. Create temporary table by joining all the tables - rental, inventory, film_category

```sql
DROP TABLE IF EXISTS complete_joint_dataset;
CREATE TEMP TABLE complete_joint_dataset AS
SELECT
  rental.customer_id,
  inventory.film_id,
  film.title,
  rental.rental_date,
  category.name AS category_name
FROM dvd_rentals.rental
INNER JOIN dvd_rentals.inventory
  ON rental.inventory_id = inventory.inventory_id
INNER JOIN dvd_rentals.film
  ON inventory.film_id = film.film_id
INNER JOIN dvd_rentals.film_category
  ON film.film_id = film_category.film_id
INNER JOIN dvd_rentals.category
  ON film_category.category_id = category.category_id;
```

2. Display customer_id based on number of rentals high to low

```sql
SELECT
  customer_id,
  category_name,
  COUNT(*) AS rental_count,
  MAX(rental_date) AS latest_rental_date
FROM complete_joint_dataset
WHERE customer_id in (1, 2, 3)
GROUP BY
  customer_id,
  category_name
ORDER BY
  customer_id,
  rental_count DESC;
```

3. Display Each customers’ aggregated rental_count values for every category_name value

```sql
SELECT
  customer_id,
  category_name,
  COUNT(*) AS rental_count,
  MAX(rental_date) AS latest_rental_date
FROM complete_joint_dataset
WHERE customer_id in (1, 2, 3)
GROUP BY
  customer_id,
  category_name
ORDER BY
  customer_id,
  rental_count DESC;
```

4. Calculate Average Rental count on categories

```sql
WITH aggregated_rental_count AS (
  SELECT
    customer_id,
    category_name,
    COUNT(*) AS rental_count
  FROM complete_joint_dataset
  WHERE customer_id in (1, 2, 3)
  GROUP BY
    customer_id,
    category_name
)
SELECT
  category_name,
  ROUND(AVG(rental_count), 1) AS avg_rental_count
FROM aggregated_rental_count
GROUP BY
  category_name
ORDER BY
  category_name;
```

5. Display Customer Rental Count for Customer = 1

```sql

DROP TABLE IF EXISTS category_rental_counts;
CREATE TEMP TABLE category_rental_counts AS
SELECT
  customer_id,
  category_name,
  COUNT(*) AS rental_count,
  MAX(rental_date) AS latest_rental_date
FROM complete_joint_dataset_with_rental_date
GROUP BY
  customer_id,
  category_name;

-- profile just customer_id = 1 values sorted by desc rental_count
SELECT *
FROM category_rental_counts
WHERE customer_id = 1
ORDER BY
  rental_count DESC;
```

6. Total Customer Rentals

```sql
DROP TABLE IF EXISTS customer_total_rentals;
CREATE TEMP TABLE customer_total_rentals AS
SELECT
  customer_id,
  SUM(rental_count) AS total_rental_count
FROM category_rental_counts
GROUP BY customer_id;

-- show output for first 5 customer_id values
SELECT *
FROM customer_total_rentals
WHERE customer_id <= 5
ORDER BY customer_id;
```

7. Average Category Rentals

```sql
DROP TABLE IF EXISTS average_category_rental_counts;
CREATE TEMP TABLE average_category_rental_counts AS
SELECT
  category_name,
  AVG(rental_count) AS avg_rental_count
FROM category_rental_counts
GROUP BY
  category_name;

-- output the entire table by desc avg_rental_count
SELECT *
FROM average_category_rental_counts
ORDER BY
  avg_rental_count DESC;
```

8. Percentile Ranks

```sql

SELECT
  customer_id,
  category_name,
  rental_count,
  PERCENT_RANK() OVER (
    PARTITION BY category_name
    ORDER BY rental_count DESC
  ) AS percentile
FROM category_rental_counts
ORDER BY customer_id, rental_count DESC
LIMIT 14;
```

9.  Checking Datatypes from schema

```sql
SELECT
  table_name,
  column_name,
  data_type
FROM information_schema.columns
WHERE table_name in ('customer_total_rentals', 'category_rental_counts');
```
