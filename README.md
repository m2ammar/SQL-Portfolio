# 🗄️ SQL Portfolio — Muhammad Ammar Saleem

> Self-learned SQL developer with hands-on experience in database design, data insertion, and querying.  
> This portfolio showcases real-world themed databases built from scratch using MySQL.

---

## 👤 About Me

- 🎓 Pre-Engineering Graduate — AKHSS, Karachi
- 📊 Aspiring **Data Analyst → Data Scientist**
- 🛠️ Skills: **SQL • Excel • Java • Tableau**
- 🔗 GitHub: [github.com/m2ammar](https://github.com/m2ammar)

---

## 📁 Databases in This Portfolio

| # | Database | Theme | Key Concepts |
|---|----------|-------|--------------|
| 1 | [Global Healthcare System](https://github.com/m2ammar/Global-Healthcare-System) | Global Health Analytics | Multi-JOIN, CTEs, Chained CTEs, CASE Logic |
| 2 | [Pakistan Financial Services](https://github.com/m2ammar/Pakistan-Financial-Services) | Banking & Finance | Joins, Aggregates, Audit Logging, Tableau |
| 3 | [Pakistan Textile Export Analysis](https://github.com/m2ammar/Pakistan-textile-export-analysis) | Real Industry Data | Joins, Tableau, FK Constraints |
| 4 | [E-Commerce](https://github.com/m2ammar/E-Commerce-database-mysql) | Online Store | Joins, Aggregates, Subqueries |
| 5 | [Bank](https://github.com/m2ammar/Bank-database-mysql) | Banking System | Subqueries, ENUMs, Multi-table Joins |
| 6 | [Organisation](https://github.com/m2ammar/Organisation-database-mysql) | Company/HR | GROUP BY, HAVING, Nested Subqueries |
| 7 | [College](https://github.com/m2ammar/College-database-mysql) | College System | ALTER, UPDATE, DDL Commands |

---

## 1. 🌍 Global Healthcare System

A 19-table relational healthcare analytics database analyzing global KPIs across 190+ countries using WHO, World Bank, and IHME inspired data.

### Tables
| Group | Tables |
|-------|--------|
| Core | Countries, Regions, Years |
| Health Metrics | Mortality Rates, Life Expectancy |
| Resources | Healthcare Spending, Infrastructure |
| Disease & Risk | Disease Burden, Risk Factors |
| Outcomes | Vaccination Coverage, Insurance, Pandemic Response |

### SQL Concepts Covered
- **Multi-table JOINs** — up to 4 tables in a single query
- **CTEs** — Common Table Expressions for readable logic
- **Chained CTEs** — multiple CTEs compared in one final output
- **CASE inside CTEs** — classify then filter by label
- **Aggregate functions** — AVG, MAX, COUNT with GROUP BY
- **Subquery filtering** — WHERE on computed columns

### Sample Query
```sql
-- Healthcare System Scorecard (4 Chained CTEs)
WITH spending_label AS (
    SELECT c.country_id, c.country_name, r.region_name,
        MAX(hs.spending_per_capi) as spending_per_capi,
        CASE
            WHEN MAX(hs.spending_per_capi) > 5000 THEN 'High'
            WHEN MAX(hs.spending_per_capi) BETWEEN 2000 AND 5000 THEN 'Medium'
            ELSE 'Low'
        END AS spending_status
    FROM countries c
    JOIN regions r ON r.region_id = c.region_id
    JOIN healthcare_spending hs ON hs.country_id = c.country_id
    JOIN years y ON y.year_id = hs.year_id
    WHERE y.year_id = 2023
    GROUP BY c.country_id, c.country_name, r.region_name
),
life_expectancy_label AS (
    SELECT c.country_id,
        CASE
            WHEN MAX(mr.life_expectancy) >= 78 THEN 'Strong'
            WHEN MAX(mr.life_expectancy) BETWEEN 65 AND 77 THEN 'Adequate'
            ELSE 'Weak'
        END AS life_expectancy_status
    FROM countries c
    JOIN mortality_rates mr ON mr.country_id = c.country_id
    JOIN years y ON y.year_id = mr.year_id
    WHERE y.year_id = 2023
    GROUP BY c.country_id
)
SELECT sl.country_name, sl.spending_status, lel.life_expectancy_status
FROM spending_label sl
JOIN life_expectancy_label lel ON sl.country_id = lel.country_id
ORDER BY sl.country_name;
```

---

## 2. 🏦 Pakistan Financial Services

A relational database simulating a Pakistani financial institution with 9 normalized tables, 500 customers, 30 branches across 7 cities, and a Tableau Executive Dashboard.

### Tables
| Table | Description |
|-------|-------------|
| `branches` | 30 branches across 7 cities and 4 regions |
| `customers` | 500 customers with income, city, and branch info |
| `accounts` | Savings, Current, and Fixed Deposit accounts |
| `employees` | 150 staff with designation and salary |
| `loans` | Loans with type, status, interest rate, tenure |
| `loan_payments` | Payment history with late fees |
| `transactions` | Deposits, withdrawals, transfers by channel |
| `credit_cards` | Visa, Mastercard, UnionPay cards |
| `audit_log` | Action log for cards, loans, accounts, customers |

### SQL Concepts Covered
- **Multi-table JOINs** across 9 normalized tables
- **Aggregate functions** — SUM, AVG, COUNT with GROUP BY
- **Subqueries** for branch and employee performance ranking
- **ENUM** types for account and loan status
- **Audit logging** — tracking changes across entities

### Sample Query
```sql
-- Top city by total loan revenue
SELECT c.city, SUM(l.loan_amount) AS total_loan_revenue
FROM customers c
JOIN loans l ON l.customer_id = c.customer_id
GROUP BY c.city
ORDER BY total_loan_revenue DESC
LIMIT 1;
```

## 3. 🧵 Pakistan Textile Export Analysis

Real-world analysis of Pakistan's textile export industry using data from Pakistan Textile Council (PTC) and Pakistan Bureau of Statistics (PBS).

**Key Insight:** Floods and COVID-19 impacted Pakistan's textile industry significantly. North America remains the dominant export destination, and value-added products consistently outperform traditional textiles.

### Tables
| Table | Description |
|-------|-------------|
| `products` | Textile product categories with HS chapter codes |
| `countries` | Export destination countries and regions |
| `exports` | Quarterly export values and volumes (2021–2025) |
| `yearly_summary` | Annual totals, YoY growth, cotton production, energy costs |

### SQL Concepts Covered
- **JOIN** across products and countries
- **GROUP BY** with **SUM()** for category and regional breakdowns
- **ORDER BY DESC** for ranking top products and destinations
- **YoY growth analysis** using yearly_summary table
- **WHERE filtering** for product-specific trend tracking

### Sample Query
```sql
-- Total exports by fiscal year
SELECT fiscal_year, SUM(export_value_usd) AS total_exports
FROM exports
GROUP BY fiscal_year
ORDER BY fiscal_year;

-- Top export destinations
SELECT c.country_name, c.region, SUM(e.export_value_usd) AS total_exports
FROM exports AS e
JOIN countries AS c ON c.country_id = e.country_id
GROUP BY c.country_name, c.region
ORDER BY total_exports DESC;
```

## 4. 🛒 E-Commerce Database

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

## 5. 🏦 Bank Database

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

## 6. 🏢 Organisation Database

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

## 7. 🎓 College Database

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
