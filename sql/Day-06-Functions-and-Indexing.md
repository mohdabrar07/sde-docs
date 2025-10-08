# Day 06 - Functions & Indexing

## 📋 Table of Contents
- [Introduction](#introduction)
- [SQL Functions Overview](#sql-functions-overview)
- [Aggregate Functions](#aggregate-functions)
- [Scalar Functions](#scalar-functions)
- [String Functions](#string-functions)
- [Numeric Functions](#numeric-functions)
- [Date and Time Functions](#date-and-time-functions)
- [Indexing in SQL](#indexing-in-sql)
- [Types of Indexes](#types-of-indexes)
- [Primary Key vs Unique Key vs Index](#primary-key-vs-unique-key-vs-index)
- [Practice Exercises](#practice-exercises)
- [Common Interview Questions](#common-interview-questions)

---

## Introduction

Functions and indexing are crucial concepts for database optimization and data manipulation. Functions help in transforming and analyzing data, while indexing improves query performance significantly.

---

## SQL Functions Overview

SQL functions are classified into two main categories:

### 1. Aggregate Functions
- Operate on multiple rows and return a single value
- Used with GROUP BY clause
- Examples: SUM(), AVG(), COUNT(), MAX(), MIN()

### 2. Scalar Functions
- Operate on individual values and return a single value
- Applied to each row independently
- Examples: UPPER(), ROUND(), NOW(), CONCAT()

---

## Aggregate Functions

Aggregate functions perform calculations on a set of values and return a single result.

### 1. COUNT()
Counts the number of rows or non-NULL values.

```sql
-- Count total employees
SELECT COUNT(*) AS total_employees FROM employees;

-- Count non-NULL emails
SELECT COUNT(email) AS employees_with_email FROM employees;

-- Count distinct departments
SELECT COUNT(DISTINCT department_id) AS unique_departments FROM employees;
```

### 2. SUM()
Calculates the sum of numeric values.

```sql
-- Total salary expenditure
SELECT SUM(salary) AS total_salary_cost FROM employees;

-- Department-wise salary sum
SELECT department_id, SUM(salary) AS dept_total_salary
FROM employees
GROUP BY department_id;
```

### 3. AVG()
Calculates the average of numeric values.

```sql
-- Average salary
SELECT AVG(salary) AS average_salary FROM employees;

-- Average salary by job title
SELECT job_title, AVG(salary) AS avg_salary
FROM employees
GROUP BY job_title
ORDER BY avg_salary DESC;
```

### 4. MAX() and MIN()
Find maximum and minimum values.

```sql
-- Highest and lowest salary
SELECT 
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;

-- Latest and earliest hire date
SELECT 
    MAX(hire_date) AS latest_hire,
    MIN(hire_date) AS earliest_hire
FROM employees;
```

### 5. GROUP_CONCAT()
Concatenates values from multiple rows.

```sql
-- Get all employee names in each department
SELECT 
    department_id,
    GROUP_CONCAT(first_name ORDER BY first_name SEPARATOR ', ') AS employees
FROM employees
GROUP BY department_id;
```

### Aggregate Functions with HAVING

```sql
-- Departments with average salary > 60000
SELECT 
    department_id,
    COUNT(*) AS emp_count,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 60000;
```

---

## Scalar Functions

Scalar functions return a single value for each row in the result set.

---

## String Functions

### 1. CONCAT() and CONCAT_WS()
Concatenate strings together.

```sql
-- Concatenate first and last name
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;

-- Concatenate with separator
SELECT CONCAT_WS(' - ', employee_id, first_name, job_title) AS employee_info
FROM employees;
```

### 2. UPPER() and LOWER()
Convert strings to uppercase or lowercase.

```sql
SELECT 
    UPPER(first_name) AS uppercase_name,
    LOWER(email) AS lowercase_email
FROM employees;
```

### 3. LENGTH() and CHAR_LENGTH()
Return the length of a string.

```sql
-- Length in bytes (LENGTH) vs characters (CHAR_LENGTH)
SELECT 
    first_name,
    LENGTH(first_name) AS byte_length,
    CHAR_LENGTH(first_name) AS char_length
FROM employees;
```

### 4. SUBSTRING() / SUBSTR()
Extract a portion of a string.

```sql
-- Extract first 3 characters
SELECT SUBSTRING(first_name, 1, 3) AS short_name FROM employees;

-- Get domain from email
SELECT 
    email,
    SUBSTRING(email, LOCATE('@', email) + 1) AS domain
FROM employees;
```

### 5. TRIM(), LTRIM(), RTRIM()
Remove whitespace from strings.

```sql
SELECT 
    TRIM('  Hello World  ') AS trimmed,
    LTRIM('  Hello World  ') AS left_trimmed,
    RTRIM('  Hello World  ') AS right_trimmed;
```

### 6. REPLACE()
Replace occurrences of a substring.

```sql
-- Replace @ with [at] in emails
SELECT REPLACE(email, '@', '[at]') AS masked_email FROM employees;
```

### 7. LEFT() and RIGHT()
Extract characters from left or right.

```sql
SELECT 
    LEFT(first_name, 1) AS first_initial,
    RIGHT(phone_number, 4) AS last_four_digits
FROM employees;
```

### 8. REVERSE()
Reverse a string.

```sql
SELECT first_name, REVERSE(first_name) AS reversed_name FROM employees;
```

### 9. LOCATE() / POSITION()
Find position of substring.

```sql
-- Find position of '@' in email
SELECT email, LOCATE('@', email) AS at_position FROM employees;
```

### 10. LPAD() and RPAD()
Pad strings to a certain length.

```sql
-- Pad employee ID with zeros
SELECT LPAD(employee_id, 6, '0') AS padded_id FROM employees;
```

---

## Numeric Functions

### 1. ROUND()
Round a number to specified decimal places.

```sql
-- Round salary to nearest thousand
SELECT 
    salary,
    ROUND(salary, -3) AS rounded_salary
FROM employees;

-- Round to 2 decimal places
SELECT ROUND(AVG(salary), 2) AS avg_salary FROM employees;
```

### 2. CEIL() and FLOOR()
Round up or down to nearest integer.

```sql
SELECT 
    salary,
    CEIL(salary / 1000) AS ceil_thousands,
    FLOOR(salary / 1000) AS floor_thousands
FROM employees;
```

### 3. ABS()
Return absolute value.

```sql
SELECT ABS(-150) AS absolute_value;  -- Returns 150
```

### 4. POWER() / POW()
Raise to a power.

```sql
SELECT POWER(2, 3) AS result;  -- Returns 8 (2^3)
```

### 5. SQRT()
Calculate square root.

```sql
SELECT SQRT(16) AS square_root;  -- Returns 4
```

### 6. MOD()
Return remainder of division.

```sql
-- Find even employee IDs
SELECT employee_id FROM employees WHERE MOD(employee_id, 2) = 0;
```

### 7. RAND()
Generate random number between 0 and 1.

```sql
-- Select 5 random employees
SELECT * FROM employees ORDER BY RAND() LIMIT 5;
```

### 8. TRUNCATE()
Truncate number to specified decimal places.

```sql
SELECT TRUNCATE(salary, -3) AS truncated_salary FROM employees;
```

---

## Date and Time Functions

### 1. NOW() and CURDATE()
Get current date and time.

```sql
SELECT 
    NOW() AS current_datetime,
    CURDATE() AS current_date,
    CURTIME() AS current_time;
```

### 2. DATE(), TIME(), YEAR(), MONTH(), DAY()
Extract parts of date.

```sql
SELECT 
    hire_date,
    DATE(hire_date) AS date_only,
    YEAR(hire_date) AS year,
    MONTH(hire_date) AS month,
    DAY(hire_date) AS day,
    MONTHNAME(hire_date) AS month_name,
    DAYNAME(hire_date) AS day_name
FROM employees;
```

### 3. DATEDIFF()
Calculate difference between dates.

```sql
-- Calculate years of service
SELECT 
    first_name,
    hire_date,
    DATEDIFF(CURDATE(), hire_date) AS days_employed,
    ROUND(DATEDIFF(CURDATE(), hire_date) / 365, 1) AS years_employed
FROM employees;
```

### 4. DATE_ADD() and DATE_SUB()
Add or subtract intervals from dates.

```sql
-- Calculate probation end date (90 days after hire)
SELECT 
    hire_date,
    DATE_ADD(hire_date, INTERVAL 90 DAY) AS probation_end
FROM employees;

-- Date 1 year ago
SELECT DATE_SUB(CURDATE(), INTERVAL 1 YEAR) AS one_year_ago;
```

### 5. DATE_FORMAT()
Format dates as strings.

```sql
SELECT 
    hire_date,
    DATE_FORMAT(hire_date, '%d-%m-%Y') AS formatted_date,
    DATE_FORMAT(hire_date, '%M %d, %Y') AS long_format,
    DATE_FORMAT(hire_date, '%W, %M %d') AS day_month
FROM employees;
```

### 6. STR_TO_DATE()
Convert string to date.

```sql
SELECT STR_TO_DATE('15-01-2024', '%d-%m-%Y') AS converted_date;
```

### 7. TIMESTAMPDIFF()
Calculate difference in specific units.

```sql
-- Calculate age in years
SELECT 
    first_name,
    date_of_birth,
    TIMESTAMPDIFF(YEAR, date_of_birth, CURDATE()) AS age
FROM employees;
```

### 8. LAST_DAY()
Get last day of the month.

```sql
SELECT LAST_DAY(CURDATE()) AS last_day_of_month;
```

---

## Indexing in SQL

Indexes are database objects that improve the speed of data retrieval operations. They work similar to book indexes, allowing quick lookup of data.

### Why Use Indexes?

**Advantages:**
- Dramatically speeds up SELECT queries with WHERE clauses
- Improves JOIN performance
- Speeds up ORDER BY and GROUP BY operations
- Enforces uniqueness (UNIQUE index)

**Disadvantages:**
- Slows down INSERT, UPDATE, DELETE operations
- Consumes additional disk space
- Maintenance overhead

### Creating Indexes

```sql
-- Create single-column index
CREATE INDEX idx_employee_lastname ON employees(last_name);

-- Create multi-column index
CREATE INDEX idx_dept_salary ON employees(department_id, salary);

-- Create unique index
CREATE UNIQUE INDEX idx_unique_email ON employees(email);

-- View indexes on a table
SHOW INDEX FROM employees;

-- Drop index
DROP INDEX idx_employee_lastname ON employees;
```

---

## Types of Indexes

### 1. Clustered Index

A clustered index determines the physical order of data in a table.

**Characteristics:**
- Only ONE clustered index per table
- Data rows are sorted and stored based on the clustered index key
- Primary key automatically creates a clustered index
- Faster for range queries

```sql
-- Primary key creates clustered index automatically
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,  -- Clustered index
    first_name VARCHAR(50),
    salary DECIMAL(10,2)
);
```

**Example Query Optimization:**
```sql
-- Fast with clustered index on employee_id
SELECT * FROM employees WHERE employee_id BETWEEN 100 AND 200;
```

### 2. Non-Clustered Index

A non-clustered index creates a separate structure that points to data rows.

**Characteristics:**
- Multiple non-clustered indexes per table (up to 999 in SQL Server)
- Contains index key values and row locators
- Doesn't affect physical data storage
- Requires additional space

```sql
-- Create non-clustered indexes
CREATE INDEX idx_salary ON employees(salary);
CREATE INDEX idx_department ON employees(department_id);
CREATE INDEX idx_name ON employees(first_name, last_name);
```

**Example Query Optimization:**
```sql
-- Fast with non-clustered index on salary
SELECT * FROM employees WHERE salary > 70000;
```

### Clustered vs Non-Clustered Index

| Feature | Clustered Index | Non-Clustered Index |
|---------|----------------|---------------------|
| Number per table | One | Multiple (999+) |
| Data storage | Physically sorted | Logical order only |
| Speed | Faster for range queries | Slower than clustered |
| Disk space | No extra space | Requires extra space |
| Key lookup | Direct | Indirect (via pointer) |
| Default | Primary Key | Must create explicitly |

---

## Primary Key vs Unique Key vs Index

### Comparison Table

| Feature | Primary Key | Unique Key | Index |
|---------|-------------|------------|-------|
| **NULL values** | Not allowed | Allowed (one NULL) | Allowed |
| **Number per table** | One | Multiple | Multiple |
| **Automatically creates index** | Yes (Clustered) | Yes (Non-clustered) | N/A |
| **Purpose** | Uniquely identify rows | Ensure uniqueness | Improve query speed |
| **Can be referenced as FK** | Yes | Yes | No |
| **Modifiable** | Difficult | Possible | Easy |

### 1. Primary Key

```sql
-- Define primary key during table creation
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50)
);

-- Add primary key to existing table
ALTER TABLE employees ADD PRIMARY KEY (employee_id);

-- Composite primary key
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

**Key Points:**
- Uniquely identifies each record
- Cannot contain NULL values
- Automatically creates a clustered index
- Only one per table

### 2. Unique Key

```sql
-- Define unique key during table creation
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(15) UNIQUE
);

-- Add unique constraint to existing table
ALTER TABLE employees ADD UNIQUE (email);

-- Named unique constraint
ALTER TABLE employees 
ADD CONSTRAINT uk_employee_email UNIQUE (email);

-- Composite unique key
ALTER TABLE employees 
ADD UNIQUE (first_name, last_name, date_of_birth);
```

**Key Points:**
- Ensures uniqueness of values
- Can contain one NULL value
- Automatically creates a non-clustered index
- Multiple unique keys allowed per table

### 3. Index

```sql
-- Simple index
CREATE INDEX idx_lastname ON employees(last_name);

-- Composite index
CREATE INDEX idx_name_salary ON employees(last_name, first_name, salary);

-- Unique index
CREATE UNIQUE INDEX idx_unique_email ON employees(email);

-- Full-text index (for text searching)
CREATE FULLTEXT INDEX idx_fulltext_bio ON employees(bio);

-- Drop index
DROP INDEX idx_lastname ON employees;
```

**Key Points:**
- Improves query performance
- Does not enforce uniqueness (unless UNIQUE index)
- Multiple indexes allowed
- Can be created and dropped without affecting data

### When to Use Each

**Use Primary Key when:**
- You need a unique identifier for each record
- You want to establish relationships (foreign keys)
- You need guaranteed uniqueness and non-NULL values

**Use Unique Key when:**
- You need to ensure uniqueness but PRIMARY KEY already exists
- You want to allow one NULL value
- Multiple columns need uniqueness (email, username, phone)

**Use Index when:**
- You frequently search by specific columns
- You want to speed up JOIN operations
- ORDER BY or GROUP BY queries are slow
- No uniqueness enforcement needed

---

## Practice Exercises

### Exercise 1: String Functions

```sql
-- Create sample data
INSERT INTO employees (first_name, last_name, email, phone_number)
VALUES 
    ('john', 'doe', 'john.doe@company.com', '555-1234'),
    ('jane', 'SMITH', 'JANE.SMITH@company.com', '555-5678');

-- Clean and format the data
SELECT 
    CONCAT(UPPER(LEFT(first_name, 1)), LOWER(SUBSTRING(first_name, 2))) AS formatted_first_name,
    CONCAT(UPPER(LEFT(last_name, 1)), LOWER(SUBSTRING(last_name, 2))) AS formatted_last_name,
    LOWER(email) AS normalized_email,
    CONCAT('(', LEFT(phone_number, 3), ') ', SUBSTRING(phone_number, 5)) AS formatted_phone
FROM employees;
```

### Exercise 2: Date Functions

```sql
-- Calculate employee tenure and upcoming anniversaries
SELECT 
    first_name,
    hire_date,
    TIMESTAMPDIFF(YEAR, hire_date, CURDATE()) AS years_of_service,
    DATEDIFF(CURDATE(), hire_date) AS days_employed,
    DATE_ADD(hire_date, INTERVAL YEAR(CURDATE()) - YEAR(hire_date) YEAR) AS this_year_anniversary,
    CASE 
        WHEN DAYOFYEAR(DATE_ADD(hire_date, INTERVAL YEAR(CURDATE()) - YEAR(hire_date) YEAR)) > DAYOFYEAR(CURDATE())
        THEN DATEDIFF(DATE_ADD(hire_date, INTERVAL YEAR(CURDATE()) - YEAR(hire_date) YEAR), CURDATE())
        ELSE NULL
    END AS days_until_anniversary
FROM employees;
```

### Exercise 3: Aggregate Functions

```sql
-- Department-wise statistics
SELECT 
    department_id,
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS avg_salary,
    MIN(salary) AS min_salary,
    MAX(salary) AS max_salary,
    STDDEV(salary) AS salary_stddev
FROM employees
GROUP BY department_id
ORDER BY avg_salary DESC;
```

### Exercise 4: Creating Indexes

```sql
-- Create indexes for optimization
CREATE INDEX idx_department ON employees(department_id);
CREATE INDEX idx_salary ON employees(salary);
CREATE INDEX idx_hire_date ON employees(hire_date);
CREATE INDEX idx_dept_salary ON employees(department_id, salary);

-- Test query performance
EXPLAIN SELECT * FROM employees WHERE department_id = 5 AND salary > 60000;

-- Drop unnecessary index
DROP INDEX idx_salary ON employees;
```

---

## Common Interview Questions

### 1. What is the difference between aggregate and scalar functions?

**Aggregate Functions:**
- Operate on multiple rows, return single value
- Used with GROUP BY
- Examples: SUM(), AVG(), COUNT()

**Scalar Functions:**
- Operate on single value, return single value
- Applied to each row
- Examples: UPPER(), ROUND(), NOW()

### 2. Can we use aggregate functions without GROUP BY?

Yes, they will operate on all rows and return a single value:
```sql
SELECT AVG(salary) FROM employees;  -- Returns one average value
```

### 3. What is the difference between CHAR_LENGTH() and LENGTH()?

- **CHAR_LENGTH()**: Returns number of characters
- **LENGTH()**: Returns number of bytes (matters for multi-byte characters)

### 4. How does indexing improve performance?

Indexes create a data structure (B-Tree) that allows fast lookup without scanning entire table, reducing query execution time from O(n) to O(log n).

### 5. What are the disadvantages of indexing?

- Slows down INSERT, UPDATE, DELETE
- Consumes additional storage
- Requires maintenance
- Too many indexes can confuse query optimizer

### 6. Can a table have multiple primary keys?

No, a table can have only ONE primary key, but it can be composite (multiple columns).

### 7. What is a composite index?

An index on multiple columns:
```sql
CREATE INDEX idx_name_dept ON employees(last_name, first_name, department_id);
```
Useful for queries filtering on multiple columns.

### 8. When should you create an index?

- Columns frequently used in WHERE clauses
- Columns used in JOIN conditions
- Columns used in ORDER BY
- Large tables with many SELECT queries
- Columns with high cardinality (many unique values)

### 9. What is the difference between DROP and TRUNCATE?

- **DROP**: Removes table structure and data
- **TRUNCATE**: Removes all data but keeps structure

### 10. Can you index NULL values?

Yes, most databases allow indexing NULL values, but behavior varies by database system.

---

## Summary

- **Aggregate functions** (COUNT, SUM, AVG, MAX, MIN) work on multiple rows
- **Scalar functions** work on individual values per row
- **String functions** manipulate text data
- **Numeric functions** perform mathematical operations
- **Date functions** handle temporal data
- **Indexes** dramatically improve query performance
- **Clustered index** determines physical data order (one per table)
- **Non-clustered index** creates separate lookup structure (multiple allowed)
- **Primary Key** ensures uniqueness and non-NULL
- **Unique Key** ensures uniqueness but allows one NULL
- **Index** improves performance without enforcing constraints

---

