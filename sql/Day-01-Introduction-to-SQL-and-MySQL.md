# MySQL/SQL - Complete Guide

## 📖 What is SQL?

SQL is the standard language for dealing with **Relational Databases**.

SQL is used to **insert**, **search**, **update**, and **delete** database records.

---

## 🚀 How to Use SQL

The following SQL statement selects all the records in the "Customers" table:

```sql
SELECT * FROM Customers;
```

---

## 📋 Important Rules

### Keep in Mind That...

* **SQL keywords are NOT case sensitive**: `select` is the same as `SELECT`
* In this guide, we write all SQL keywords in **UPPER-CASE**

### Semicolon after SQL Statements

* Some database systems require a semicolon at the end of each SQL statement
* Semicolon is the standard way to separate each SQL statement
* We will use semicolon at the end of each SQL statement

---

## 🔑 Most Important SQL Commands

| Command | Description |
|---------|-------------|
| `SELECT` | Extracts data from a database |
| `UPDATE` | Updates data in a database |
| `DELETE` | Deletes data from a database |
| `INSERT INTO` | Inserts new data into a database |
| `CREATE DATABASE` | Creates a new database |
| `ALTER DATABASE` | Modifies a database |
| `CREATE TABLE` | Creates a new table |
| `ALTER TABLE` | Modifies a table |
| `DROP TABLE` | Deletes a table |
| `CREATE INDEX` | Creates an index (search key) |
| `DROP INDEX` | Deletes an index |

---

## 📊 SELECT - Extract Data

```sql
-- Select all columns
SELECT * FROM Customers;

-- Select specific columns
SELECT CustomerName, City FROM Customers;

-- Select with condition
SELECT * FROM Customers WHERE Country = 'USA';
```

---

## ✏️ UPDATE - Update Data

```sql
-- Update single record
UPDATE Customers
SET ContactName = 'John Doe'
WHERE CustomerID = 1;

-- Update multiple records
UPDATE Customers
SET Country = 'USA'
WHERE City = 'New York';
```

---

## 🗑️ DELETE - Delete Data

```sql
-- Delete specific record
DELETE FROM Customers
WHERE CustomerID = 1;

-- Delete all records (use carefully!)
DELETE FROM Customers;
```

---

## ➕ INSERT INTO - Insert Data

```sql
-- Insert with all columns
INSERT INTO Customers (CustomerName, ContactName, City, Country)
VALUES ('Cardinal', 'Tom Erichsen', 'Stavanger', 'Norway');

-- Insert multiple rows
INSERT INTO Customers (CustomerName, City, Country)
VALUES 
    ('Customer 1', 'London', 'UK'),
    ('Customer 2', 'Paris', 'France'),
    ('Customer 3', 'Berlin', 'Germany');
```

---

## 🏗️ CREATE DATABASE - Create Database

```sql
-- Create new database
CREATE DATABASE my_database;

-- Create if not exists
CREATE DATABASE IF NOT EXISTS my_database;
```

---

## 🔧 ALTER DATABASE - Modify Database

```sql
-- Modify database character set
ALTER DATABASE my_database
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

---

## 📋 CREATE TABLE - Create Table

```sql
-- Create basic table
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY AUTO_INCREMENT,
    CustomerName VARCHAR(100) NOT NULL,
    ContactName VARCHAR(100),
    City VARCHAR(50),
    Country VARCHAR(50)
);

-- Create table with constraints
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT,
    OrderDate DATE NOT NULL,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

---

## 🛠️ ALTER TABLE - Modify Table

```sql
-- Add column
ALTER TABLE Customers
ADD Email VARCHAR(100);

-- Modify column
ALTER TABLE Customers
MODIFY COLUMN City VARCHAR(100);

-- Drop column
ALTER TABLE Customers
DROP COLUMN Email;

-- Rename table
ALTER TABLE Customers
RENAME TO Clients;
```

---

## 🗑️ DROP TABLE - Delete Table

```sql
-- Delete table
DROP TABLE Customers;

-- Delete if exists
DROP TABLE IF EXISTS Customers;
```

---

## 🔍 CREATE INDEX - Create Index

```sql
-- Create index on single column
CREATE INDEX idx_city
ON Customers (City);

-- Create index on multiple columns
CREATE INDEX idx_name_city
ON Customers (CustomerName, City);

-- Create unique index
CREATE UNIQUE INDEX idx_email
ON Customers (Email);
```

---

## ❌ DROP INDEX - Delete Index

```sql
-- Drop index
DROP INDEX idx_city ON Customers;

-- Alternative syntax
ALTER TABLE Customers
DROP INDEX idx_city;
```

---

## 📝 Additional Examples

### Complete CRUD Example

```sql
-- CREATE DATABASE
CREATE DATABASE shop_db;
USE shop_db;

-- CREATE TABLE
CREATE TABLE Products (
    ProductID INT PRIMARY KEY AUTO_INCREMENT,
    ProductName VARCHAR(100) NOT NULL,
    Price DECIMAL(10, 2),
    Stock INT DEFAULT 0
);

-- INSERT DATA
INSERT INTO Products (ProductName, Price, Stock)
VALUES 
    ('Laptop', 999.99, 50),
    ('Mouse', 29.99, 200),
    ('Keyboard', 79.99, 150);

-- SELECT DATA
SELECT * FROM Products;

-- UPDATE DATA
UPDATE Products
SET Price = 899.99
WHERE ProductName = 'Laptop';

-- DELETE DATA
DELETE FROM Products
WHERE Stock = 0;

-- DROP TABLE
DROP TABLE Products;

-- DROP DATABASE
DROP DATABASE shop_db;
```

---

## 🎯 Quick Reference

```sql
-- Database Operations
CREATE DATABASE database_name;
ALTER DATABASE database_name;
DROP DATABASE database_name;

-- Table Operations
CREATE TABLE table_name (column1 datatype, column2 datatype);
ALTER TABLE table_name ADD column_name datatype;
DROP TABLE table_name;

-- Data Operations
INSERT INTO table_name (col1, col2) VALUES (val1, val2);
SELECT * FROM table_name;
UPDATE table_name SET col1 = val1 WHERE condition;
DELETE FROM table_name WHERE condition;

-- Index Operations
CREATE INDEX index_name ON table_name (column);
DROP INDEX index_name ON table_name;
```

---

## ✅ Best Practices

1. **Always use semicolons** to end SQL statements
2. **Write keywords in UPPERCASE** for better readability
3. **Use meaningful names** for tables and columns
4. **Always backup** before DROP or DELETE operations
5. **Use WHERE clause** carefully with UPDATE and DELETE
6. **Test queries** on a copy before running on production

---
