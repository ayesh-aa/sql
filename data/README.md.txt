


# Database Analysis

## Overview

The purpose of this project is to analyze the employee data . This involves creating a database schema, importing data from CSV files, and performing various SQL queries to extract meaningful insights from the data.

## Table of Contents

- [Overview](#overview)
- [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)
- [Files](#files)
- [Instructions](#instructions)
  - [Setup](#setup)
  - [Create Database and Tables](#create-database-and-tables)
  - [Import Data](#import-data)
  - [Run Data Analysis Queries](#run-data-analysis-queries)
- [Queries](#queries)
- [Contributors](#contributors)

## Entity Relationship Diagram (ERD)

![ERD](path/to/your/erd.png)

## Files

- `schema.sql`: Contains the SQL commands to create the database schema.
- `data_import.sql`: Contains the SQL commands to import data from the CSV files into the database.
- `data_analysis.sql`: Contains the SQL queries to perform data analysis on the imported data.
- `departments.csv`, `dept_emp.csv`, `dept_emp_corrected.csv`, `dept_manager.csv`, `employees.csv`, `salaries.csv`, `titles.csv`: The CSV files containing the raw data.

## Instructions

### Setup

1. **Clone the repository**:
    ```sh
    git clone https://github.com/ayesh-aa/sql.git
    cd sql/EmployeeSQL
    ```

### Create Database and Tables

1. Open pgAdmin and connect to your PostgreSQL database.
2. Open the Query Tool and run the commands in `schema.sql` to create the tables.

### Import Data

1. Use the Import/Export Data feature in pgAdmin to import data from the CSV files into the corresponding tables.
2. Example commands (replace `path/to` with actual paths):
    ```sql
    COPY departments(dept_no, dept_name) FROM 'C:/path/to/departments.csv' DELIMITER ',' CSV HEADER;
    COPY employees(emp_no, emp_title_id, birth_date, first_name, last_name, sex, hire_date) FROM 'C:/path/to/employees.csv' DELIMITER ',' CSV HEADER;
    COPY dept_emp(emp_no, dept_no, from_date, to_date) FROM 'C:/path/to/dept_emp_corrected.csv' DELIMITER ',' CSV HEADER;
    COPY dept_manager(emp_no, dept_no, from_date, to_date) FROM 'C:/path/to/dept_manager.csv' DELIMITER ',' CSV HEADER;
    COPY salaries(emp_no, salary, from_date, to_date) FROM 'C:/path/to/salaries.csv' DELIMITER ',' CSV HEADER;
    COPY titles(emp_no, title, from_date, to_date) FROM 'C:/path/to/titles.csv' DELIMITER ',' CSV HEADER;
    ```

### Run Data Analysis Queries

1. Open the Query Tool in pgAdmin and run the queries in `data_analysis.sql` to perform data analysis.

## Queries

### 1. List the employee number, last name, first name, sex, and salary of each employee

```sql
SELECT e.emp_no, e.last_name, e.first_name, e.sex, s.salary
FROM employees e
JOIN salaries s ON e.emp_no = s.emp_no;
2. List the first name, last name, and hire date for the employees who were hired in 1986
sql
Copy code
SELECT first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1986-12-31';
3. List the manager of each department along with their department number, department name, employee number, last name, and first name
sql
Copy code
SELECT d.dept_no, d.dept_name, dm.emp_no, e.last_name, e.first_name
FROM dept_manager dm
JOIN departments d ON dm.dept_no = d.dept_no
JOIN employees e ON dm.emp_no = e.emp_no;
4. List the department number for each employee along with that employee’s employee number, last name, first name, and department name
sql
Copy code
SELECT de.dept_no, e.emp_no, e.last_name, e.first_name, d.dept_name
FROM dept_emp de
JOIN employees e ON de.emp_no = e.emp_no
JOIN departments d ON de.dept_no = d.dept_no;
5. List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B
sql
Copy code
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%';
6. List each employee in the Sales department, including their employee number, last name, and first name
sql
Copy code
SELECT e.emp_no, e.last_name, e.first_name
FROM dept_emp de
JOIN employees e ON de.emp_no = e.emp_no
JOIN departments d ON de.dept_no = d.dept_no
WHERE d.dept_name = 'Sales';
7. List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name
sql
Copy code
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM dept_emp de
JOIN employees e ON de.emp_no = e.emp_no
JOIN departments d ON de.dept_no = d.dept_no
WHERE d.dept_name IN ('Sales', 'Development');
``
-- 8. List the frequency counts, in descending order, of all the employee last names
SELECT last_name AS "Surname", COUNT(last_name) AS "Number of Staff with the same Surname"
FROM employees
GROUP BY last_name
ORDER BY "Number of Staff with the same Surname" DESC;