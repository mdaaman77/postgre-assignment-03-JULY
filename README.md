# postgre-assignment-03-JULY


# primary key - it is a unique and not null value attribute in the table , it is one of the composite key , it can act as foreign key in other table - eg - phone number , aadhar number , email-id etc.
 
 # foreign key - it is a refrence key of other table which is primary key in other table - it allow helps us to join the table tightly coupled 


# er diagram from eraser io 
index.html



# create user table


CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);


# create product table

CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10,2) NOT NULL CHECK (price >= 0),
    stock INT NOT NULL CHECK (stock >= 0),
    created_at TIMESTAMP DEFAULT NOW()
);


# create order item 

CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL,
    product_id BIGINT NOT NULL,
    quantity INT NOT NULL,
    price_at_purchase NUMERIC(10,2),
    status TEXT NOT NULL ,
    created_at TIMESTAMP DEFAULT NOW(),

    CONSTRAINT fk_user FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_product FOREIGN KEY (product_id)
        REFERENCES products(id)
        ON DELETE RESTRICT
);


# create table oder items

CREATE TABLE order_items (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL,
    order_id BIGINT NOT NULL,

    CONSTRAINT fk_user_oi FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE,

    CONSTRAINT fk_order_oi FOREIGN KEY (order_id)
        REFERENCES orders(id)
        ON DELETE CASCADE
);


# insert values into users
INSERT INTO users (email, name) VALUES
('Aaman@gmail.com', 'Aaman'),
('Saurabh@gmail.com', 'Saurabh'),
('Tanya@gmail.com', 'Tanya');

# insert values into products 
INSERT INTO products (name, price, stock) VALUES
('BRASSZIP ', 10000, 1000),
('YKK SLIDER', 1500, 5000),
('7 inch cfc ', 12000, 1500);

# insert values into orders 
INSERT INTO orders (user_id, product_id, quantity, price_at_purchase, status) VALUES
(1, 1, 1, 80000.00, 'SUCCESS'),   
(1, 2, 2, 1500.00, 'SUCCESS'),   
(2, 3, 1, 120000.00, 'PENDING'),
(3, 2, 1, 1500.00, 'PENDING');  

# insert values into order_item
INSERT INTO order_items (user_id, order_id) VALUES
(1, 1),
(1, 2),
(2, 3),
(3, 4);




# Write the following JOIN queries:


# List all orders with customer names.
SELECT 
    o.id AS order_id,
    u.name AS customer_name,
    p.name AS product_name,
    o.quantity,
    o.status,
    o.created_at
FROM orders o
JOIN users u ON o.user_id = u.id
JOIN products p ON o.product_id = p.id;




# List products in each order.
SELECT 
    oi.user_id,
    o.id AS order_id,
    p.name AS product_name,
    o.quantity
FROM order_items oi
JOIN orders o ON oi.order_id = o.id
JOIN products p ON o.product_id = p.id;




# Show the total number of orders placed by each customer.
SELECT 
    u.id,
    u.name,
    COUNT(o.id) AS total_orders
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id;



# error 
# as uuid & BIGINT is not supported for foreign key so i used BIGSERIAL for id  - given by pgadmin 