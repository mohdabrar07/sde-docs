# Day 05 - DML & DDL Operations and Transactions

## 📋 Table of Contents
- [Introduction](#introduction)
- [DDL (Data Definition Language)](#ddl-data-definition-language)
- [DML (Data Manipulation Language)](#dml-data-manipulation-language)
- [Transactions](#transactions)
- [Practice Exercises](#practice-exercises)
- [Common Interview Questions](#common-interview-questions)

---

## Introduction

Understanding DML and DDL operations is fundamental for any database developer. These commands allow you to define database structures and manipulate data within them.

**Key Differences:**
- **DDL (Data Definition Language)**: Defines and modifies database structure
- **DML (Data Manipulation Language)**: Manipulates data within database objects
- **Transactions**: Ensure data integrity through ACID properties

---

## DDL (Data Definition Language)

DDL commands are used to define, modify, and remove database structures.

### 1. CREATE Statement

Used to create new database objects like tables, views, indexes, etc.

#### Basic Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);
```

#### Example - Creating an Employee Table:
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(15),
    hire_date DATE NOT NULL,
    job_title VARCHAR(50),
    salary DECIMAL(10, 2),
    department_id INT,
    manager_id INT,
    status VARCHAR(20) DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Creating Table with Foreign Key:
```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(100) NOT NULL,
    location VARCHAR(100)
);

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

### 2. ALTER Statement

Used to modify existing database structures.

#### Adding a Column:
```sql
ALTER TABLE employees
ADD COLUMN date_of_birth DATE;
```

#### Modifying a Column:
```sql
-- Change column datatype
ALTER TABLE employees
MODIFY COLUMN salary DECIMAL(12, 2);

-- Change column name
ALTER TABLE employees
CHANGE COLUMN phone_number contact_number VARCHAR(15);
```

#### Dropping a Column:
```sql
ALTER TABLE employees
DROP COLUMN date_of_birth;
```

#### Adding Constraints:
```sql
-- Add PRIMARY KEY
ALTER TABLE employees
ADD PRIMARY KEY (employee_id);

-- Add FOREIGN KEY
ALTER TABLE employees
ADD CONSTRAINT fk_department
FOREIGN KEY (department_id) REFERENCES departments(department_id);

-- Add UNIQUE constraint
ALTER TABLE employees
ADD CONSTRAINT unique_email UNIQUE (email);

-- Add CHECK constraint
ALTER TABLE employees
ADD CONSTRAINT check_salary CHECK (salary > 0);
```

#### Dropping Constraints:
```sql
ALTER TABLE employees
DROP CONSTRAINT fk_department;

ALTER TABLE employees
DROP INDEX unique_email;
```

### 3. DROP Statement

Used to delete database objects permanently.

#### Drop Table:
```sql
DROP TABLE employees;
```

#### Drop Table if Exists:
```sql
DROP TABLE IF EXISTS employees;
```

#### Drop Database:
```sql
DROP DATABASE company_db;
```

### 4. TRUNCATE Statement

Removes all records from a table but keeps the structure.

```sql
TRUNCATE TABLE employees;
```

**Difference between TRUNCATE and DELETE:**
- `TRUNCATE` is faster and uses less transaction log space
- `TRUNCATE` cannot be rolled back in most databases
- `TRUNCATE` resets auto-increment counters
- `DELETE` can have WHERE clause, TRUNCATE cannot

### 5. RENAME Statement

Renames database objects.

```sql
-- Rename table
RENAME TABLE employees TO staff_members;

-- Alternative syntax
ALTER TABLE employees RENAME TO staff_members;
```

---

## DML (Data Manipulation Language)

DML commands are used to manipulate data within existing database structures.

### 1. INSERT Statement

Used to add new records to a table.

#### Single Row Insert:
```sql
INSERT INTO employees (first_name, last_name, email, hire_date, job_title, salary)
VALUES ('John', 'Doe', 'john.doe@company.com', '2024-01-15', 'Software Engineer', 75000.00);
```

#### Multiple Row Insert:
```sql
INSERT INTO employees (first_name, last_name, email, hire_date, job_title, salary, department_id)
VALUES 
    ('Jane', 'Smith', 'jane.smith@company.com', '2024-02-01', 'Data Analyst', 65000.00, 2),
    ('Mike', 'Johnson', 'mike.j@company.com', '2024-02-15', 'HR Manager', 70000.00, 3),
    ('Sarah', 'Williams', 'sarah.w@company.com', '2024-03-01', 'Marketing Specialist', 60000.00, 4),
    ('David', 'Brown', 'david.b@company.com', '2024-03-10', 'Sales Executive', 55000.00, 5);
```

#### Insert with SELECT:
```sql
-- Copy data from one table to another
INSERT INTO employees_backup
SELECT * FROM employees WHERE department_id = 2;
```

#### Insert Default Values:
```sql
INSERT INTO employees (first_name, last_name, email, hire_date)
VALUES ('Tom', 'Anderson', 'tom.a@company.com', CURDATE());
-- Other columns will use default values or NULL
```

### 2. UPDATE Statement

Used to modify existing records in a table.

#### Basic Update:
```sql
UPDATE employees
SET salary = 80000.00
WHERE employee_id = 1;
```

#### Update Multiple Columns:
```sql
UPDATE employees
SET 
    salary = salary * 1.10,  -- 10% salary increase
    job_title = 'Senior Software Engineer'
WHERE employee_id = 1;
```

#### Update with Condition:
```sql
-- Update salary for employees earning less than 5000
UPDATE employees
SET salary = salary + 5000
WHERE salary < 50000;
```

#### Update Using Subquery:
```sql
UPDATE employees
SET salary = (
    SELECT AVG(salary) 
    FROM employees 
    WHERE department_id = 2
)
WHERE department_id = 2 AND salary < 60000;
```

#### Update with JOIN:
```sql
UPDATE employees e
JOIN departments d ON e.department_id = d.department_id
SET e.salary = e.salary * 1.05
WHERE d.department_name = 'IT';
```

#### Update All Rows (Use with Caution):
```sql
-- Increases all salaries by 5%
UPDATE employees
SET salary = salary * 1.05;
```

### 3. DELETE Statement

Used to remove records from a table.

#### Delete Specific Record:
```sql
DELETE FROM employees
WHERE employee_id = 10;
```

#### Delete with Condition:
```sql
-- Delete employees who have left the company
DELETE FROM employees
WHERE status = 'Inactive' OR status = 'Terminated';
```

#### Delete Using Multiple Conditions:
```sql
DELETE FROM employees
WHERE salary < 30000 
  AND hire_date < '2020-01-01'
  AND status = 'Inactive';
```

#### Delete Using Subquery:
```sql
DELETE FROM employees
WHERE department_id IN (
    SELECT department_id 
    FROM departments 
    WHERE location = 'Remote'
);
```

#### Delete with JOIN:
```sql
DELETE e
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name = 'Temporary Projects';
```

#### Delete All Rows (Use with Extreme Caution):
```sql
DELETE FROM employees;
-- This deletes all records but keeps the table structure
```

---

## Transactions

Transactions ensure data integrity and consistency through ACID properties (Atomicity, Consistency, Isolation, Durability).

### ACID Properties

1. **Atomicity**: All operations in a transaction succeed or all fail
2. **Consistency**: Database remains in a valid state before and after transaction
3. **Isolation**: Concurrent transactions don't interfere with each other
4. **Durability**: Committed changes are permanent

### Transaction Commands

#### 1. START TRANSACTION / BEGIN

Initiates a new transaction.

```sql
START TRANSACTION;
-- or
BEGIN;
```

#### 2. COMMIT

Saves all changes made in the current transaction permanently.

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 1000 WHERE account_id = 101;
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 102;

COMMIT;  -- Both updates are saved permanently
```

#### 3. ROLLBACK

Undoes all changes made in the current transaction.

```sql
START TRANSACTION;

UPDATE employees SET salary = salary * 2;  -- Accidental update

ROLLBACK;  -- Undo all changes, salaries remain unchanged
```

#### 4. SAVEPOINT

Creates a point within a transaction to which you can rollback.

```sql
START TRANSACTION;

INSERT INTO employees (first_name, last_name, email, hire_date)
VALUES ('Alex', 'Turner', 'alex.t@company.com', CURDATE());

SAVEPOINT after_insert;

UPDATE employees SET salary = 60000 WHERE email = 'alex.t@company.com';

SAVEPOINT after_update;

DELETE FROM employees WHERE email = 'alex.t@company.com';

-- Rollback to specific savepoint
ROLLBACK TO SAVEPOINT after_update;  -- DELETE is undone, INSERT and UPDATE remain

COMMIT;  -- Saves INSERT and UPDATE
```

#### 5. RELEASE SAVEPOINT

Removes a savepoint.

```sql
RELEASE SAVEPOINT after_insert;
```

### Transaction Examples

#### Example 1: Bank Transfer
```sql
START TRANSACTION;

-- Deduct from sender
UPDATE accounts 
SET balance = balance - 500 
WHERE account_id = 1001;

-- Check if sufficient balance
SELECT balance INTO @sender_balance 
FROM accounts 
WHERE account_id = 1001;

IF @sender_balance < 0 THEN
    ROLLBACK;
    SELECT 'Insufficient balance' AS message;
ELSE
    -- Add to receiver
    UPDATE accounts 
    SET balance = balance + 500 
    WHERE account_id = 1002;
    
    COMMIT;
    SELECT 'Transfer successful' AS message;
END IF;
```

#### Example 2: Order Processing
```sql
START TRANSACTION;

-- Insert order
INSERT INTO orders (customer_id, order_date, total_amount)
VALUES (105, CURDATE(), 1500.00);

SET @order_id = LAST_INSERT_ID();

-- Insert order items
INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES 
    (@order_id, 201, 2, 500.00),
    (@order_id, 202, 1, 500.00);

-- Update inventory
UPDATE products SET stock = stock - 2 WHERE product_id = 201;
UPDATE products SET stock = stock - 1 WHERE product_id = 202;

-- If everything is successful
COMMIT;
```

#### Example 3: Error Handling
```sql
START TRANSACTION;

-- Attempt to insert employee
INSERT INTO employees (first_name, last_name, email, hire_date, department_id)
VALUES ('Chris', 'Evans', 'chris.e@company.com', CURDATE(), 99);

-- Check if department exists
SELECT COUNT(*) INTO @dept_count 
FROM departments 
WHERE department_id = 99;

IF @dept_count = 0 THEN
    ROLLBACK;
    SELECT 'Error: Department does not exist' AS message;
ELSE
    COMMIT;
    SELECT 'Employee added successfully' AS message;
END IF;
```

### Auto-commit Mode

By default, MySQL operates in auto-commit mode where each statement is automatically committed.

```sql
-- Disable auto-commit
SET autocommit = 0;

-- Enable auto-commit
SET autocommit = 1;
```

---

## Practice Exercises

### Exercise 1: Create Table and Insert Records

```sql
-- Create employees table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(15),
    hire_date DATE NOT NULL,
    job_title VARCHAR(50),
    salary DECIMAL(10, 2),
    department_id INT,
    status VARCHAR(20) DEFAULT 'Active'
);

-- Insert 5 sample records
INSERT INTO employees (first_name, last_name, email, phone_number, hire_date, job_title, salary, department_id, status)
VALUES 
    ('John', 'Doe', 'john.doe@company.com', '555-1001', '2023-01-15', 'Software Engineer', 75000.00, 1, 'Active'),
    ('Jane', 'Smith', 'jane.smith@company.com', '555-1002', '2023-03-20', 'Data Analyst', 45000.00, 2, 'Active'),
    ('Mike', 'Johnson', 'mike.j@company.com', '555-1003', '2022-06-10', 'HR Manager', 65000.00, 3, 'Active'),
    ('Sarah', 'Williams', 'sarah.w@company.com', '555-1004', '2023-08-05', 'Marketing Specialist', 38000.00, 4, 'Active'),
    ('David', 'Brown', 'david.b@company.com', '555-1005', '2021-11-22', 'Sales Executive', 42000.00, 5, 'Inactive');

-- Verify insertion
SELECT * FROM employees;
```

### Exercise 2: Update Records with Salary < 50000

```sql
-- Update employees with salary less than 50000
START TRANSACTION;

UPDATE employees
SET salary = salary + 10000  -- Give $10,000 raise
WHERE salary < 50000;

-- Verify the update
SELECT 
    employee_id,
    CONCAT(first_name, ' ', last_name) AS full_name,
    job_title,
    salary
FROM employees
WHERE salary < 60000;  -- Check updated salaries

COMMIT;
```

**Alternative approach with percentage increase:**
```sql
UPDATE employees
SET 
    salary = salary * 1.15,  -- 15% increase
    status = 'Active'
WHERE salary < 50000 AND status = 'Active';
```

### Exercise 3: Delete Employees Who Have Left

```sql
-- First, check which employees will be deleted
SELECT 
    employee_id,
    CONCAT(first_name, ' ', last_name) AS full_name,
    email,
    status
FROM employees
WHERE status IN ('Inactive', 'Terminated', 'Left');

-- Delete employees who have left the company
START TRANSACTION;

DELETE FROM employees
WHERE status IN ('Inactive', 'Terminated', 'Left');

-- Verify deletion
SELECT COUNT(*) AS remaining_employees FROM employees;
SELECT * FROM employees;

COMMIT;
```

### Exercise 4: Combined Operations with Transactions

```sql
START TRANSACTION;

-- Create a backup before making changes
CREATE TABLE employees_backup AS
SELECT * FROM employees;

SAVEPOINT backup_created;

-- Update salaries
UPDATE employees
SET salary = salary * 1.10
WHERE department_id = 1;

SAVEPOINT salary_updated;

-- Delete inactive employees
DELETE FROM employees
WHERE status = 'Inactive';

-- If something goes wrong, rollback to a savepoint
-- ROLLBACK TO SAVEPOINT salary_updated;

-- If everything is correct, commit
COMMIT;

-- Verify changes
SELECT * FROM employees;
SELECT * FROM employees_backup;
```

### Exercise 5: Complex Transaction with Error Handling

```sql
DELIMITER //

CREATE PROCEDURE transfer_employee_department(
    IN emp_id INT,
    IN new_dept_id INT
)
BEGIN
    DECLARE dept_exists INT;
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SELECT 'Error occurred, transaction rolled back' AS message;
    END;
    
    START TRANSACTION;
    
    -- Check if department exists
    SELECT COUNT(*) INTO dept_exists
    FROM departments
    WHERE department_id = new_dept_id;
    
    IF dept_exists = 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Department does not exist';
    END IF;
    
    -- Update employee department
    UPDATE employees
    SET 
        department_id = new_dept_id,
        status = 'Active'
    WHERE employee_id = emp_id;
    
    -- Log the transfer
    INSERT INTO employee_transfers (employee_id, new_department_id, transfer_date)
    VALUES (emp_id, new_dept_id, CURDATE());
    
    COMMIT;
    SELECT 'Transfer successful' AS message;
END //

DELIMITER ;

-- Call the procedure
CALL transfer_employee_department(1, 3);
```

---

## Common Interview Questions

### 1. What is the difference between DELETE, TRUNCATE, and DROP?

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| Type | DML | DDL | DDL |
| Removes | Rows | All rows | Table structure and data |
| WHERE clause | Yes | No | No |
| Rollback | Yes | No (in most DBs) | No |
| Speed | Slower | Faster | Fastest |
| Triggers | Activated | Not activated | Not applicable |
| Auto-increment | Preserved | Reset | Removed |

### 2. What is the difference between COMMIT and ROLLBACK?

- **COMMIT**: Permanently saves all changes made in the current transaction
- **ROLLBACK**: Undoes all changes made in the current transaction

### 3. What are SAVEPOINT and why use them?

SAVEPOINT creates a marker within a transaction to which you can rollback without affecting the entire transaction. Useful for:
- Complex transactions with multiple steps
- Partial rollback scenarios
- Error recovery at specific points

### 4. Can we rollback after COMMIT?

No, once COMMIT is executed, changes are permanent and cannot be rolled back.

### 5. What is AUTO_INCREMENT?

AUTO_INCREMENT automatically generates a unique number for each new record, typically used for primary keys.

```sql
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50)
);
```

### 6. What is the difference between WHERE and HAVING?

- **WHERE**: Filters rows before grouping (used with individual rows)
- **HAVING**: Filters groups after grouping (used with aggregate functions)

```sql
-- WHERE example
SELECT * FROM employees WHERE salary > 50000;

-- HAVING example
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 60000;
```

### 7. How do you add a column to an existing table?

```sql
ALTER TABLE employees
ADD COLUMN date_of_birth DATE;
```

### 8. What happens if you don't use WHERE clause in UPDATE or DELETE?

All rows in the table will be affected:
- **UPDATE without WHERE**: Updates all rows
- **DELETE without WHERE**: Deletes all rows

Always use WHERE clause unless you intentionally want to affect all rows.

### 9. What is the purpose of BEGIN and START TRANSACTION?

Both are used to start a new transaction. They are synonymous in MySQL:
```sql
BEGIN;
-- or
START TRANSACTION;
```

### 10. How do you rename a table?

```sql
-- Method 1
RENAME TABLE old_table_name TO new_table_name;

-- Method 2
ALTER TABLE old_table_name RENAME TO new_table_name;
```

---

## Best Practices

### DDL Best Practices
1. Always backup data before using DROP or ALTER
2. Use `IF EXISTS` and `IF NOT EXISTS` clauses
3. Plan schema changes carefully in production
4. Document all schema changes

### DML Best Practices
1. Always use WHERE clause with UPDATE and DELETE
2. Test UPDATE/DELETE with SELECT first
3. Use transactions for related operations
4. Limit batch operations to avoid locking issues

### Transaction Best Practices
1. Keep transactions short and focused
2. Avoid user interaction during transactions
3. Handle errors properly with ROLLBACK
4. Use appropriate isolation levels
5. Always COMMIT or ROLLBACK explicitly
6. Use SAVEPOINT for complex multi-step operations

---

## Summary

- **DDL** commands define and modify database structure: CREATE, ALTER, DROP, TRUNCATE, RENAME
- **DML** commands manipulate data: INSERT, UPDATE, DELETE
- **Transactions** ensure data integrity: BEGIN, COMMIT, ROLLBACK, SAVEPOINT
- Always use WHERE clause with UPDATE and DELETE to avoid unintended changes
- Use transactions for operations that require atomicity
- Practice with real-world scenarios to master these concepts

---
