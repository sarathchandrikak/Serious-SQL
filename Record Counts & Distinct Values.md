# Lesson 2 - Record Counts and Distinct Values

* Total number of Records
```sql
SELECT COUNT(*) AS row_count
FROM dvd_rentals.film_list;
```
* Unique values also referred as dedupe, deduplicate, distinct 
```sql
SELECT DISTINCT rating
FROM dvd_rentals.film_list;
```
* Number of Distinct records
```sql
SELECT COUNT(DISTINCT category) 
AS unique_category_count 
FROM dvd_rentals.film_list
```

* Group by counts
### What is the frequency of values in the rating column in the film_list table?
```sql
SELECT rating, COUNT(rating) AS count
FROM dvd_rentals.film_list
GROUP BY Rating
ORDER BY COUNT DESC 
```
* Group by counts and displaying percentage
```sql
SELECT rating, COUNT(rating) AS count,
ROUND (100 * COUNT(rating) :: NUMERIC/ SUM(COUNT(rating)) OVER(), 2) AS percentage 
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY count DESC;
```
Definitely need to use NUMERIC keyword, else it gives floor of the division => 15 / 20 gets evaluated to 0, instead of 0.75

* Counts for multiple column combinations
### What are the 5 most frequent rating and category combinations in the film_list table?
```sql
SELECT rating, category, COUNT(rating) AS count
FROM dvd_rentals.film_list 
GROUP BY rating, category
ORDER BY count DESC 
LIMIT 5;
```
# Exercises

1. Which actor_id has the most number of unique film_id records in the dvd_rentals.film_actor table?
```sql

SELECT actor_id, count(DISTINCT film_id) as count
FROM dvd_rentals.film_actor
GROUP BY actor_id
ORDER BY count DESC 
LIMIT 1;
```
2. How many distinct fid values are there for the 3rd most common price value in the dvd_rentals.nicer_but_slower_film_list table?
```sql
SELECT price,
COUNT(DISTINCT fid) as count
FROM dvd_rentals.nicer_but_slower_film_list
GROUP BY price
ORDER BY count DESC;
```
3. How many unique country_id values exist in the dvd_rentals.city table?
```sql
SELECT COUNT(DISTINCT country_id) 
FROM dvd_rentals.city;
```
4. What percentage of overall total_sales does the Sports category make up in the dvd_rentals.sales_by_film_category table?
```sql
select category,
ROUND ( 100 * total_sales :: NUMERIC / SUM(total_sales) OVER(), 2)
AS percentage
FROM dvd_rentals.sales_by_film_category
```
5. What percentage of unique fid values are in the Children category in the dvd_rentals.film_list table?
```sql
SELECT category,
ROUND(100 * COUNT(DISTINCT fid) :: NUMERIC / SUM(COUNT(fid)) OVER(), 2)
AS percentage
FROM dvd_rentals.film_list
group by category;
```
