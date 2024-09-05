
### `PERFORMANCE_COMPARISON.md`

# Performance Comparison

## Star Schema Query

    ```sql
    SELECT
      dimMovie.title,
      dimDate.month,
      dimCustomer.city,
      SUM(factSales.sales_amount) AS revenue
    FROM factSales
    JOIN dimMovie ON dimMovie.movie_key = factSales.movie_key
    JOIN dimDate ON dimDate.date_key = factSales.date_key
    JOIN dimCustomer ON dimCustomer.customer_key = factSales.customer_key
    GROUP BY dimMovie.title, dimDate.month, dimCustomer.city
    ORDER BY dimMovie.title, dimDate.month, dimCustomer.city, revenue DESC;

## 3NF Query
    SELECT
      f.title,
      EXTRACT(month FROM p.payment_date) AS month,
      ci.city,
      SUM(p.amount) AS revenue
    FROM payment p
    JOIN rental r ON (p.rental_id = r.rental_id)
    JOIN inventory i ON (r.inventory_id = i.inventory_id)
    JOIN film f ON (i.film_id = f.film_id)
    JOIN customer c ON (p.customer_id = c.customer_id)
    JOIN address a ON (c.address_id = a.address_id)
    JOIN city ci ON (a.city_id = ci.city_id)
    GROUP BY f.title, EXTRACT(month FROM p.payment_date), ci.city
    ORDER BY f.title, EXTRACT(month FROM p.payment_date), ci.city, revenue DESC;

  # Conclusions

## Star Schema vs. 3NF

### Star Schema:

- Simplifies queries by reducing the number of joins.
- Optimized for read-heavy operations and analytical queries.
- Provides faster query performance due to denormalization.

### 3NF:

- Ensures data integrity by normalizing data and reducing redundancy.
- Requires more complex queries with multiple joins.
- Better suited for transactional systems where data consistency is crucial.

## Performance Insights:

- Queries using the star schema typically execute faster compared to 3NF due to its simplified design and reduced number of joins.
- The performance difference becomes more pronounced with larger datasets, highlighting the efficiency benefits of the star schema in data warehousing scenarios.


    
