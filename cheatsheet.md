# 🧠 COMPLETE SQL REFERENCE (MySQL + PostgreSQL)

==================================================

1. DATABASE & SCHEMA MANAGEMENT (DDL)
   ==================================================

-- Create database
CREATE DATABASE mydb;

-- Drop database
DROP DATABASE mydb;

-- Select database (MySQL)
USE mydb;

-- PostgreSQL equivalent
SET search_path TO public;

-- Create schema (Postgres mainly)
CREATE SCHEMA myschema;

-- Drop schema
DROP SCHEMA myschema CASCADE;

---

## TABLES

-- Create table
CREATE TABLE users (
id INT PRIMARY KEY AUTO_INCREMENT, -- SERIAL in Postgres
name VARCHAR(100) NOT NULL,
email VARCHAR(100) UNIQUE,
age INT CHECK (age >= 18),
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- PostgreSQL version
CREATE TABLE users (
id SERIAL PRIMARY KEY,
name VARCHAR(100) NOT NULL,
email VARCHAR(100) UNIQUE,
age INT CHECK (age >= 18),
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Alter table
ALTER TABLE users ADD COLUMN phone VARCHAR(15);

-- Modify column
ALTER TABLE users MODIFY name VARCHAR(150); -- MySQL
ALTER TABLE users ALTER COLUMN name TYPE VARCHAR(150); -- Postgres

-- Rename column
ALTER TABLE users RENAME COLUMN name TO full_name;

-- Drop column
ALTER TABLE users DROP COLUMN phone;

-- Drop table
DROP TABLE users;

---

## CONSTRAINTS

ALTER TABLE users ADD CONSTRAINT pk_user PRIMARY KEY (id);
ALTER TABLE orders ADD CONSTRAINT fk_user FOREIGN KEY (user_id) REFERENCES users(id);
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);
ALTER TABLE users ADD CONSTRAINT chk_age CHECK (age >= 18);

==================================================
2. DATA MANIPULATION (DML)
==========================

-- Insert single row
INSERT INTO users (name, email, age) VALUES ('John', '[john@example.com](mailto:john@example.com)', 25);

-- Insert multiple rows
INSERT INTO users (name, email, age) VALUES
('Alice', '[alice@example.com](mailto:alice@example.com)', 30),
('Bob', '[bob@example.com](mailto:bob@example.com)', 22);

-- Update
UPDATE users SET age = 26 WHERE id = 1;

-- Delete
DELETE FROM users WHERE id = 1;

-- Truncate
TRUNCATE TABLE users;

---

## UPSERT

-- MySQL
INSERT INTO users (id, name) VALUES (1, 'John')
ON DUPLICATE KEY UPDATE name = 'Updated';

-- PostgreSQL
INSERT INTO users (id, name) VALUES (1, 'John')
ON CONFLICT (id) DO UPDATE SET name = 'Updated';

==================================================
3. DATA QUERYING (DQL)
======================

-- Select
SELECT * FROM users;

-- Where
SELECT * FROM users WHERE age > 18;

-- Order
SELECT * FROM users ORDER BY age DESC;

-- Limit
SELECT * FROM users LIMIT 10 OFFSET 5;

---

## JOINS

SELECT u.name, o.amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

LEFT JOIN orders o ON u.id = o.user_id;
RIGHT JOIN orders o ON u.id = o.user_id;

-- PostgreSQL FULL JOIN
FULL JOIN orders o ON u.id = o.user_id;

---

## AGGREGATION

SELECT COUNT(*), AVG(age), SUM(age)
FROM users
GROUP BY age
HAVING COUNT(*) > 1;

---

## SUBQUERY

SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders);

---

## CTE

WITH adult_users AS (
SELECT * FROM users WHERE age > 18
)
SELECT * FROM adult_users;

==================================================
4. ADVANCED FEATURES
====================

-- Window functions
SELECT name, ROW_NUMBER() OVER (ORDER BY age) FROM users;

