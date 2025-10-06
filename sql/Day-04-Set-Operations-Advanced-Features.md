Day 4: MySQL Set Operations & Advanced Features
Table of Contents
Set Operations

Subquery Operations

Views and Temporary Tables

Index Basics

Practice Exercises

Set Operations
UNION vs UNION ALL
UNION

Combines results from multiple SELECT statements

Removes duplicate rows

Requires same number of columns and compatible data types

Syntax:

sql
SELECT column1, column2 FROM table1
UNION
SELECT column1, column2 FROM table2;
UNION ALL

Combines results from multiple SELECT statements

Includes all rows, including duplicates

More efficient than UNION when duplicates are acceptable

Syntax:

sql
SELECT column1, column2 FROM table1
UNION ALL
SELECT column1, column2 FROM table2;
Key Differences:

sql
-- Example demonstrating difference
CREATE TABLE employees_ny (
    id INT,
    name VARCHAR(50),
    department VARCHAR(50)
);

CREATE TABLE employees_la (
    id INT,
    name VARCHAR(50),
    department VARCHAR(50)
);

INSERT INTO employees_ny VALUES 
(1, 'John Doe', 'IT'),
(2, 'Jane Smith', 'HR'),
(3, 'Mike Johnson', 'Finance');

INSERT INTO employees_la VALUES 
(2, 'Jane Smith', 'HR'),
(4, 'Sarah Wilson', 'Marketing'),
(5, 'Tom Brown', 'IT');

-- UNION (removes duplicates)
SELECT name, department FROM employees_ny
UNION
SELECT name, department FROM employees_la;

-- UNION ALL (includes duplicates)
SELECT name, department FROM employees_ny
UNION ALL
SELECT name, department FROM employees_la;
INTERSECT and EXCEPT
INTERSECT (MySQL 8.0+)

Returns distinct rows that appear in both result sets

Alternative in older MySQL versions using INNER JOIN or EXISTS

sql
-- MySQL 8.0+ syntax
SELECT name FROM employees_ny
INTERSECT
SELECT name FROM employees_la;

-- Alternative using INNER JOIN
SELECT DISTINCT n.name 
FROM employees_ny n
INNER JOIN employees_la l ON n.name = l.name;

-- Alternative using EXISTS
SELECT name 
FROM employees_ny n
WHERE EXISTS (SELECT 1 FROM employees_la l WHERE l.name = n.name);
EXCEPT (MySQL 8.0+) or MINUS

Returns distinct rows from first query that don't appear in second query

sql
-- MySQL 8.0+ syntax
SELECT name FROM employees_ny
EXCEPT
SELECT name FROM employees_la;

-- Alternative using LEFT JOIN
SELECT n.name 
FROM employees_ny n
LEFT JOIN employees_la l ON n.name = l.name
WHERE l.name IS NULL;
Subquery Operations
IN vs EXISTS
IN Operator

Checks if a value matches any value in a list or subquery

Better for small result sets

Subquery executes first, then main query uses the result

sql
-- Find employees in specific departments
SELECT name, department 
FROM employees 
WHERE department IN ('IT', 'HR', 'Finance');

-- Using subquery
SELECT name, department 
FROM employees 
WHERE department IN (SELECT DISTINCT department FROM departments WHERE active = 1);
EXISTS Operator

Checks if subquery returns any rows

More efficient for large datasets

Stops execution when first match is found (correlated subquery)

sql
-- Find employees who have at least one order
SELECT e.name, e.department
FROM employees e
WHERE EXISTS (
    SELECT 1 
    FROM orders o 
    WHERE o.employee_id = e.id
);

-- Alternative with JOIN (often more efficient)
SELECT DISTINCT e.name, e.department
FROM employees e
INNER JOIN orders o ON e.id = o.employee_id;
Performance Comparison:

sql
-- Sample data setup for comparison
CREATE TABLE large_table (id INT PRIMARY KEY, data VARCHAR(100));
CREATE TABLE small_table (id INT PRIMARY KEY, info VARCHAR(100));

-- IN approach (subquery executes first)
SELECT * FROM large_table 
WHERE id IN (SELECT id FROM small_table WHERE condition);

-- EXISTS approach (correlated subquery)
SELECT * FROM large_table lt
WHERE EXISTS (SELECT 1 FROM small_table st WHERE st.id = lt.id AND condition);
Views and Temporary Tables
Views
Virtual tables based on result set of a SELECT statement

Don't store data physically

Simplify complex queries and enhance security

sql
-- Creating a view
CREATE VIEW employee_summary AS
SELECT 
    e.id,
    e.name,
    e.department,
    d.department_head,
    COUNT(o.id) as order_count
