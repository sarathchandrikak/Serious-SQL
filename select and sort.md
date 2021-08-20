# Lesson 1 - Select and Sort Data

* Select all Columns
```sql
SELECT * FROM dvd_rentals.language;
```
* Select specific Columns
```sql
SELECT 
  language_id, 
  name 
FROM dvd_rentals.language;
```
* Limit Output Rows
```sql
SELECT *
FROM dvd_rentals.actor
LIMIT 10;
```
In SQL Server and Teradata TOP is used instead of LIMIT and it is placed infront of SELECT statement.

* Sort by text column
```sql
SELECT country
FROM dvd_rentals.country
ORDER BY country
LIMIT 5;
```
* Sort by Numeric/Date Column
```sql
SELECT 
  total_sales
FROM dvd_rentals.sales_by_film_category
ORDER BY 1
LIMIT 5;
```
* Sort by Descending
``` sql
SELECT country 
FROM dvd_rentals.country
ORDER BY country DESC
LIMIT 5;
```
* Sort by Multiple Columns (Can use combination of ascending and descending on column names)
```sql
SELECT *
FROM dvd_rentals.actor
ORDER BY first_name, last_nmae
LIMIT 5;
```
* Order by Non - Alphabetical Characters 
  * If null present in column after sorting records containing null end up at the last of the table.
  * If specified NULLS FIRST records containing null end up at the first of the table. 
  * Ordering is done as follows for strings 
      1. as Numbers
      2. starting with Special characters like '_'. '*' ' '
      3. as alpnabets
  * Can also apply DESC with these records

# Exercises 

1. What is the name of the category with the highest category_id in the dvd_rentals.category table?
```sql
SELECT name, category_id 
FROM dvd_rentals.category 
ORDER BY category_id DESC 
LIMIT 1;
```
2. For the films with the longest length, what is the title of the “R” rated film with the lowest replacement_cost in dvd_rentals.film table?
```sql
SELECT title, length, replacement_cost 
FROM dvd_rentals.film
WHERE rating = 'R'
ORDER BY length DESC, replacement_cost
LIMIT 1;
```
3. Who was the manager of the store with the highest total_sales in the dvd_rentals.sales_by_store table?
```sql
SELECT manager, total_sales 
FROM dvd_rentals.sales_by_store
ORDER BY total_sales DESC 
LIMIT 1;
```
4. What is the postal_code of the city with the 5th highest city_id in the dvd_rentals.address table?
```sql
SELECT postal_code, city_id 
FROM dvd_rentals.address
ORDER BY city_id DESC
LIMIT 5;
```
(Last record of the above values is with the 5th highest city_id)