SELECT name, RANK() OVER (ORDER BY age DESC) FROM users;

---

## SET OPERATIONS

SELECT name FROM users
UNION
SELECT name FROM admins;

SELECT name FROM users
INTERSECT
SELECT name FROM admins;

SELECT name FROM users
EXCEPT
SELECT name FROM admins;

==================================================
5. INDEXES
==========

CREATE INDEX idx_users_name ON users(name);
CREATE UNIQUE INDEX idx_email ON users(email);

-- PostgreSQL partial index
CREATE INDEX idx_active_users ON users(name) WHERE age > 18;

-- Full-text
-- MySQL
ALTER TABLE users ADD FULLTEXT(name);

-- PostgreSQL
CREATE INDEX idx_gin ON users USING GIN(to_tsvector('english', name));

==================================================
6. USER & PERMISSIONS
=====================

CREATE USER 'test' IDENTIFIED BY 'password'; -- MySQL
CREATE USER test WITH PASSWORD 'password'; -- Postgres

GRANT SELECT, INSERT ON users TO test;
REVOKE INSERT ON users FROM test;

==================================================
7. TRANSACTIONS
===============

BEGIN;
INSERT INTO users (name) VALUES ('Temp');
ROLLBACK;

BEGIN;
INSERT INTO users (name) VALUES ('Final');
COMMIT;

-- Savepoint
SAVEPOINT sp1;
ROLLBACK TO sp1;

==================================================
8. FUNCTIONS & PROCEDURES
=========================

-- MySQL procedure
DELIMITER //
CREATE PROCEDURE GetUsers()
BEGIN
SELECT * FROM users;
END //
DELIMITER ;

-- PostgreSQL function
CREATE FUNCTION get_users()
RETURNS TABLE(id INT, name TEXT)
AS $$
BEGIN
RETURN QUERY SELECT id, name FROM users;
END;
$$ LANGUAGE plpgsql;

---

## TRIGGERS

-- MySQL
CREATE TRIGGER before_insert_user
BEFORE INSERT ON users
FOR EACH ROW
SET NEW.created_at = NOW();

-- PostgreSQL
CREATE FUNCTION set_timestamp()
RETURNS TRIGGER AS $$
BEGIN
NEW.created_at = NOW();
RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_users
BEFORE INSERT ON users
FOR EACH ROW
EXECUTE FUNCTION set_timestamp();

==================================================
9. VIEWS
========

CREATE VIEW adult_users AS
SELECT * FROM users WHERE age > 18;

-- PostgreSQL materialized view
CREATE MATERIALIZED VIEW mat_users AS
SELECT * FROM users;

==================================================
10. DATA TYPES
==============

-- Numeric
INT, BIGINT, DECIMAL

-- String
VARCHAR, TEXT

-- Date
DATE, TIMESTAMP

-- Boolean
BOOLEAN

-- JSON
-- MySQL
JSON

-- PostgreSQL
JSONB

-- Array (Postgres only)
INT[]

==================================================
11. UTILITIES
=============

-- MySQL
SHOW TABLES;
DESCRIBE users;

-- PostgreSQL
\dt
\d users

==================================================
12. PERFORMANCE & ANALYSIS
==========================

-- Explain query
EXPLAIN SELECT * FROM users;

-- PostgreSQL analyze
EXPLAIN ANALYZE SELECT * FROM users;

==================================================
13. LOCKING & CONCURRENCY
=========================

SELECT * FROM users FOR UPDATE;
SELECT * FROM users FOR SHARE;

==================================================
14. PARTITIONING
================

-- PostgreSQL
CREATE TABLE users_partitioned (
id INT,
created_at DATE
) PARTITION BY RANGE (created_at);

==================================================
15. EXTENSIONS (PostgreSQL)
===========================

CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

==================================================
END OF COMPLETE SQL REFERENCE
=============================