FROM employees e
LEFT JOIN departments d ON e.department = d.name
LEFT JOIN orders o ON e.id = o.employee_id
GROUP BY e.id, e.name, e.department, d.department_head;

-- Using the view
SELECT * FROM employee_summary WHERE order_count > 5;

-- Updating a view (if it meets certain conditions)
CREATE OR REPLACE VIEW employee_summary AS
SELECT ... -- updated query

-- Dropping a view
DROP VIEW IF EXISTS employee_summary;
Updatable Views Requirements:

No aggregate functions

No DISTINCT, GROUP BY, or HAVING

No subqueries in certain positions

Must contain all NOT NULL columns from base table

Temporary Tables
Exist only for current database session

Automatically dropped when session ends

Useful for intermediate calculations

sql
-- Creating temporary table
CREATE TEMPORARY TABLE temp_employee_stats AS
SELECT 
    department,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department;

-- Using temporary table
SELECT * FROM temp_employee_stats 
WHERE employee_count > 10 
ORDER BY avg_salary DESC;

-- Explicitly dropping temporary table
DROP TEMPORARY TABLE IF EXISTS temp_employee_stats;

-- Alternative syntax (MySQL specific)
CREATE TEMPORARY TABLE temp_products LIKE products;
INSERT INTO temp_products SELECT * FROM products WHERE price > 100;
Temporary Table vs View:

sql
-- View (virtual, always current data)
CREATE VIEW current_employees AS 
SELECT * FROM employees WHERE status = 'active';

-- Temporary Table (physical storage, snapshot in time)
CREATE TEMPORARY TABLE temp_current_employees AS 
SELECT * FROM employees WHERE status = 'active';
Index Basics
Why Use Indexes?
Improve query performance

Speed up data retrieval

Enforce uniqueness constraints

When to Use Indexes
Columns frequently used in WHERE clauses

Columns used in JOIN conditions

Columns used for sorting (ORDER BY)

Columns with high cardinality

Index Types in MySQL
Basic Index Creation:

sql
-- Single column index
CREATE INDEX idx_employee_department ON employees(department);

-- Composite index (multiple columns)
CREATE INDEX idx_employee_name_dept ON employees(last_name, first_name, department);

-- Unique index
CREATE UNIQUE INDEX idx_employee_email ON employees(email);

-- Primary key (automatically indexed)
ALTER TABLE employees ADD PRIMARY KEY (id);

-- Show indexes on a table
SHOW INDEX FROM employees;

-- Dropping indexes
DROP INDEX idx_employee_department ON employees;
When NOT to Use Indexes:

Small tables

Tables with frequent write operations and few reads

Columns with low cardinality (few unique values)

Columns rarely used in queries

Best Practices:

sql
-- Good candidates for indexing
CREATE INDEX idx_orders_date ON orders(order_date);
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_products_category_price ON products(category_id, price);

-- Query using index
SELECT * FROM orders WHERE order_date BETWEEN '2024-01-01' AND '2024-01-31';

-- EXPLAIN to check index usage
EXPLAIN SELECT * FROM employees WHERE department = 'IT';
Practice Exercises
Exercise 1: UNION Operations
sql
-- Create sample tables
CREATE TABLE current_employees (
    id INT,
    name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);

CREATE TABLE former_employees (
    id INT,
    name VARCHAR(50),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);

INSERT INTO current_employees VALUES 
(1, 'Alice Johnson', 'IT', 75000),
(2, 'Bob Smith', 'HR', 65000),
(3, 'Carol Davis', 'Finance', 80000),
(4, 'David Wilson', 'IT', 72000);

INSERT INTO former_employees VALUES 
(2, 'Bob Smith', 'HR', 65000),
(5, 'Eva Brown', 'Marketing', 70000),
(6, 'Frank Miller', 'IT', 68000);

-- 1. Combine all employees (current and former) without duplicates
SELECT name, department, salary FROM current_employees
UNION
SELECT name, department, salary FROM former_employees;

-- 2. Combine all employees including duplicates
SELECT name, department, salary FROM current_employees
UNION ALL
SELECT name, department, salary FROM former_employees;

-- 3. Find employees who worked in both tables (simulate INTERSECT)
SELECT name, department, salary FROM current_employees
WHERE (name, department) IN (SELECT name, department FROM former_employees);

-- 4. Find employees only in current_employees (simulate EXCEPT)
SELECT name, department, salary FROM current_employees
WHERE (name, department) NOT IN (SELECT name, department FROM former_employees);
Exercise 2: EXISTS and IN Practice
sql
-- Create related tables
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    budget DECIMAL(12,2)
);

