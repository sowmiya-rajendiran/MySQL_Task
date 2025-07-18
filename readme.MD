## SQL Task 

Create Database :
```sql 
    CREATE DATABASE ecommerce;
```

Select Database :
```sql
    SELECT DATABASE();
```

Use Database :
```sql 
    USE ecommerce; 
```
Create table name of customers :
``` sql
    CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    address VARCHAR(200)
    );
```
create another table name of orders:
``` sql 
    CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
    );
```
create another table name of products:
``` sql
    CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    productname VARCHAR(100),
    price DECIMAL(10,2),
    description VARCHAR(100)
    );
```
And Then fill some sample datas on all the tables done:- using
```sql
    INSERT INTO customers (name,email,address) VALUES ..... -- same as orders and products for insert data 
```

## Queries to Write:

- 1.Retrieve all customers who have placed an order in the last 30 days.
```sql
    SELECT c.id, c.name, c.email, c.address, o.order_date 
    FROM customers AS c
    JOIN orders AS o ON c.id = o.customer_id 
    WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;

    --Retrieve all customer records using a JOIN because we want the date based on the orders table.
    --CURDATE() returns the current date, and subtracting 30 days gives the date 30 days ago.
    --Using the condition >= ensures we get data from the last 30 days.
```

- 2.Get the total amount of all orders placed by each customer.
```sql
    SELECT customer_id, SUM(total_amount)
    FROM orders
    GROUP BY customer_id;

    -- Get the total amount for each customers using group by.
```

- 3.Update the price of Product C to 45.00.
``` sql 
    UPDATE products SET price=45.00 WHERE id=3; 

    -- this query update the product price using where for particular product update .
```

4.Add a new column discount to the products table.
``` sql
    ALTER TABLE products ADD column discount DECIMAL(10,2);

    -- using alter table for add discount column in products table
```

5.Retrieve the top 3 products with the highest price.
```sql
    SELECT * FROM products ORDER BY price DESC LIMIT 3 ;

    -- I am retrieving the highest-priced products by using ORDER BY in descending order and LIMIT 3 to get only the top 3 products.
```

6.Get the names of customers who have ordered Product A.
```sql
    SELECT DISTINCT c.name
    FROM customers AS c
    JOIN orders AS o ON c.id = o.customer_id
    JOIN order_items AS oi ON o.id = oi.order_id
    JOIN products AS p ON oi.product_id = p.id
    WHERE p.productname = 'Product A';

    -- JOINs are used to connect all related tables:
    -- customers → orders → order_items → products
    -- WHERE clause filters only orders containing 'Product A'
    -- DISTINCT ensures no duplicate customer names in the result, even if a customer ordered it more than once

```

7.Join the orders and customers tables to retrieve the customer's name and order date for each order. 
``` sql 
    SELECT customers.name,orders.order_date FROM customers JOIN orders 
    ON customers.id = orders.customer_id;
    --(OR)
    SELECT c.name,o.order_date FROM cutomers AS c JOIN orders AS o ON c.id = o.customer_id;

    -- I am retrieving customer name from customer table and order date from orders table using join 
```

8.Retrieve the orders with a total amount greater than 150.00.
``` sql
    SELECT * FROM orders WHERE total_amount>150;

    -- I am retrieving orders with condition of total amount greater than 150.00.
```

9.Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.
``` sql
    CREATE TABLE order_items (
	id int AUTO_INCREMENT PRIMARY KEY,
    order_id INT ,
    product_id INT,
    quantity DECIMAL(20,2),
    price DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
    );

    -- We create a separate "order_items" table to store each product line in an order.
    -- Each row in "order_items" represents: 
    -- One product in a specific order
    -- Quantity and price of that product
    --The "order_items" table references the "orders" table using a foreign key. 
    
```
10.Retrieve the average total of all orders.
``` sql
    SELECT avg(total_amount) FROM orders;
    
    -- avg() used for getting avg total.
```