# 🗄️ SQL Portfolio — Muhammad Ammar Saleem

> Self-learned SQL developer with hands-on experience in database design, data insertion, and querying.  
> This portfolio showcases real-world themed databases built from scratch using MySQL.

---

## 👤 About Me

- 🎓 Pre-Engineering Graduate — AKHSS, Karachi
- 📊 Aspiring **Data Analyst → Data Scientist**
- 🛠️ Skills: **SQL • Excel • Java** | Currently Learning: **Tableau**
- 🔗 GitHub: [github.com/m2ammar](https://github.com/m2ammar)

---

## 📁 Databases in This Portfolio

| # | Database | Theme | Key Concepts |
|---|----------|-------|--------------|
| 1 | [E-Commerce](https://github.com/m2ammar/E-Commerce-database-mysql) | Online Store | Joins, Aggregates, Subqueries |
| 2 | [Bank](https://github.com/m2ammar/Bank-database-mysql) | Banking System | Subqueries, ENUMs, Multi-table Joins |
| 3 | [Organisation](https://github.com/m2ammar/Organisation-database-mysql) | Company/HR | GROUP BY, HAVING, Nested Subqueries |
| 4 | [College](https://github.com/m2ammar/College-database-mysql) | College System | ALTER, UPDATE, DDL Commands |
---

## 1. 🛒 E-Commerce Database

A fully structured e-commerce system with **8 related tables** covering the complete order lifecycle.

### Tables
| Table | Description |
|-------|-------------|
| `Customers` | Customer personal and contact info |
| `Categories` | Product categories |
| `Products` | Product catalog with pricing and stock |
| `Orders` | Customer orders with status tracking |
| `Order_Items` | Line items within each order |
| `Payments` | Payment records with method and status |
| `Reviews` | Customer product reviews and ratings |
| `Shipping` | Shipping and delivery tracking |

### SQL Concepts Covered
-  Database & table creation with **Primary and Foreign Keys**
-  **INNER JOIN** across multiple tables
-  **Aliases** (`AS`) for cleaner queries
-  **Aggregate functions** — `SUM()`, `COUNT()`, `MAX()`
-  **GROUP BY** and **HAVING**
-  `CONCAT()` for full name formatting
-  **ORDER BY DESC** and **LIMIT**
-  Filtering with **WHERE** and **LIKE**

### Sample Query
```sql
-- Top rated product
SELECT p.product_name, MAX(r.rating) AS Highest_Rating
FROM Reviews r
JOIN Products p ON p.product_id = r.product_id
GROUP BY p.product_name
ORDER BY Highest_Rating DESC
LIMIT 1;
```

---

## 2. 🏦 Bank Database

A banking system simulating real-world accounts, transactions, loans, and employees.

### Tables
| Table | Description |
|-------|-------------|
| `Customers` | Bank customers with city and contact |
| `Accounts` | Savings and Current accounts with balance |
| `Transactions` | Credit and Debit transaction history |
| `Loans` | Loan records with interest rates and status |
| `Employees` | Bank staff with roles and salaries |

### SQL Concepts Covered
-  **ENUM** data types for controlled values
-  **Multi-table INNER JOINs** (3 tables at once)
-  **Subqueries** — employees earning above average salary
-  **Aggregate functions** — `SUM()`, `MAX()`, `AVG()`
-  **GROUP BY** with **ORDER BY DESC**
-  Conditional filtering with **WHERE**
-  Balance calculation using arithmetic in queries

### Sample Query
```sql
-- Employees earning above average salary
SELECT name, salary
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
```

---

## 3. 🏢 Organisation Database

A company database with departments, employees, projects, teachers, and students.

### Tables
| Table | Description |
|-------|-------------|
| `employees` | Staff with department, salary, and city |
| `department` | Department listing |
| `projects` | Projects assigned to employees with budgets |
| `worker` | Worker salary and department data |
| `TEACHER` | Teachers linked to departments |
| `DEPT` | Department reference for teachers |
| `student` / `course` | Student and course enrollment |

### SQL Concepts Covered
-  **ON DELETE CASCADE / ON UPDATE CASCADE**
-  **GROUP BY** with **AVG()** and **MAX()**
-  **Nested Subqueries** inside HAVING clause
-  **INNER JOIN** with foreign key relationships
-  Salary analysis across departments

### Sample Query
```sql
-- Department with the highest average salary
SELECT department, AVG(salary) AS avg_salary
FROM worker
GROUP BY department
HAVING AVG(salary) = (
    SELECT MAX(avg_salary)
    FROM (
        SELECT AVG(salary) AS avg_salary
        FROM worker
        GROUP BY department
    ) AS dept_avg
);
```

---

## 4. 🎓 College Database

A college-level database focused on DDL (Data Definition Language) commands alongside DML.

### Tables
| Table | Description |
|-------|-------------|
| `Employee` | Basic employee name records |
| `Student` | Students with age |
| `workers` | Staff with department, city, and salary |

### SQL Concepts Covered
-  **ALTER TABLE** — rename, drop column, add column
-  **UPDATE** with conditional **WHERE**
-  **IN** operator for filtering multiple values
-  **COUNT()** with **GROUP BY**
-  Salary increment using arithmetic in UPDATE
-  **UNIQUE** constraint on columns

### Sample Query
```sql
-- Give 10% salary raise to all Karachi employees
UPDATE workers
SET Salary = Salary + (Salary * 0.10)
WHERE City IN ('Karachi');
```

---

## 🛠️ Tools & Environment

- **Database:** MySQL
- **Editor:** MySQL Workbench
- **Version Control:** Git & GitHub

---

## 📈 Skills Summary

| Skill | Level |
|-------|-------|
| Table Design & Foreign Keys |  Comfortable |
| Data Insertion (INSERT) |  Comfortable |
| SELECT with WHERE / ORDER BY |  Comfortable |
| JOINs (INNER JOIN, Aliases) |  Comfortable |
| Aggregate Functions |  Comfortable |
| GROUP BY / HAVING |  Comfortable |
| Subqueries |  Comfortable |
| DDL (ALTER, UPDATE, DROP) |  Comfortable |

---

## 📬 Contact

- 📧 ma9731501@gmail.com
- 🔗 [github.com/m2ammar](https://github.com/m2ammar)
