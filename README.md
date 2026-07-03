# postgre-assignment-03-JULY

# create user table

# as uuid is not supported for foreign key so used BIGSERIAL for id 
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
    quantity INT NOT NULL CHECK,
    price_at_purchase NUMERIC(10,2),
    status TEXT NOT NULL CHECK,
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