CREATE TABLE projects (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    budget DECIMAL(10,2)
);

INSERT INTO departments VALUES 
(1, 'IT', 1000000),
(2, 'HR', 500000),
(3, 'Finance', 800000),
(4, 'Marketing', 600000);

INSERT INTO projects VALUES 
(1, 'Website Redesign', 1, 50000),
(2, 'Recruitment System', 2, 30000),
(3, 'Financial Reporting', 3, 40000),
(4, 'Social Media Campaign', 4, 25000);

-- 1. Find departments that have projects using EXISTS
SELECT d.name, d.budget
FROM departments d
WHERE EXISTS (
    SELECT 1 
    FROM projects p 
    WHERE p.department_id = d.id
);

-- 2. Find departments that have projects using IN
SELECT d.name, d.budget
FROM departments d
WHERE d.id IN (SELECT DISTINCT department_id FROM projects);

-- 3. Find departments without projects using NOT EXISTS
SELECT d.name, d.budget
FROM departments d
WHERE NOT EXISTS (
    SELECT 1 
    FROM projects p 
    WHERE p.department_id = d.id
);

-- 4. Find departments with high-budget projects (> 35000)
SELECT d.name, d.budget
FROM departments d
WHERE EXISTS (
    SELECT 1 
    FROM projects p 
    WHERE p.department_id = d.id AND p.budget > 35000
);
Exercise 3: Views and Indexes
sql
-- 1. Create a comprehensive employee view
CREATE VIEW employee_detailed_view AS
SELECT 
    e.id,
    e.name,
    e.department,
    e.salary,
    d.budget as department_budget,
    COUNT(p.id) as project_count,
    SUM(p.budget) as total_project_budget
FROM current_employees e
LEFT JOIN departments d ON e.department = d.name
LEFT JOIN projects p ON d.id = p.department_id
GROUP BY e.id, e.name, e.department, e.salary, d.budget;

-- Query the view
SELECT * FROM employee_detailed_view 
WHERE total_project_budget > 30000 
ORDER BY salary DESC;

-- 2. Create appropriate indexes
-- Index for department filtering
CREATE INDEX idx_employees_department ON current_employees(department);

-- Index for salary range queries
CREATE INDEX idx_employees_salary ON current_employees(salary);

-- Composite index for department and salary queries
CREATE INDEX idx_employees_dept_salary ON current_employees(department, salary);

-- 3. Test index usage
EXPLAIN SELECT * FROM current_employees WHERE department = 'IT' AND salary > 70000;

-- 4. Create temporary table for reporting
CREATE TEMPORARY TABLE temp_department_report AS
SELECT 
    department,
    COUNT(*) as employee_count,
    AVG(salary) as average_salary,
    MAX(salary) as max_salary,
    MIN(salary) as min_salary
FROM current_employees
GROUP BY department;

-- Use temporary table for analysis
SELECT * FROM temp_department_report 
WHERE employee_count >= 2 
ORDER BY average_salary DESC;
Exercise 4: Performance Comparison
sql
-- Compare IN vs EXISTS with large datasets
-- Setup (conceptual - for large tables)

-- Method 1: IN (subquery executes first)
SELECT * FROM large_table 
WHERE id IN (SELECT id FROM filtered_table WHERE condition = true);

-- Method 2: EXISTS (correlated subquery)
SELECT * FROM large_table lt
WHERE EXISTS (SELECT 1 FROM filtered_table ft WHERE ft.id = lt.id AND condition = true);

-- Method 3: JOIN (often most efficient)
SELECT lt.* FROM large_table lt
INNER JOIN filtered_table ft ON lt.id = ft.id
WHERE ft.condition = true;

-- Check execution plans
EXPLAIN SELECT * FROM current_employees WHERE department IN ('IT', 'HR');
EXPLAIN SELECT * FROM current_employees e WHERE EXISTS (
    SELECT 1 FROM departments d WHERE d.name = e.department AND d.name IN ('IT', 'HR')
);
Key Takeaways
Set Operations: Use UNION for distinct combinations, UNION ALL for all rows including duplicates

INTERSECT/EXCEPT: Available in MySQL 8.0+, use JOIN alternatives in older versions

IN vs EXISTS: IN better for small lists, EXISTS better for correlated subqueries and large datasets

Views: Virtual tables for simplifying complex queries and security

Temporary Tables: Session-specific storage for intermediate results

Indexes: Crucial for performance on frequently queried columns, but use judiciously