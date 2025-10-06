# Day 2: MySQL Data Types, Constraints & Filtering Data

## Table of Contents
1. [MySQL Data Types in Depth](#mysql-data-types)
2. [Understanding Constraints](#understanding-constraints)
3. [Foreign Keys & Relationships](#foreign-keys)
4. [Indexes Explained](#indexes-explained)
5. [WHERE Clause & Filtering](#where-clause)
6. [Pattern Matching with LIKE](#like-operator)
7. [Working with NULL Values](#null-values)
8. [IN and BETWEEN Operators](#in-and-between)
9. [Practice Exercises](#practice-exercises)
10. [Summary](#summary)

---

## MySQL Data Types in Depth

Understanding data types is crucial for efficient database design and optimal performance.

### 1. Numeric Data Types

#### Integer Types
```sql
-- TINYINT: -128 to 127 (1 byte)
CREATE TABLE age_table (
    age TINYINT UNSIGNED  -- 0 to 255, perfect for human ages
);

-- SMALLINT: -32,768 to 32,767 (2 bytes)
CREATE TABLE year_table (
    year SMALLINT  -- Good for years
);

-- MEDIUMINT: -8,388,608 to 8,388,607 (3 bytes)
CREATE TABLE population (
    city_population MEDIUMINT
);

-- INT: -2,147,483,648 to 2,147,483,647 (4 bytes)
CREATE TABLE products (
    product_id INT,
    stock_quantity INT
);

-- BIGINT: Very large numbers (8 bytes)
CREATE TABLE analytics (
    view_count BIGINT
);
```

**Comparison Table:**
| Type | Storage | Signed Range | Unsigned Range | Use Case |
|------|---------|--------------|----------------|----------|
| TINYINT | 1 byte | -128 to 127 | 0 to 255 | Age, small counts |
| SMALLINT | 2 bytes | -32K to 32K | 0 to 65K | Year, small IDs |
| MEDIUMINT | 3 bytes | -8M to 8M | 0 to 16M | Population |
| INT | 4 bytes | -2B to 2B | 0 to 4B | Primary keys, quantities |
| BIGINT | 8 bytes | Very large | Very large | Analytics, big data |

#### Decimal Types
```sql
-- DECIMAL(precision, scale) - Exact values
CREATE TABLE financial (
    price DECIMAL(10, 2),      -- 10 digits total, 2 after decimal
    tax_rate DECIMAL(5, 4)     -- e.g., 0.0825 (8.25%)
);
-- Example: 12345678.90

-- FLOAT - Approximate (4 bytes)
CREATE TABLE measurements (
    weight FLOAT  -- Less precise, uses less space
);

-- DOUBLE - More precise approximate (8 bytes)
CREATE TABLE scientific (
    calculation DOUBLE
);
```

**When to Use What:**
- **DECIMAL**: Money, prices, exact calculations
- **FLOAT/DOUBLE**: Scientific calculations, approximate values
- **Rule**: Use DECIMAL for financial data!

### 2. String Data Types

```sql
-- CHAR(n) - Fixed length
CREATE TABLE country_codes (
    code CHAR(2)  -- Always 2 chars: 'US', 'IN', 'UK'
);
-- Pads with spaces if shorter, wastes space if varied length

-- VARCHAR(n) - Variable length
CREATE TABLE users (
    username VARCHAR(50),      -- Max 50 chars, uses only what's needed
    email VARCHAR(100),
    bio VARCHAR(500)
);

-- TEXT types - Large text
CREATE TABLE articles (
    title VARCHAR(200),
    content TEXT,              -- Up to 65,535 characters
    long_content MEDIUMTEXT,   -- Up to 16MB
    book_content LONGTEXT      -- Up to 4GB
);

-- ENUM - Predefined values
CREATE TABLE orders (
    status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled')
);
-- Efficient storage, only allows listed values
```

**String Type Comparison:**
| Type | Max Length | Storage | Use Case |
|------|-----------|---------|----------|
| CHAR(n) | 255 | Fixed | Country codes, fixed IDs |
| VARCHAR(n) | 65,535 | Variable | Names, emails, URLs |
| TEXT | 65,535 | Variable | Articles, descriptions |
| MEDIUMTEXT | 16MB | Variable | Books, large documents |
| LONGTEXT | 4GB | Variable | Huge documents |
| ENUM | 65,535 values | 1-2 bytes | Status, categories |

### 3. Date and Time Types

```sql
CREATE TABLE events (
    -- DATE: 'YYYY-MM-DD'
    event_date DATE,                    -- '2025-10-04'
    
    -- TIME: 'HH:MM:SS'
    event_time TIME,                    -- '14:30:00'
    
    -- DATETIME: 'YYYY-MM-DD HH:MM:SS'
    created_at DATETIME,                -- '2025-10-04 14:30:00'
    
    -- TIMESTAMP: Auto-updating timestamp
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP 
               ON UPDATE CURRENT_TIMESTAMP,
    
    -- YEAR: 'YYYY'
    birth_year YEAR                     -- 1995, 2000, etc.
);
```

**Date Type Comparison:**
| Type | Format | Range | Use Case |
|------|--------|-------|----------|
| DATE | YYYY-MM-DD | 1000-9999 | Birthdays, deadlines |
| TIME | HH:MM:SS | -838:59:59 to 838:59:59 | Duration, schedules |
| DATETIME | YYYY-MM-DD HH:MM:SS | 1000 to 9999 | Event timestamps |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1970 to 2038 | Auto-tracking changes |
| YEAR | YYYY | 1901 to 2155 | Birth year, model year |

### 4. Boolean Type

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50),
    is_active BOOLEAN,           -- Stored as TINYINT(1): 0=false, 1=true
    is_verified BOOL,            -- Synonym for BOOLEAN
    email_notifications TINYINT(1) DEFAULT 1
);

-- Usage
INSERT INTO users (user_id, username, is_active, is_verified)
VALUES (1, 'john_doe', TRUE, 1);

-- Both work the same
INSERT INTO users (user_id, username, is_active, is_verified)
VALUES (2, 'jane_smith', 1, TRUE);
```

### 5. Binary Data Types

```sql
CREATE TABLE files (
    file_id INT PRIMARY KEY,
    file_name VARCHAR(255),
    file_data BLOB,              -- Binary Large Object up to 65KB
    large_file MEDIUMBLOB,       -- Up to 16MB
    video_file LONGBLOB          -- Up to 4GB
);

-- For storing: images, PDFs, videos, files
```

---

## Understanding Constraints

Constraints enforce rules on data to maintain accuracy and reliability.

### 1. NOT NULL Constraint

```sql
CREATE TABLE employees (
    emp_id INT NOT NULL,
    first_name VARCHAR(50) NOT NULL,    -- Must provide value
    last_name VARCHAR(50) NOT NULL,
    middle_name VARCHAR(50),            -- Can be NULL
    email VARCHAR(100) NOT NULL
);

-- This will fail
INSERT INTO employees (emp_id, last_name, email)
VALUES (1, 'Doe', 'john@email.com');
-- Error: first_name cannot be NULL

-- This works
INSERT INTO employees (emp_id, first_name, last_name, email)
VALUES (1, 'John', 'Doe', 'john@email.com');
```

### 2. UNIQUE Constraint

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,        -- No duplicates allowed
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(15) UNIQUE
);

-- First insert works
INSERT INTO users VALUES (1, 'john_doe', 'john@email.com', '1234567890');

-- This fails - duplicate username
INSERT INTO users VALUES (2, 'john_doe', 'jane@email.com', '9876543210');
-- Error: Duplicate entry 'john_doe'

-- Multiple column UNIQUE constraint
CREATE TABLE registrations (
    user_id INT,
    course_id INT,
    enrollment_date DATE,
    UNIQUE(user_id, course_id)  -- Combination must be unique
);
```

### 3. DEFAULT Constraint

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INT DEFAULT 0,           -- Default to 0 if not specified
    status VARCHAR(20) DEFAULT 'active',    -- Default to 'active'
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Don't specify stock_quantity or status
INSERT INTO products (product_id, product_name, price)
VALUES (1, 'Laptop', 999.99);

SELECT * FROM products;
-- stock_quantity will be 0, status will be 'active'
```

### 4. CHECK Constraint

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18 AND age <= 65),        -- Must be 18-65
    salary DECIMAL(10,2) CHECK (salary > 0),        -- Must be positive
    email VARCHAR(100) CHECK (email LIKE '%@%'),    -- Must contain @
    gender ENUM('M', 'F', 'Other') NOT NULL
);

-- This fails - age is 15
INSERT INTO employees VALUES (1, 'John', 15, 50000, 'john@email.com', 'M');
-- Error: Check constraint violated

-- This works
INSERT INTO employees VALUES (1, 'John', 25, 50000, 'john@email.com', 'M');

-- Named CHECK constraint
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    price DECIMAL(10,2),
    discount_price DECIMAL(10,2),
    CONSTRAINT price_check CHECK (discount_price < price)
);
```

### 5. AUTO_INCREMENT

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,  -- Starts at 1, increments by 1
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(100)
);

-- Don't specify customer_id
INSERT INTO customers (customer_name, email)
VALUES 
    ('Alice Johnson', 'alice@email.com'),    -- Gets ID 1
    ('Bob Smith', 'bob@email.com'),          -- Gets ID 2
    ('Carol White', 'carol@email.com');      -- Gets ID 3

-- Start AUTO_INCREMENT at specific value
ALTER TABLE customers AUTO_INCREMENT = 1000;
```

---

## Foreign Keys & Relationships

Foreign keys create relationships between tables and maintain referential integrity.

### Understanding Foreign Keys

```sql
-- Parent table
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL,
    location VARCHAR(100)
);

-- Child table with foreign key
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100) NOT NULL,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);

-- Insert departments first
INSERT INTO departments VALUES 
    (1, 'Engineering', 'Building A'),
    (2, 'Marketing', 'Building B'),
    (3, 'HR', 'Building C');

-- Now insert employees
INSERT INTO employees VALUES (101, 'John Doe', 1);      -- Works (dept 1 exists)
INSERT INTO employees VALUES (102, 'Jane Smith', 2);    -- Works (dept 2 exists)

-- This fails - department 99 doesn't exist
INSERT INTO employees VALUES (103, 'Bob Wilson', 99);
-- Error: Cannot add or update a child row: foreign key constraint fails
```

### Foreign Key Actions

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    
    -- ON DELETE CASCADE: Delete orders when customer is deleted
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

CREATE TABLE order_items (
    item_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    
    -- ON DELETE SET NULL: Set to NULL when order is deleted
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE,
    
    -- ON DELETE RESTRICT: Prevent deletion if items exist
    FOREIGN KEY (product_id) REFERENCES products(product_id)
        ON DELETE RESTRICT
);
```

**Foreign Key Actions Explained:**
| Action | What Happens |
|--------|--------------|
| CASCADE | Deletes/updates child rows when parent is deleted/updated |
| SET NULL | Sets foreign key to NULL when parent is deleted |
| RESTRICT | Prevents deletion/update of parent if children exist |
| NO ACTION | Same as RESTRICT |
| SET DEFAULT | Sets to default value (not fully supported in MySQL) |

### Practical Example: Complete Schema

```sql
-- School Management System

-- 1. Students table
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    enrollment_date DATE DEFAULT (CURRENT_DATE),
    age INT CHECK (age >= 16)
);

-- 2. Courses table
CREATE TABLE courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    credits INT CHECK (credits > 0),
    max_students INT DEFAULT 30
);

-- 3. Enrollments table (many-to-many relationship)
CREATE TABLE enrollments (
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    enrollment_date DATE DEFAULT (CURRENT_DATE),
    grade CHAR(2) CHECK (grade IN ('A', 'B', 'C', 'D', 'F')),
    
    FOREIGN KEY (student_id) REFERENCES students(student_id)
        ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
        ON DELETE CASCADE,
    
    UNIQUE(student_id, course_id)  -- Student can't enroll in same course twice
);
```

---

## Indexes Explained

Indexes speed up data retrieval but slow down INSERT/UPDATE operations.

### What is an Index?

Think of an index like a book's index - it helps you find information quickly without reading every page.

```sql
-- Create table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,        -- PRIMARY KEY automatically creates index
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);

-- Create index on last_name for faster searches
CREATE INDEX idx_last_name ON employees(last_name);

-- Create index on multiple columns (composite index)
CREATE INDEX idx_name ON employees(first_name, last_name);

-- Create unique index
CREATE UNIQUE INDEX idx_email ON employees(email);

-- View indexes
SHOW INDEX FROM employees;

-- Drop index
DROP INDEX idx_last_name ON employees;
```

### When to Use Indexes

✅ **Use indexes when:**
- Column is frequently used in WHERE clauses
- Column is used in JOIN conditions
- Column is used for sorting (ORDER BY)
- Column has high cardinality (many unique values)

❌ **Avoid indexes when:**
- Table is small (< 1000 rows)
- Column has few unique values (like gender: M/F)
- Table has frequent INSERT/UPDATE operations
- Column is rarely searched

```sql
-- Good index candidates
CREATE INDEX idx_email ON users(email);              -- Searched often
CREATE INDEX idx_order_date ON orders(order_date);   -- Used for filtering
CREATE INDEX idx_status ON orders(status);           -- Used in WHERE

-- Bad index candidates
CREATE INDEX idx_gender ON users(gender);            -- Only 2-3 values
CREATE INDEX idx_is_active ON users(is_active);      -- Boolean (0/1)
```

---

## WHERE Clause & Filtering

The WHERE clause filters rows based on conditions.

### Basic WHERE Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

### Comparison Operators

```sql
-- Sample data setup
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE,
    is_active BOOLEAN
);

INSERT INTO employees VALUES
    (1, 'John Doe', 'Engineering', 75000, '2022-01-15', 1),
    (2, 'Jane Smith', 'Marketing', 65000, '2022-03-20', 1),
    (3, 'Mike Johnson', 'Engineering', 80000, '2021-11-10', 1),
    (4, 'Sarah Williams', 'HR', 60000, '2023-01-05', 1),
    (5, 'Tom Brown', 'Sales', 70000, '2022-08-22', 0);

-- Equal to (=)
SELECT * FROM employees WHERE department = 'Engineering';

-- Not equal to (<> or !=)
SELECT * FROM employees WHERE department != 'Sales';
SELECT * FROM employees WHERE department <> 'Sales';

-- Greater than (>)
SELECT * FROM employees WHERE salary > 70000;

-- Less than (<)
SELECT * FROM employees WHERE salary < 70000;

-- Greater than or equal (>=)
SELECT * FROM employees WHERE salary >= 70000;

-- Less than or equal (<=)
SELECT * FROM employees WHERE salary <= 70000;
```

### Logical Operators

```sql
-- AND: All conditions must be true
SELECT * FROM employees 
WHERE department = 'Engineering' AND salary > 70000;

-- OR: At least one condition must be true
SELECT * FROM employees 
WHERE department = 'Engineering' OR department = 'Marketing';

-- NOT: Negates a condition
SELECT * FROM employees 
WHERE NOT department = 'Sales';

-- Combining multiple conditions
SELECT * FROM employees 
WHERE (department = 'Engineering' OR department = 'Marketing')
  AND salary > 65000
  AND is_active = 1;
```

---

## LIKE Operator - Pattern Matching

LIKE is used for pattern matching with wildcards.

### Wildcards
- `%` : Matches zero or more characters
- `_` : Matches exactly one character

```sql
-- Sample data
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    city VARCHAR(50)
);

INSERT INTO customers VALUES
    (1, 'John Doe', 'john.doe@gmail.com', 'New York'),
    (2, 'Jane Smith', 'jane.smith@yahoo.com', 'Los Angeles'),
    (3, 'Mike Johnson', 'mike.j@gmail.com', 'New York'),
    (4, 'Sarah Williams', 'sarah.w@outlook.com', 'Chicago'),
    (5, 'Robert Brown', 'rob.brown@gmail.com', 'New Jersey');

-- Names starting with 'J'
SELECT * FROM customers WHERE name LIKE 'J%';
-- Result: John Doe, Jane Smith

-- Names ending with 'son'
SELECT * FROM customers WHERE name LIKE '%son';
-- Result: Mike Johnson

-- Names containing 'oh'
SELECT * FROM customers WHERE name LIKE '%oh%';
-- Result: John Doe

-- Emails with gmail
SELECT * FROM customers WHERE email LIKE '%@gmail.com';
-- Result: john.doe@gmail.com, mike.j@gmail.com, rob.brown@gmail.com

-- Cities starting with 'New'
SELECT * FROM customers WHERE city LIKE 'New%';
-- Result: New York, New Jersey

-- Names with exactly 8 characters starting with 'J' (J + 7 more)
SELECT * FROM customers WHERE name LIKE 'J_______';

-- Names with 'o' as second character
SELECT * FROM customers WHERE name LIKE '_o%';
-- Result: John Doe, Robert Brown

-- Case-insensitive by default in MySQL
SELECT * FROM customers WHERE name LIKE 'john%';  -- Matches 'John Doe'

-- Case-sensitive search using BINARY
SELECT * FROM customers WHERE BINARY name LIKE 'john%';  -- No match
SELECT * FROM customers WHERE BINARY name LIKE 'John%';  -- Matches 'John Doe'

-- NOT LIKE: Find names NOT starting with 'J'
SELECT * FROM customers WHERE name NOT LIKE 'J%';
```

### Practical LIKE Examples

```sql
-- Find phone numbers with specific pattern
SELECT * FROM customers WHERE phone LIKE '555-%';

-- Find zip codes in specific range
SELECT * FROM addresses WHERE zip_code LIKE '900__';  -- 90000-90099

-- Find products with specific codes
SELECT * FROM products WHERE product_code LIKE 'ELEC-%';

-- Search in multiple columns
SELECT * FROM employees 
WHERE first_name LIKE '%ann%' OR last_name LIKE '%ann%';
```

---

## NULL Values

NULL represents missing or unknown data (not zero, not empty string).

### IS NULL / IS NOT NULL

```sql
-- Sample data
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    manager_id INT
);

INSERT INTO employees VALUES
    (1, 'John Doe', 'john@email.com', '1234567890', NULL),     -- No manager (CEO)
    (2, 'Jane Smith', 'jane@email.com', NULL, 1),              -- No phone
    (3, 'Mike Johnson', NULL, '5555555555', 1),                -- No email
    (4, 'Sarah Williams', 'sarah@email.com', '9876543210', 2);

-- Find employees without phone
SELECT * FROM employees WHERE phone IS NULL;
-- Result: Jane Smith

-- Find employees with phone
SELECT * FROM employees WHERE phone IS NOT NULL;
-- Result: John Doe, Mike Johnson, Sarah Williams

-- Find employees without manager (top-level)
SELECT * FROM employees WHERE manager_id IS NULL;
-- Result: John Doe

-- WRONG way (doesn't work with NULL)
SELECT * FROM employees WHERE phone = NULL;     -- Returns nothing!
SELECT * FROM employees WHERE phone != NULL;    -- Returns nothing!

-- Combining with other conditions
SELECT * FROM employees 
WHERE manager_id IS NOT NULL AND email IS NOT NULL;
-- Result: Jane Smith, Sarah Williams
```

### Working with NULL in Calculations

```sql
-- NULL in arithmetic returns NULL
SELECT 5 + NULL;        -- Result: NULL
SELECT 10 * NULL;       -- Result: NULL

-- Using COALESCE to handle NULL
SELECT 
    name,
    phone,
    COALESCE(phone, 'No phone provided') AS phone_display
FROM employees;

-- Using IFNULL (MySQL specific)
SELECT 
    name,
    IFNULL(email, 'No email') AS email_display
FROM employees;

-- Using NULLIF
SELECT NULLIF(column1, column2);  -- Returns NULL if both are equal
```

---

## IN and BETWEEN Operators

### IN Operator

```sql
-- IN: Check if value matches any in a list
SELECT * FROM employees 
WHERE department IN ('Engineering', 'Marketing', 'Sales');

-- Equivalent to multiple OR conditions
SELECT * FROM employees 
WHERE department = 'Engineering' 
   OR department = 'Marketing' 
   OR department = 'Sales';

-- NOT IN: Exclude values
SELECT * FROM employees 
WHERE department NOT IN ('HR', 'Admin');

-- IN with numbers
SELECT * FROM products 
WHERE product_id IN (1, 5, 10, 15, 20);

-- IN with subquery (preview of later topics)
SELECT * FROM employees 
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Building A');
```

### BETWEEN Operator

```sql
-- BETWEEN: Check if value is in a range (inclusive)
SELECT * FROM employees 
WHERE salary BETWEEN 60000 AND 75000;

-- Equivalent to:
SELECT * FROM employees 
WHERE salary >= 60000 AND salary <= 75000;

-- NOT BETWEEN
SELECT * FROM employees 
WHERE salary NOT BETWEEN 60000 AND 75000;

-- BETWEEN with dates
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- BETWEEN with strings (alphabetical)
SELECT * FROM customers 
WHERE name BETWEEN 'A' AND 'M';  -- Names starting with A to M
```

### Combining Operators

```sql
-- Complex filtering
SELECT * FROM employees 
WHERE department IN ('Engineering', 'Marketing')
  AND salary BETWEEN 65000 AND 80000
  AND hire_date >= '2022-01-01'
  AND email IS NOT NULL
  AND name LIKE 'J%';

-- Using parentheses for complex logic
SELECT * FROM products 
WHERE (category = 'Electronics' AND price < 500)
   OR (category = 'Books' AND stock_quantity > 100);
```

---

## Practice Exercises

### Exercise 1: Create Table with All Constraints
Create a table `products` with the following requirements:
- product_id: Auto-incrementing primary key
- product_name: Required, max 100 characters
- category: ENUM of 'Electronics', 'Clothing', 'Food', 'Books'
- price: Decimal, must be positive
- stock_quantity: Integer, default 0, must be >= 0
- discount_price: Decimal, must be less than price
- sku: Unique, 20 characters
- is_active: Boolean, default true
- created_at: Timestamp with default current time

<details>
<summary>Solution</summary>

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(100) NOT NULL,
    category ENUM('Electronics', 'Clothing', 'Food', 'Books') NOT NULL,
    price DECIMAL(10,2) CHECK (price > 0),
    stock_quantity INT DEFAULT 0 CHECK (stock_quantity >= 0),
    discount_price DECIMAL(10,2),
    sku VARCHAR(20) UNIQUE NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT price_discount_check CHECK (discount_price < price OR discount_price IS NULL)
);
```
</details>

### Exercise 2: Foreign Key Relationships
Create tables for a library system:
- `authors` (author_id, name, country)
- `books` (book_id, title, author_id, published_year, isbn)
- Ensure referential integrity with foreign keys

<details>
<summary>Solution</summary>

```sql
CREATE TABLE authors (
    author_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    country VARCHAR(50)
);

CREATE TABLE books (
    book_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(200) NOT NULL,
    author_id INT NOT NULL,
    published_year YEAR,
    isbn VARCHAR(20) UNIQUE,
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);
```
</details>

### Exercise 3: Complex WHERE Queries

Given this table:
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE,
    email VARCHAR(100),
    manager_id INT
);
```

Write queries for:
1. Find all employees in Engineering or Marketing with salary > 60000
2. Find employees hired in 2023
3. Find employees whose name starts with 'A' and ends with 'n'
4. Find employees without a manager (manager_id IS NULL)
5. Find employees with gmail email addresses

<details>
<summary>Solution</summary>

```sql
-- 1. Engineering or Marketing with salary > 60000
SELECT * FROM employees 
WHERE department IN ('Engineering', 'Marketing') 
  AND salary > 60000;

-- 2. Hired in 2023
SELECT * FROM employees 
WHERE hire_date BETWEEN '2023-01-01' AND '2023-12-31';
-- or
SELECT * FROM employees 
WHERE YEAR(hire_date) = 2023;

-- 3. Name starts with 'A' and ends with 'n'
SELECT * FROM employees 
WHERE name LIKE 'A%n';

-- 4. Without manager
SELECT * FROM employees 
WHERE manager_id IS NULL;

-- 5. Gmail addresses
SELECT * FROM employees 
WHERE email LIKE '%@gmail.com';
```
</details>

### Exercise 4: Data Type Selection
Choose appropriate data types for these columns:
1. Person's age
2. Product price (up to millions with 2 decimals)
3. Blog post content
4. Country code (2 letters, always)
5. Order status (pending, processing, shipped, delivered)
6. Phone number (international format)
7. Is account verified (yes/no)
8. Number of page views (can be billions)

<details>
<summary>Solution</summary>

```sql
CREATE TABLE example (
    age TINYINT UNSIGNED,                                    -- 0-255
    price DECIMAL(12,2),                                     -- 9999999999.99
    content TEXT,                                            -- Up to 65KB
    country_code CHAR(2),                                    -- Fixed 2 chars
    status ENUM('pending','processing','shipped','delivered'),
    phone VARCHAR(20),                                       -- Variable length
    is_verified BOOLEAN,                                     -- TRUE/FALSE
    page_views BIGINT UNSIGNED                               -- Very large numbers
);
```
</details>

### Exercise 5: Create Indexes
For the employees table, create appropriate indexes for:
1. Frequently searching by email
2. Often filtering by department and salary together
3. Regular searches by last name

<details>
<summary>Solution</summary>

```sql
-- 1. Email searches (also ensures uniqueness)
CREATE UNIQUE INDEX idx_email ON employees(email);

-- 2. Department and salary filtering
CREATE INDEX idx_dept_salary ON employees(department, salary);

-- 3. Last name searches
CREATE INDEX idx_last_name ON employees(last_name);
```
</details>

---

## Summary

### What You Learned Today:

✅ **Data Types Mastery**
- Numeric types: INT, DECIMAL, FLOAT, DOUBLE
- String types: CHAR, VARCHAR, TEXT, ENUM
- Date/Time types: DATE, TIME, DATETIME, TIMESTAMP
- Boolean and Binary types

✅ **Constraints**
- NOT NULL, UNIQUE, DEFAULT
- CHECK constraints
- AUTO_INCREMENT

✅ **Foreign Keys**
- Creating relationships between tables
- ON DELETE and ON UPDATE actions
- Referential integrity

✅ **Indexes**
- What indexes are and why they matter
- When to use/avoid indexes
- Creating single and composite indexes

✅ **WHERE Clause & Filtering**
- Comparison operators (=, !=, >, <, >=, <=)
- Logical operators (AND, OR, NOT)
- Pattern matching with LIKE (%, _)
- NULL handling (IS NULL, IS NOT NULL)
- IN and BETWEEN operators

### Commands Learned Today:
```sql
-- Data Types
INT, TINYINT, BIGINT, DECIMAL(p,s), FLOAT, DOUBLE
CHAR(n), VARCHAR(n), TEXT, ENUM()
DATE, TIME, DATETIME, TIMESTAMP, YEAR
BOOLEAN, BLOB

-- Constraints
NOT NULL, UNIQUE, DEFAULT, CHECK
AUTO_INCREMENT
PRIMARY KEY, FOREIGN KEY

-- Indexes
CREATE INDEX idx_name ON table(column);
CREATE UNIQUE INDEX idx_name ON table(column);
SHOW INDEX FROM table;
DROP INDEX idx_name ON table;

-- Filtering
WHERE column = value
WHERE column IN (val1, val2, val3)
WHERE column BETWEEN val1 AND val2
WHERE column LIKE 'pattern'
WHERE column IS NULL
WHERE condition1 AND condition2
WHERE condition1 OR condition2
```

### Key Takeaways:
1. **Choose the right data type** - saves space and improves performance
2. **DECIMAL for money** - never use FLOAT for financial data
3. **NOT NULL for required fields** - prevents incomplete data
4. **Foreign keys maintain integrity** - prevent orphaned records
5. **Indexes speed up reads** but slow down writes
6. **NULL is not zero** - use IS NULL, not = NULL
7. **LIKE is case-insensitive** in MySQL by default
8. **IN is cleaner than multiple ORs**
9. **BETWEEN is inclusive** - includes both boundary values

### Common Mistakes to Avoid:
❌ Using VARCHAR for fixed-length data (use CHAR)
❌ Using FLOAT/DOUBLE for money (use DECIMAL)
❌ Forgetting NOT NULL on important fields
❌ Creating too many indexes (slows INSERT/UPDATE)
❌ Using = NULL instead of IS NULL
❌ Not using foreign keys for relationships
❌ Using LIKE without wildcards (use = instead)
❌ Forgetting BETWEEN is inclusive

---

## Next Steps (Day 3 Preview)

Tomorrow you'll learn:
- **ORDER BY** - Sorting results (ASC/DESC)
- **GROUP BY** - Grouping data for aggregation
- **Aggregate Functions** - COUNT(), SUM(), AVG(), MIN(), MAX()
- **HAVING** clause - Filtering grouped data
- **DISTINCT** keyword - Removing duplicates
- **LIMIT** clause - Controlling result size

### Homework:

#### Task 1: E-commerce Database
Create a complete e-commerce schema with:
- `customers` table (customer_id, name, email, phone, registration_date)
- `products` table (product_id, name, price, stock, category)
- `orders` table (order_id, customer_id, order_date, total_amount, status)
- `order_items` table (item_id, order_id, product_id, quantity, price)

Include all appropriate:
- Data types
- Constraints (NOT NULL, UNIQUE, CHECK)
- Foreign keys with appropriate actions
- Indexes on frequently searched columns

<details>
<summary>Solution</summary>

```sql
-- Customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(15),
    registration_date DATE DEFAULT (CURRENT_DATE),
    INDEX idx_email (email)
);

-- Products table
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(200) NOT NULL,
    price DECIMAL(10,2) CHECK (price > 0),
    stock INT DEFAULT 0 CHECK (stock >= 0),
    category VARCHAR(50),
    INDEX idx_category (category),
    INDEX idx_name (name)
);

-- Orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2) CHECK (total_amount >= 0),
    status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled') DEFAULT 'pending',
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE,
    INDEX idx_customer (customer_id),
    INDEX idx_order_date (order_date),
    INDEX idx_status (status)
);

-- Order items table
CREATE TABLE order_items (
    item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT CHECK (quantity > 0),
    price DECIMAL(10,2) CHECK (price > 0),
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(product_id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE,
    INDEX idx_order (order_id),
    INDEX idx_product (product_id)
);
```
</details>

#### Task 2: Practice Queries

Using the employees table:
```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE,
    manager_id INT,
    is_active BOOLEAN DEFAULT TRUE
);

-- Insert sample data
INSERT INTO employees (first_name, last_name, email, department, salary, hire_date, manager_id, is_active) VALUES
('John', 'Doe', 'john.doe@company.com', 'Engineering', 75000, '2022-01-15', NULL, TRUE),
('Jane', 'Smith', 'jane.smith@company.com', 'Marketing', 65000, '2022-03-20', 1, TRUE),
('Mike', 'Johnson', 'mike.j@company.com', 'Engineering', 80000, '2021-11-10', 1, TRUE),
('Sarah', 'Williams', 'sarah.w@company.com', 'HR', 60000, '2023-01-05', 1, TRUE),
('Tom', 'Brown', 'tom.brown@company.com', 'Sales', 70000, '2022-08-22', 1, FALSE),
('Emily', 'Davis', 'emily.d@company.com', 'Engineering', 72000, '2023-02-14', 3, TRUE),
('David', 'Wilson', 'david.w@company.com', 'Marketing', 68000, '2022-06-30', 2, TRUE),
('Lisa', 'Anderson', 'lisa.a@company.com', 'Engineering', 85000, '2020-09-01', 1, TRUE),
('Robert', 'Taylor', 'robert.t@company.com', 'Sales', 71000, '2023-03-15', 5, TRUE),
('Jennifer', 'Martinez', 'jennifer.m@company.com', 'HR', 62000, '2022-11-20', 4, TRUE);
```

Write queries for:

1. Find all employees in Engineering department with salary > 70000
2. Find employees whose first name starts with 'J' or 'S'
3. Find employees hired between 2022-01-01 and 2022-12-31
4. Find inactive employees (is_active = FALSE)
5. Find employees without a manager
6. Find employees with company email (contains '@company.com')
7. Find employees in Engineering or Marketing with salary between 65000 and 75000
8. Find all active employees whose last name contains 'son'
9. Find employees in departments NOT in ('HR', 'Sales')
10. Find employees with NULL email or manager_id

<details>
<summary>Solutions</summary>

```sql
-- 1. Engineering with salary > 70000
SELECT * FROM employees 
WHERE department = 'Engineering' AND salary > 70000;

-- 2. First name starts with 'J' or 'S'
SELECT * FROM employees 
WHERE first_name LIKE 'J%' OR first_name LIKE 'S%';

-- 3. Hired in 2022
SELECT * FROM employees 
WHERE hire_date BETWEEN '2022-01-01' AND '2022-12-31';

-- 4. Inactive employees
SELECT * FROM employees 
WHERE is_active = FALSE;

-- 5. Without manager
SELECT * FROM employees 
WHERE manager_id IS NULL;

-- 6. Company email
SELECT * FROM employees 
WHERE email LIKE '%@company.com';

-- 7. Engineering or Marketing, salary 65000-75000
SELECT * FROM employees 
WHERE department IN ('Engineering', 'Marketing') 
  AND salary BETWEEN 65000 AND 75000;

-- 8. Active employees, last name contains 'son'
SELECT * FROM employees 
WHERE is_active = TRUE AND last_name LIKE '%son%';

-- 9. Not in HR or Sales
SELECT * FROM employees 
WHERE department NOT IN ('HR', 'Sales');

-- 10. NULL email or manager_id
SELECT * FROM employees 
WHERE email IS NULL OR manager_id IS NULL;
```
</details>

#### Task 3: Design Your Own Database

Choose one of these scenarios and create a complete database schema:

**Option A: Hospital Management**
- Patients (patient_id, name, dob, gender, phone, address)
- Doctors (doctor_id, name, specialization, phone, email)
- Appointments (appointment_id, patient_id, doctor_id, date_time, status, notes)

**Option B: Online Learning Platform**
- Students (student_id, name, email, enrollment_date)
- Instructors (instructor_id, name, email, specialization)
- Courses (course_id, title, instructor_id, price, duration)
- Enrollments (enrollment_id, student_id, course_id, enrollment_date, progress)

**Option C: Restaurant Management**
- Customers (customer_id, name, phone, email)
- Menu_Items (item_id, name, category, price, is_available)
- Orders (order_id, customer_id, order_date, total_amount, status)
- Order_Details (detail_id, order_id, item_id, quantity, price)

Requirements:
- Use appropriate data types for each column
- Add NOT NULL constraints where necessary
- Add CHECK constraints for validation
- Create foreign keys with appropriate actions
- Add UNIQUE constraints where needed
- Set DEFAULT values where appropriate
- Create indexes on frequently searched columns
- Insert at least 5 records in each table
- Write 5 different SELECT queries using WHERE, LIKE, IN, BETWEEN, IS NULL

---

## Additional Resources

### Documentation
- [MySQL Data Types Reference](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
- [MySQL Constraints](https://dev.mysql.com/doc/refman/8.0/en/constraints.html)
- [MySQL Indexes](https://dev.mysql.com/doc/refman/8.0/en/optimization-indexes.html)

### Practice Platforms
- [LeetCode Database](https://leetcode.com/problemset/database/)
- [HackerRank SQL](https://www.hackerrank.com/domains/sql)
- [SQLZoo](https://sqlzoo.net/)

### Tools
- [DB Fiddle](https://www.db-fiddle.com/) - Online SQL editor
- [DbDiagram.io](https://dbdiagram.io/) - Database schema design
- MySQL Workbench - Visual database design

---

## Quick Reference Card

### Data Type Cheat Sheet
```sql
-- Numeric
TINYINT        -- 1 byte, -128 to 127
INT            -- 4 bytes, ~2 billion
BIGINT         -- 8 bytes, very large
DECIMAL(10,2)  -- Exact decimal, 10 digits, 2 after point

-- String
CHAR(10)       -- Fixed 10 characters
VARCHAR(100)   -- Variable up to 100
TEXT           -- Up to 65KB
ENUM('a','b')  -- List of allowed values

-- Date/Time
DATE           -- YYYY-MM-DD
DATETIME       -- YYYY-MM-DD HH:MM:SS
TIMESTAMP      -- Auto-updating timestamp

-- Other
BOOLEAN        -- TRUE/FALSE (stored as TINYINT)
```

### Constraint Syntax
```sql
column_name DATA_TYPE NOT NULL
column_name DATA_TYPE UNIQUE
column_name DATA_TYPE DEFAULT value
column_name DATA_TYPE CHECK (condition)
column_name INT PRIMARY KEY AUTO_INCREMENT
FOREIGN KEY (column) REFERENCES other_table(column)
```

### WHERE Operators
```sql
=, !=, <, >, <=, >=           -- Comparison
AND, OR, NOT                   -- Logical
LIKE 'pattern'                 -- Pattern matching
IN (val1, val2)               -- List membership
BETWEEN val1 AND val2         -- Range (inclusive)
IS NULL, IS NOT NULL          -- NULL checking
```

---
