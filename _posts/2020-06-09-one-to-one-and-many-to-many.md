Script for MySQL, that explain basics of One-To-One and Many-To-Many relationship with practical examples:
```mysql-psql
CREATE DATABASE relationship;

USE relationship;

-- ONE TO ONE example:
CREATE TABLE customer (
    customer_id INT,
    first_name VARCHAR(55) NOT NULL,
    last_name VARCHAR(55),
    CONSTRAINT PK_Customer PRIMARY KEY (customer_id)
);
-- NOTE: constraint is optional word, if you don't provide the name of constraint "PK_Customer", MSQL Server will generate it for you, for example "PK_Customer_23874228273"
-- BEST PRACTICE - always specify name of constraint

CREATE TABLE address (
    address_id INT,
    building_number VARCHAR(55) NOT NULL,
    street VARCHAR(55) NOT NULL,
    city VARCHAR(55),
    post_code VARCHAR(55) NOT NULL,
    country VARCHAR(55),
    customer_address_id INT,
    CONSTRAINT PK_Address PRIMARY KEY (address_id),
    CONSTRAINT FK_Address FOREIGN KEY (customer_address_id) REFERENCES customer (customer_id)
);
--
INSERT INTO customer 
(customer_id, first_name, last_name) 
VALUES 
(1, 'Jon', 'Flanders'),
(2, 'Sam', 'Smith');

INSERT INTO address 
(address_id, building_number, street, city, post_code, country) 
VALUES 
(1, '20', 'Birch Alley', 'London', 'SE24 0AB', 'UK'),
(2, '17', 'Oak Street', 'London', 'SE25 0XY', NULL);

select * from address;
-- it will show you that data saved but values in column customer_address_id is NULL

INSERT INTO address 
(address_id, building_number, street, city, post_code, country, customer_address_id) 
VALUES 
(3, '20', 'Birch Alley', 'London', 'SE24 0AB', 'UK', 1);
-- this will be OK as customer_id = 1 exist in table customer

INSERT INTO address 
(address_id, building_number, street, city, post_code, country, customer_address_id) 
VALUES 
(4, '20', 'Birch Alley', 'London', 'SE24 0AB', 'UK', 3);
-- this will be failed as customer_id = 3 doesn't exist in table customer

-- WHAT IF WILL DELETE ROW WITH customer_id = 1 FROM CUSTOMER TABLE ???
-- (we already have a refernce on this row from address table!)
DELETE FROM customer WHERE customer_id = 1;
-- so it won't let you delete this row as it already have a reference to address table
-- you firstly need to delete with record from address:
DELETE FROM address WHERE customer_address_id = 1;
-- after it you can delete this row from customer:
DELETE FROM customer WHERE customer_id = 1;


-- MANY TO MANY example - orders and items

CREATE TABLE orders (
    order_id INT,
    orders_customer_id INTEGER NOT NULL,
    order_date DATE NOT NULL,
    CONSTRAINT PK_order_id PRIMARY KEY (order_id),
    CONSTRAINT FK_oreder_customer_id FOREIGN KEY (orders_customer_id) REFERENCES customer (customer_id) -- remember: it's example of ONE TO MANY relation
);

CREATE TABLE items (
    item_id INT,
    item_name VARCHAR(50) NOT NULL,
    item_color VARCHAR(50) NULL,
    items_orders_id INT,
    CONSTRAINT PK_item_id PRIMARY KEY (item_id),
    CONSTRAINT FK_item_id FOREIGN KEY (items_orders_id) REFERENCES orders (order_id)
);

ALTER TABLE orders ADD COLUMN items_order_id INT;
ALTER TABLE orders ADD CONSTRAINT FK_order_item_id FOREIGN KEY (items_order_id) REFERENCES orders (order_id);
-- note: you can't DROP TABLE adter you add Foreign key constraint:
-- DROP TABLE orders; -- will be failed
-- before you need to alter table and drop created foreign key:
-- ALTER TABLE items DROP FOREIGN KEY FK_item_id;

-- INSERT
INSERT INTO items (item_id, item_name, item_color) VALUES 
(1, "scarf", "blue"),
(2, "scarf", "red"),
(3, "scarf", "yellow");

INSERT INTO orders (order_id, orders_customer_id, order_date, items_order_id) VALUES
(1, 2, '2019-08-20', 1),
(2, 2, '2019-08-20', 2),
(3, 2, '2019-08-20', 1),
(4, 2, '2019-08-20', 2),
(5, 2, '2019-08-20', 3);

SELECT * FROM orders;
-- but for table items, before you will add proper order_id to items_orders_id column, you will not see there relation to items:
SELECT * FROM items;
-- and you need to add this value to 
INSERT INTO items (item_id, item_name, item_color, items_orders_id) VALUES 
(1, "scarf", "blue", 1),
(1, "scarf", "blue", 3),
(2, "scarf", "red", 2),
(2, "scarf", "red", 4),
(3, "scarf", "yellow", 5); 
-- OOOPS! You can't store duplicated IDs!
-- solution is to not put items_orders_id to the table items, but create one more table that will store your many to many relationship:
ALTER TABLE items DROP COLUMN items_orders_id;
SELECT * FROM items;

CREATE TABLE items_orders (
	id INT,
    item_id INT NOT NULL,
    order_id INT NOT NULL,
    CONSTRAINT Pk_id PRIMARY KEY (id),
    CONSTRAINT FK_item_id FOREIGN KEY (item_id) REFERENCES items (item_id),
    CONSTRAINT FK_order_id FOREIGN KEY (order_id) REFERENCES orders (order_id)
);

INSERT INTO items_orders (id, item_id, order_id) VALUES 
(1, 1, 1),
(2, 1, 3),
(3, 2, 2),
(4, 2, 4),
(5, 3, 5);

SELECT * FROM items_orders;
-- now you finished to implement MANY-TO-MANY relationship!

-- one more thing - you can not only drop table, you also can DROP full database!!!
-- (it's very dangerous as you will lose all data, all schemas, everything! 0_0 )
-- DROP DATABASE relationship;

-- I also want to mention, that you can you INT instead of INTEGER and you don't need specify NOT NULL if column will be PRIMARY KEY

```