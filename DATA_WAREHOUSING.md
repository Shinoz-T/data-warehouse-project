
### `DATA_WAREHOUSING.md`

    ```markdown
# Data Warehousing Design

## Designing the Star Schema

The star schema is a data modeling technique that simplifies complex queries and improves performance by organizing data into fact and dimension tables.

### Fact Table

- `factSales`: Contains measurable data like sales amounts.

### Dimension Tables

- `dimDate`: Contains date-related information.
- `dimCustomer`: Contains customer details.
- `dimStore`: Contains store details.
- `dimMovie`: Contains movie details.

## SQL Queries

### Creating and Populating Dimension Tables

    ```sql
    -- Creating and Populating Dimension Tables
    
    -- 1. Dimension Table: dimDate
    -- Contains date-related information
    CREATE TABLE dimDate (
        date_key INTEGER NOT NULL PRIMARY KEY,
        date DATE NOT NULL,
        year SMALLINT NOT NULL,
        quarter SMALLINT NOT NULL,
        month SMALLINT NOT NULL,
        day SMALLINT NOT NULL,
        week SMALLINT NOT NULL,
        is_weekend BOOLEAN
    );
    
    INSERT INTO dimDate (date_key, date, year, quarter, month, day, week, is_weekend)
    SELECT DISTINCT(TO_CHAR(payment_date::DATE, 'yyyyMMDD')::INTEGER) AS date_key,
           date(payment_date) AS date,
           EXTRACT(YEAR FROM payment_date) AS year,
           EXTRACT(QUARTER FROM payment_date) AS quarter,
           EXTRACT(MONTH FROM payment_date) AS month,
           EXTRACT(DAY FROM payment_date) AS day,
           EXTRACT(WEEK FROM payment_date) AS week,
           CASE WHEN EXTRACT(ISODOW FROM payment_date) IN (6, 7) THEN TRUE ELSE FALSE END AS is_weekend
    FROM payment;
    
    -- 2. Dimension Table: dimCustomer
    -- Contains customer details
    CREATE TABLE dimCustomer (
        customer_key INTEGER PRIMARY KEY,
        customer_id INTEGER NOT NULL,
        first_name VARCHAR(50),
        last_name VARCHAR(50),
        email VARCHAR(100),
        address VARCHAR(255),
        city VARCHAR(100),
        country VARCHAR(100)
    );
    
    INSERT INTO dimCustomer (customer_key, customer_id, first_name, last_name, email, address, city, country)
    SELECT DISTINCT customer_id AS customer_key,
           customer_id,
           first_name,
           last_name,
           email,
           address,
           city,
           country
    FROM customer;
    
    -- 3. Dimension Table: dimStore
    -- Contains store details
    CREATE TABLE dimStore (
        store_key INTEGER PRIMARY KEY,
        store_id INTEGER NOT NULL,
        store_name VARCHAR(100),
        store_location VARCHAR(255)
    );
    
    INSERT INTO dimStore (store_key, store_id, store_name, store_location)
    SELECT DISTINCT store_id AS store_key,
           store_id,
           store_name,
           store_location
    FROM store;
    
    -- 4. Dimension Table: dimMovie
    -- Contains movie details
    CREATE TABLE dimMovie (
        movie_key INTEGER PRIMARY KEY,
        movie_id INTEGER NOT NULL,
        title VARCHAR(255),
        release_year SMALLINT,
        genre VARCHAR(50)
    );
    
    INSERT INTO dimMovie (movie_key, movie_id, title, release_year, genre)
    SELECT DISTINCT movie_id AS movie_key,
           movie_id,
           title,
           release_year,
           genre
    FROM movie;

### Creating and Populating Fact Table

    INSERT INTO factSales (date_key, customer_key, movie_key, store_key, sales_amount)
    SELECT 
      TO_CHAR(payment_date :: DATE, 'yyyyMMDD')::integer AS date_key,
      p.customer_id AS customer_key,
      i.film_id AS movie_key,
      i.store_id AS store_key,
      p.amount AS sales_amount
    FROM payment p
    JOIN rental r ON (p.rental_id = r.rental_id)
    JOIN inventory i ON (r.inventory_id = i.inventory_id);


