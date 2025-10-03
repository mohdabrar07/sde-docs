# Day 1: Introduction to SQL and MySQL - Setup & Basics

## Table of Contents
1. [What is SQL?](#what-is-sql)
2. [What is MySQL?](#what-is-mysql)
3. [Installing MySQL](#installing-mysql)
4. [Understanding Databases](#understanding-databases)
5. [MySQL Command Line and Workbench](#mysql-tools)
6. [Your First Database](#your-first-database)
7. [Basic SQL Syntax Rules](#basic-sql-syntax)
8. [Practice Exercises](#practice-exercises)
9. [Summary](#summary)

---

## What is SQL?

**SQL (Structured Query Language)** is a standardized programming language specifically designed for managing and manipulating relational databases.

### Key Points:
- SQL is **declarative** - you describe what you want, not how to get it
- Used for **CRUD operations**: Create, Read, Update, Delete
- Industry standard for relational database management
- Not case-sensitive (but conventionally written in UPPERCASE for keywords)

### What Can You Do With SQL?
- Create and manage databases
- Store, retrieve, and manipulate data
- Control user access and permissions
- Create complex queries to analyze data
- Integrate with programming languages (Python, Java, PHP, etc.)

---

## What is MySQL?

**MySQL** is an open-source Relational Database Management System (RDBMS) that uses SQL.

### Why MySQL?
- **Free and Open Source**
- **Fast and Reliable**
- **Widely Used**: Powers websites like Facebook, Twitter, YouTube
- **Cross-platform**: Works on Windows, Linux, macOS
- **Great Community Support**
- **Perfect for Learning**: Easy to set up and use

### MySQL vs Other Databases
| Feature | MySQL | PostgreSQL | SQLite | Oracle |
|---------|-------|------------|--------|--------|
| Cost | Free | Free | Free | Paid |
| Complexity | Medium | High | Low | High |
| Best For | Web Apps | Complex Queries | Small Apps | Enterprise |

---

## Installing MySQL

### Windows Installation

1. **Download MySQL Installer**
   - Go to: https://dev.mysql.com/downloads/installer/
   - Choose "MySQL Installer for Windows"
   - Download the larger "Community" version

2. **Run the Installer**
   - Choose "Developer Default" setup type
   - Click "Execute" to download all components
   - Configure MySQL Server:
     - Choose "Standalone MySQL Server"
     - Keep default port: 3306
     - Set root password (remember this!)

3. **Verify Installation**
   ```bash
   mysql --version
   ```

### macOS Installation

```bash
# Using Homebrew
brew install mysql

# Start MySQL service
brew services start mysql

# Secure installation
mysql_secure_installation
```

### Linux (Ubuntu/Debian) Installation

```bash
# Update package index
sudo apt update

# Install MySQL Server
sudo apt install mysql-server

# Start MySQL service
sudo systemctl start mysql

# Secure installation
sudo mysql_secure_installation
```

### Accessing MySQL

```bash
# Login as root user
mysql -u root -p
# Enter your password when prompted
```

---

## Understanding Databases

### What is a Database?

A database is an organized collection of structured data stored electronically. Think of it as a digital filing cabinet.

### Relational Database Concepts

**1. Tables**: Store data in rows and columns (like Excel spreadsheets)

**2. Rows (Records)**: Individual entries in a table

**3. Columns (Fields)**: Attributes or properties of the data

**4. Primary Key**: Unique identifier for each row

**5. Relationships**: Connections between tables

### Example Visualization

```
DATABASE: school
│
├── TABLE: students
│   ├── student_id (Primary Key)
│   ├── first_name
│   ├── last_name
│   └── email
│
└── TABLE: courses
    ├── course_id (Primary Key)
    ├── course_name
    └── credits
```

---

## MySQL Tools

### 1. MySQL Command Line Client

The terminal-based interface for executing SQL commands.

```bash
# Login
mysql -u root -p

# Show databases
SHOW DATABASES;

# Exit
exit;
```

### 2. MySQL Workbench

A visual GUI tool for database design and management.

**Features:**
- Visual database design
- SQL editor with syntax highlighting
- Query execution and results viewing
- Database administration tools

**Download**: https://dev.mysql.com/downloads/workbench/

### 3. Command Line Shortcuts

```bash
# Login to specific database
mysql -u root -p database_name

# Execute SQL file
mysql -u root -p database_name < script.sql

# Export database
mysqldump -u root -p database_name > backup.sql
```

---

## Your First Database

### Step 1: Login to MySQL

```sql
mysql -u root -p
```

### Step 2: View Existing Databases

```sql
SHOW DATABASES;
```

**Output:**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

### Step 3: Create Your First Database

```sql
CREATE DATABASE learning_sql;
```

### Step 4: Select (Use) the Database

```sql
USE learning_sql;
```

### Step 5: Verify Current Database

```sql
SELECT DATABASE();
```

### Step 6: Create Your First Table

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    enrollment_date DATE,
    age INT
);
```

**Breaking Down the Command:**
- `CREATE TABLE students` - Creates a new table named "students"
- `student_id INT` - Integer column for student ID
- `PRIMARY KEY` - Uniquely identifies each row
- `AUTO_INCREMENT` - Automatically generates sequential numbers
- `VARCHAR(50)` - Variable character string, max 50 characters
- `NOT NULL` - Field cannot be empty
- `UNIQUE` - No duplicate values allowed
- `DATE` - Stores date values

### Step 7: View Table Structure

```sql
DESCRIBE students;
-- or
DESC students;
```

**Output:**
```
+-----------------+--------------+------+-----+---------+----------------+
| Field           | Type         | Null | Key | Default | Extra          |
+-----------------+--------------+------+-----+---------+----------------+
| student_id      | int          | NO   | PRI | NULL    | auto_increment |
| first_name      | varchar(50)  | NO   |     | NULL    |                |
| last_name       | varchar(50)  | NO   |     | NULL    |                |
| email           | varchar(100) | YES  | UNI | NULL    |                |
| enrollment_date | date         | YES  |     | NULL    |                |
| age             | int          | YES  |     | NULL    |                |
+-----------------+--------------+------+-----+---------+----------------+
```

### Step 8: Insert Data

```sql
INSERT INTO students (first_name, last_name, email, enrollment_date, age)
VALUES ('John', 'Doe', 'john.doe@email.com', '2025-01-15', 20);

INSERT INTO students (first_name, last_name, email, enrollment_date, age)
VALUES 
    ('Jane', 'Smith', 'jane.smith@email.com', '2025-01-16', 22),
    ('Mike', 'Johnson', 'mike.j@email.com', '2025-01-17', 21),
    ('Sarah', 'Williams', 'sarah.w@email.com', '2025-01-18', 23);
```

### Step 9: View Your Data

```sql
SELECT * FROM students;
```

**Output:**
```
+------------+------------+-----------+----------------------+-----------------+------+
| student_id | first_name | last_name | email                | enrollment_date | age  |
+------------+------------+-----------+----------------------+-----------------+------+
|          1 | John       | Doe       | john.doe@email.com   | 2025-01-15      |   20 |
|          2 | Jane       | Smith     | jane.smith@email.com | 2025-01-16      |   22 |
|          3 | Mike       | Johnson   | mike.j@email.com     | 2025-01-17      |   21 |
|          4 | Sarah      | Williams  | sarah.w@email.com    | 2025-01-18      |   23 |
+------------+------------+-----------+----------------------+-----------------+------+
```

---

## Basic SQL Syntax Rules

### 1. Statement Termination
Always end SQL statements with a semicolon (`;`)

```sql
SELECT * FROM students;
```

### 2. Case Insensitivity
SQL keywords are not case-sensitive, but convention uses UPPERCASE for keywords:

```sql
-- All valid, but first is preferred
SELECT * FROM students;
select * from students;
SeLeCt * FrOm students;
```

### 3. Comments

```sql
-- This is a single-line comment

/* This is a
   multi-line
   comment */

SELECT * FROM students; -- inline comment
```

### 4. String Values
Always use single quotes for string values:

```sql
-- Correct
INSERT INTO students (first_name) VALUES ('John');

-- Incorrect (will cause error)
INSERT INTO students (first_name) VALUES ("John");
```

### 5. Whitespace
SQL ignores extra whitespace, so you can format for readability:

```sql
-- Both are equivalent
SELECT * FROM students;

SELECT *
FROM students;
```

### 6. Naming Conventions
- Use lowercase with underscores: `student_id`, `first_name`
- Avoid spaces in names
- Use descriptive names
- Table names: plural (students, courses)
- Column names: singular (student_id, not students_id)

---

## Practice Exercises

### Exercise 1: Create a Database
Create a database named `company_db` and use it.

<details>
<summary>Solution</summary>

```sql
CREATE DATABASE company_db;
USE company_db;
```
</details>

### Exercise 2: Create an Employees Table
Create a table named `employees` with the following columns:
- employee_id (INT, PRIMARY KEY, AUTO_INCREMENT)
- first_name (VARCHAR 50, NOT NULL)
- last_name (VARCHAR 50, NOT NULL)
- department (VARCHAR 50)
- salary (DECIMAL 10,2)
- hire_date (DATE)

<details>
<summary>Solution</summary>

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    department VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE
);
```
</details>

### Exercise 3: Insert Multiple Records
Insert at least 5 employees into the table.

<details>
<summary>Solution</summary>

```sql
INSERT INTO employees (first_name, last_name, department, salary, hire_date)
VALUES 
    ('Alice', 'Johnson', 'Engineering', 75000.00, '2023-03-15'),
    ('Bob', 'Smith', 'Marketing', 65000.00, '2023-05-20'),
    ('Carol', 'Davis', 'Engineering', 80000.00, '2022-11-10'),
    ('David', 'Wilson', 'HR', 60000.00, '2024-01-05'),
    ('Emma', 'Brown', 'Sales', 70000.00, '2023-08-22');
```
</details>

### Exercise 4: View All Data
Retrieve all records from the employees table.

<details>
<summary>Solution</summary>

```sql
SELECT * FROM employees;
```
</details>

### Exercise 5: Check Table Structure
Display the structure of the employees table.

<details>
<summary>Solution</summary>

```sql
DESCRIBE employees;
-- or
DESC employees;
```
</details>

---

## Summary

### What You Learned Today:

✅ **Concepts**
- What SQL and MySQL are
- Relational database fundamentals
- Tables, rows, columns, and keys

✅ **Technical Skills**
- Installing MySQL
- Logging into MySQL
- Creating databases
- Creating tables with various data types
- Inserting data into tables
- Viewing data with SELECT

✅ **Commands Learned**
```sql
SHOW DATABASES;
CREATE DATABASE database_name;
USE database_name;
CREATE TABLE table_name (...);
DESCRIBE table_name;
INSERT INTO table_name (...) VALUES (...);
SELECT * FROM table_name;
```

### Key Takeaways:
1. SQL is the language; MySQL is the database system
2. Databases contain tables, tables contain rows and columns
3. Always terminate SQL statements with semicolon
4. PRIMARY KEY uniquely identifies each record
5. AUTO_INCREMENT automatically generates sequential numbers

### Common Beginner Mistakes to Avoid:
- Forgetting the semicolon at the end of statements
- Using double quotes instead of single quotes for strings
- Not selecting a database before creating tables (use `USE database_name;`)
- Forgetting to specify NOT NULL for required fields

---

## Next Steps (Day 2 Preview)

Tomorrow you'll learn:
- Data types in depth
- SELECT queries with filtering (WHERE clause)
- Sorting results (ORDER BY)
- Limiting results
- Basic aggregate functions (COUNT, SUM, AVG)

### Homework:
1. Create a database for a library system
2. Create a `books` table with relevant columns
3. Insert at least 10 books
4. Practice describing and viewing your data

---

## Additional Resources

- [MySQL Official Documentation](https://dev.mysql.com/doc/)
- [MySQL Tutorial](https://www.mysqltutorial.org/)
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

---

**Happy Learning! 🚀**

*Remember: Practice is key. Don't just read—type out every command yourself!*