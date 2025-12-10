# Project-SQL — MavenMovies Analysis

This repository contains a short SQL analysis project performed against the `mavenmovies` sample database.  
The goal was to extract useful business and security-related metrics such as staff and customer information, inventory distribution across stores, film diversity, replacement costs, and payment statistics.

All queries below were run against the `mavenmovies` database (MySQL / MariaDB syntax). The SQL files or queries used are included in this repository.

---

## How to run

1. Restore or connect to the `mavenmovies` database in your MySQL/MariaDB server.
2. Use a SQL client (mysql CLI, MySQL Workbench, DBeaver, etc.).
3. Run:
   USE mavenmovies;
4. Copy and run the queries shown in the "Queries" section below.

---

## Queries and purpose

1) List all staff members (first name, last name, email, and store id)
```sql
SELECT
  first_name,
  last_name,
  email,
  store_id
FROM staff;
```
Purpose: Inventory of employees and which store they work at.

2) Count of inventory items held at each store
```sql
SELECT store_id, COUNT(inventory_id) AS total_inventory
FROM inventory
GROUP BY store_id;
```
Purpose: Compare stock levels between stores.

3) Count of active customers per store
```sql
SELECT store_id, COUNT(customer_id) AS active_customers
FROM customer
WHERE active = 1
GROUP BY store_id;
```
Purpose: Measure active customer base per location.

4) Count of all customer email addresses stored
```sql
SELECT COUNT(email) AS email_count
FROM customer;
```
Purpose: Assess size of contact data (useful for breach/liability analysis).

5) Diversity of film offering
- Unique film titles in inventory at each store:
```sql
SELECT store_id, COUNT(DISTINCT film_id) AS unique_films
FROM inventory
GROUP BY store_id;
```
- Count of unique film categories available (overall)
```sql
SELECT COUNT(DISTINCT category_id) AS unique_categories
FROM film_category;
```
Purpose: Understand variety of film catalogue and how it differs between stores.

6) Replacement cost statistics for films (min, max, average)
```sql
SELECT
  MIN(replacement_cost) AS min_replacement_cost,
  MAX(replacement_cost) AS max_replacement_cost,
  AVG(replacement_cost) AS avg_replacement_cost
FROM film;
```
Purpose: Estimate replacement liability and average replacement expense.

7) Payment monitoring: average and maximum payment processed
```sql
SELECT
  MAX(amount) AS max_payment,
  AVG(amount) AS avg_payment
FROM payment;
```
Purpose: Detect unusually large transactions and understand payment size distribution.

8) Customer rental volume — list customers and their all-time rental counts (highest first)
```sql
SELECT
  customer_id,
  COUNT(rental_id) AS number_of_rentals
FROM rental
GROUP BY customer_id
ORDER BY number_of_rentals DESC;
```
Purpose: Identify top customers by rental frequency.

---

## Results

This repository contains the queries. If you want me to add:
- The exported result sets (CSV/JSON),
- Visualizations (charts of inventory / customers / payments),
- Or an automated script to run all queries and save outputs,

tell me which format you prefer and I will add them.

---

## Assumptions & notes

- Database name used: `mavenmovies` (run `USE mavenmovies;` before queries).
- Column names and table names match the schema present in the sample dataset (e.g., staff, inventory, customer, film, film_category, payment, rental).
- `active` is a boolean or integer flag where `1` means active.
- AVG values are returned by SQL as decimals; formatting/rounding can be applied as needed.
- Depending on SQL mode and server, AVG(replacement_cost) may return many decimals — consider ROUND(..., 2) if you want currency formatting.

---

## Next steps (suggestions)

- Export query outputs to CSV for reporting.
- Add simple Python or shell script to run queries and save results automatically.
- Add data visualizations to show inventory distribution and customer activity over time.
- Implement monitoring alerts for unusually large payments.

---

## License & contact

- License: MIT (add a LICENSE file if you want this explicitly).
- Author: sainathnallapillai-tech

If you'd like, I can:
- Add these queries into .sql files in this repo,
- Create a runnable script that connects to the DB and exports results,
- Or produce example CSV outputs from sample data.
