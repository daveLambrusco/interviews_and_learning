# SQL, NoSQL & MongoDB Q&A

## Table of Contents

1. [SQL](#sql)
   - [DDL](#ddl-data-definition-language) — CREATE, ALTER, DROP, TRUNCATE, INDEX
   - [DML](#dml-data-manipulation-language) — SELECT, INSERT, UPDATE, DELETE
   - [JOINs](#join) — INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF
   - [Aggregate Functions](#funzioni-aggregate) — COUNT, SUM, AVG, MIN, MAX
   - [GROUP BY & HAVING](#group-by-e-having)
   - [Subqueries](#subquery) — WHERE, IN, EXISTS, SELECT, FROM
   - [Window Functions](#window-functions-over) — ROW_NUMBER, RANK, DENSE_RANK, PARTITION BY, LAG/LEAD
   - [CASE](#case-conditional-logic), [UNION / UNION ALL](#union--union-all), [CTE](#cte-common-table-expressions)
2. [Stored Procedures & Functions](#stored-procedures--functions)
   - [What is a Stored Procedure?](#what-is-a-stored-procedure)
   - [Stored Procedure Syntax](#stored-procedure-syntax-mysql--postgresql)
   - [Functions vs Procedures](#stored-functions-vs-stored-procedures)
   - [Variables, Conditionals & Loops](#variables-conditionals-and-loops)
   - [Error Handling](#error-handling-in-stored-procedures)
   - [Triggers](#triggers)
3. [Query Optimization](#query-optimization)
   - [EXPLAIN / EXPLAIN ANALYZE](#understanding-explain--explain-analyze)
   - [Indexes Deep Dive](#indexes-deep-dive) — single, composite, covering, partial index
   - [N+1 Query Problem](#the-n1-query-problem)
   - [Optimization Techniques](#query-optimization-techniques) — SELECT *, functions on indexed columns, EXISTS vs IN, keyset pagination
   - [Query Execution Order](#query-execution-order)
4. [SQL Best Practices](#sql-best-practices)
   - [Schema Design & Normalization](#schema-design-best-practices) — 1NF, 2NF, 3NF, when to denormalize
   - [Transaction Best Practices](#transaction-best-practices)
   - [Isolation Levels](#transaction-isolation-levels) — dirty reads, non-repeatable reads, phantom reads
5. [NoSQL](#nosql) — Types, SQL vs NoSQL comparison
6. [MongoDB](#mongodb)
   - [Architecture & Document Structure](#mongodb-architecture)
   - [CRUD Operations](#crud-operations)
   - [Aggregation Pipeline](#aggregation-pipeline)
   - [Indexing](#indexing-in-mongodb)
   - [MongoDB with Spring Boot](#mongodb-with-spring-boot)

---

## SQL

### What is SQL?

SQL (Structured Query Language) is a standard language for managing and manipulating relational databases. It is used to perform tasks such as querying data, updating records, and managing database structures.

### What are the different types of SQL commands?

The different types of SQL commands include:

- **DDL (Data Definition Language):** Commands like `CREATE`, `ALTER`, `DROP`.
- **DML (Data Manipulation Language):** Commands like `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
- **DCL (Data Control Language):** Commands like `GRANT`, `REVOKE`.
- **TCL (Transaction Control Language):** Commands like `COMMIT`, `ROLLBACK`.

### What is a JOIN in SQL?

A JOIN is a SQL operation used to combine rows from two or more tables based on a related column between them. Types of JOINs include INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

### Panoramica SQL

#### DDL (Data Definition Language)

##### CREATE TABLE

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department_id INT,
    salary DECIMAL(10, 2),
    hire_date DATE,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

##### ALTER TABLE

```sql
-- Aggiungere una colonna
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Modificare una colonna
ALTER TABLE employees MODIFY COLUMN name VARCHAR(150);

-- Eliminare una colonna
ALTER TABLE employees DROP COLUMN phone;
```

##### DROP TABLE

```sql
DROP TABLE employees;
```

##### TRUNCATE TABLE

Rimuove tutti i dati da una tabella ma ne mantiene la struttura. È più veloce di DELETE e resetta gli indici auto-incrementali.

```sql
TRUNCATE TABLE employees;
```

##### CREATE INDEX

```sql
CREATE INDEX idx_employee_name ON employees(name);
CREATE INDEX idx_dept_salary ON employees(department_id, salary);
```

---

#### DML (Data Manipulation Language)

##### SELECT

```sql
-- Base
SELECT name, salary FROM employees;

-- Con condizioni
SELECT * FROM employees WHERE salary > 50000;

-- Con ordinamento
SELECT * FROM employees ORDER BY salary DESC, name ASC;

-- Con limite
SELECT * FROM employees LIMIT 10 OFFSET 20;

-- Distinct
SELECT DISTINCT department_id FROM employees;
```

##### INSERT

```sql
-- Singolo record
INSERT INTO employees (name, email, salary) 
VALUES ('Mario Rossi', 'mario@email.com', 45000);

-- Multiple righe
INSERT INTO employees (name, email, salary) VALUES
    ('Luigi Verdi', 'luigi@email.com', 48000),
    ('Anna Bianchi', 'anna@email.com', 52000);
```

##### UPDATE

```sql
UPDATE employees 
SET salary = salary * 1.1 
WHERE department_id = 3;

-- Update multipli campi
UPDATE employees 
SET salary = 55000, department_id = 5 
WHERE id = 10;
```

##### DELETE

```sql
DELETE FROM employees WHERE id = 10;

-- Delete multipli
DELETE FROM employees WHERE hire_date < '2020-01-01';
```

---

#### JOIN

##### INNER JOIN

Restituisce solo le righe che hanno corrispondenza in entrambe le tabelle.

```sql
SELECT e.name, e.salary, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```

##### LEFT JOIN (LEFT OUTER JOIN)

Restituisce tutte le righe della tabella sinistra + corrispondenze dalla destra.

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
-- Include anche dipendenti senza dipartimento (NULL)
```

##### RIGHT JOIN

Restituisce tutte le righe della tabella destra + corrispondenze dalla sinistra.

```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
-- Include anche dipartimenti senza dipendenti
```

##### FULL OUTER JOIN

Restituisce tutte le righe di entrambe le tabelle (non supportato in MySQL, usa UNION).

```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

##### CROSS JOIN

Prodotto cartesiano (ogni riga di A con ogni riga di B).

```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

##### SELF JOIN

Join di una tabella con se stessa.

```sql
-- Trovare dipendenti con lo stesso manager
SELECT e1.name AS employee, e2.name AS colleague
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.manager_id
WHERE e1.id != e2.id;
```

---

#### Funzioni Aggregate

```sql
-- COUNT
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department_id) FROM employees;

-- SUM
SELECT SUM(salary) FROM employees;

-- AVG
SELECT AVG(salary) FROM employees;

-- MIN / MAX
SELECT MIN(salary), MAX(salary) FROM employees;
```

##### COUNT(*) vs COUNT(column)

`COUNT(*)`: Conta tutte le righe, incluse quelle con valori NULL in qualsiasi colonna.

`COUNT(column)`: Conta solo le righe dove quella colonna specifica NON è NULL.

---

#### GROUP BY e HAVING

##### GROUP BY

Raggruppa righe con valori comuni.

```sql
-- Contare dipendenti per dipartimento
SELECT department_id, COUNT(*) as num_employees
FROM employees
GROUP BY department_id;

-- Salario medio per dipartimento
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id;

-- Raggruppamento multiplo
SELECT department_id, YEAR(hire_date), COUNT(*)
FROM employees
GROUP BY department_id, YEAR(hire_date);
```

##### HAVING

Filtra i risultati dopo il GROUP BY (WHERE filtra prima).

```sql
-- Dipartimenti con più di 5 dipendenti
SELECT department_id, COUNT(*) as num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

-- Dipartimenti con salario medio > 50000
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

**Ordine di esecuzione**: WHERE → GROUP BY → HAVING → ORDER BY

---

#### Subquery

##### Subquery nella WHERE

```sql
-- Dipendenti con salario sopra la media
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Dipendenti nello stesso dipartimento di 'Mario'
SELECT name
FROM employees
WHERE department_id = (
    SELECT department_id FROM employees WHERE name = 'Mario'
);
```

##### Subquery con IN

```sql
-- Dipendenti in dipartimenti con budget > 100000
SELECT name
FROM employees
WHERE department_id IN (
    SELECT id FROM departments WHERE budget > 100000
);
```

##### Subquery con EXISTS

```sql
-- Dipartimenti che hanno almeno un dipendente
SELECT d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.id
);
```

##### Subquery nella SELECT

```sql
SELECT 
    name,
    salary,
    (SELECT AVG(salary) FROM employees) as avg_salary
FROM employees;
```

##### Subquery nella FROM (Derived Table)

```sql
SELECT dept_avg.department_id, dept_avg.avg_salary
FROM (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
) as dept_avg
WHERE dept_avg.avg_salary > 50000;
```

---

#### Window Functions (OVER)

Le window functions eseguono calcoli su un set di righe correlate senza collassarle.

##### ROW_NUMBER()

Assegna un numero progressivo.

```sql
SELECT 
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;
```

##### RANK() e DENSE_RANK()

```sql
-- RANK: salta numeri dopo i tie (1,2,2,4)
SELECT 
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- DENSE_RANK: non salta numeri (1,2,2,3)
SELECT 
    name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;
```

##### PARTITION BY

Divide i dati in gruppi per calcoli separati.

```sql
-- Ranking per dipartimento
SELECT 
    name,
    department_id,
    salary,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as dept_rank
FROM employees;
```

##### LAG() e LEAD()

Accedono a righe precedenti o successive.

```sql
-- Confrontare con il salario precedente
SELECT 
    name,
    salary,
    LAG(salary) OVER (ORDER BY hire_date) as previous_salary,
    LEAD(salary) OVER (ORDER BY hire_date) as next_salary
FROM employees;
```

##### Funzioni aggregate con OVER

```sql
-- Running total
SELECT 
    name,
    salary,
    SUM(salary) OVER (ORDER BY hire_date) as running_total
FROM employees;

-- Media mobile
SELECT 
    name,
    salary,
    AVG(salary) OVER (
        ORDER BY hire_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) as moving_avg
FROM employees;
```

---

#### CASE (Conditional Logic)

```sql
-- Case semplice
SELECT 
    name,
    salary,
    CASE 
        WHEN salary < 40000 THEN 'Low'
        WHEN salary < 60000 THEN 'Medium'
        ELSE 'High'
    END as salary_category
FROM employees;

-- Case con aggregate
SELECT 
    department_id,
    COUNT(CASE WHEN salary > 50000 THEN 1 END) as high_earners
FROM employees
GROUP BY department_id;
```

---

#### UNION / UNION ALL

```sql
-- UNION: rimuove duplicati
SELECT name FROM employees_2023
UNION
SELECT name FROM employees_2024;

-- UNION ALL: mantiene duplicati (più veloce)
SELECT name FROM employees_2023
UNION ALL
SELECT name FROM employees_2024;
```

---

#### CTE (Common Table Expressions)

```sql
-- CTE singola
WITH dept_avg AS (
    SELECT department_id, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.name, e.salary, da.avg_salary
FROM employees e
JOIN dept_avg da ON e.department_id = da.department_id
WHERE e.salary > da.avg_salary;

-- CTE ricorsiva (esempio: gerarchia)
WITH RECURSIVE employee_hierarchy AS (
    -- Base case
    SELECT id, name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

---

## Stored Procedures & Functions

### What is a Stored Procedure?

A **stored procedure** is a precompiled set of SQL statements stored in the database that can be executed by name. Unlike a regular query sent from the application, a stored procedure lives on the database server.

**Benefits:**
- **Performance**: Precompiled and cached execution plan
- **Reduced network traffic**: Send one call instead of many SQL statements
- **Reusability**: Can be called from multiple applications
- **Security**: Grant execution permission without exposing tables
- **Encapsulation**: Business logic lives in the DB (though debated)

**Drawbacks:**
- Hard to version control and test
- Business logic scattered between app and DB
- Tightly couples application to a specific database engine

### Stored Procedure Syntax (MySQL / PostgreSQL)

```sql
-- MySQL: Create a procedure to get employees by department
DELIMITER $$
CREATE PROCEDURE GetEmployeesByDepartment(IN dept_id INT)
BEGIN
    SELECT id, name, salary, hire_date
    FROM employees
    WHERE department_id = dept_id
    ORDER BY name;
END$$
DELIMITER ;

-- Call the procedure
CALL GetEmployeesByDepartment(3);
```

```sql
-- PostgreSQL: Create a procedure with INOUT parameter
CREATE OR REPLACE PROCEDURE update_salary(
    IN emp_id INT,
    IN raise_percent DECIMAL(5,2),
    INOUT new_salary DECIMAL(10,2)
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE employees
    SET salary = salary * (1 + raise_percent / 100)
    WHERE id = emp_id
    RETURNING salary INTO new_salary;
END;
$$;

-- Call in PostgreSQL
DO $$
DECLARE v_salary DECIMAL(10,2);
BEGIN
    CALL update_salary(1, 10.00, v_salary);
    RAISE NOTICE 'New salary: %', v_salary;
END;
$$;
```

### Stored Functions vs Stored Procedures

| Feature | Stored Function | Stored Procedure |
|---------|----------------|------------------|
| **Returns** | Single value | Zero or more result sets |
| **Call in SQL** | Yes (`SELECT my_func()`) | No (`CALL my_proc()`) |
| **DML inside** | Generally read-only | Can INSERT/UPDATE/DELETE |
| **Transaction control** | Cannot | Can |
| **Use case** | Computations, transformations | Business workflows |

```sql
-- Stored FUNCTION (returns a value, usable in SELECT)
CREATE FUNCTION calculate_annual_salary(monthly_salary DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN monthly_salary * 12;
END;

-- Use in a query
SELECT name, calculate_annual_salary(salary) AS annual_salary
FROM employees;
```

### Variables, Conditionals, and Loops

```sql
-- MySQL example with variables, IF, and LOOP
DELIMITER $$
CREATE PROCEDURE ProcessBonuses(IN dept_id INT, IN performance_year INT)
BEGIN
    DECLARE done INT DEFAULT FALSE;
    DECLARE emp_id INT;
    DECLARE emp_salary DECIMAL(10,2);
    DECLARE bonus_rate DECIMAL(5,2);
    
    -- Cursor to iterate over employees
    DECLARE emp_cursor CURSOR FOR
        SELECT id, salary FROM employees WHERE department_id = dept_id;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    OPEN emp_cursor;
    
    read_loop: LOOP
        FETCH emp_cursor INTO emp_id, emp_salary;
        IF done THEN
            LEAVE read_loop;
        END IF;
        
        -- Conditional bonus rate
        IF emp_salary > 70000 THEN
            SET bonus_rate = 0.05;
        ELSEIF emp_salary > 50000 THEN
            SET bonus_rate = 0.08;
        ELSE
            SET bonus_rate = 0.10;
        END IF;
        
        INSERT INTO bonuses (employee_id, year, amount)
        VALUES (emp_id, performance_year, emp_salary * bonus_rate);
    END LOOP;
    
    CLOSE emp_cursor;
END$$
DELIMITER ;
```

### Error Handling in Stored Procedures

```sql
-- MySQL: Transaction with error handling
DELIMITER $$
CREATE PROCEDURE TransferFunds(
    IN from_account INT,
    IN to_account INT,
    IN amount DECIMAL(10,2)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        RESIGNAL; -- Re-raise the error to the caller
    END;
    
    START TRANSACTION;
    
    UPDATE accounts SET balance = balance - amount WHERE id = from_account;
    
    -- Check balance didn't go negative
    IF (SELECT balance FROM accounts WHERE id = from_account) < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Insufficient funds';
    END IF;
    
    UPDATE accounts SET balance = balance + amount WHERE id = to_account;
    
    COMMIT;
END$$
DELIMITER ;
```

### Triggers

Triggers are stored procedures that execute **automatically** when a table event (INSERT, UPDATE, DELETE) occurs.

```sql
-- Audit trigger: log every salary change
CREATE TABLE salary_audit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employee_id INT,
    old_salary DECIMAL(10,2),
    new_salary DECIMAL(10,2),
    changed_by VARCHAR(100),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

DELIMITER $$
CREATE TRIGGER after_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF OLD.salary != NEW.salary THEN
        INSERT INTO salary_audit(employee_id, old_salary, new_salary, changed_by)
        VALUES (NEW.id, OLD.salary, NEW.salary, CURRENT_USER());
    END IF;
END$$
DELIMITER ;
```

**Types of triggers:**
- `BEFORE INSERT / UPDATE / DELETE` — validate or modify data before the operation
- `AFTER INSERT / UPDATE / DELETE` — audit logging, cascading updates

---

## Query Optimization

### Understanding EXPLAIN / EXPLAIN ANALYZE

Always use `EXPLAIN` to understand how the query engine executes your query.

```sql
-- MySQL
EXPLAIN SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE e.salary > 60000;

-- PostgreSQL (with actual timing)
EXPLAIN ANALYZE SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE e.salary > 60000;
```

**Key fields to look at:**
| Field | What to watch for |
|-------|-------------------|
| `type` | `ALL` = full table scan ❌, `ref`/`eq_ref`/`index` = good ✅ |
| `rows` | Estimated rows examined — lower is better |
| `Extra` | `Using filesort` or `Using temporary` = potential issue |
| `key` | Which index is being used (NULL = no index used) |

### Indexes Deep Dive

```sql
-- Check existing indexes
SHOW INDEX FROM employees;

-- Single column index
CREATE INDEX idx_salary ON employees(salary);

-- Composite index — order matters! (most selective first)
CREATE INDEX idx_dept_salary ON employees(department_id, salary);
-- This index helps: WHERE department_id = 3 AND salary > 50000
-- This index helps: WHERE department_id = 3
-- This index does NOT help: WHERE salary > 50000 (leading column missing)

-- Covering index: includes all columns the query needs
-- Query: SELECT name, salary FROM employees WHERE department_id = 3
CREATE INDEX idx_covering ON employees(department_id, name, salary);
-- With a covering index, the DB never touches the main table — just the index

-- Partial index (PostgreSQL) — index only active employees
CREATE INDEX idx_active_employees ON employees(name)
WHERE active = true;
```

**Index selectivity rule:** High selectivity = few rows per value = better index.
- `status` column with 3 values → low selectivity → bad index candidate
- `email` column with unique values → high selectivity → excellent index candidate

### The N+1 Query Problem

A critical performance anti-pattern — one query to get a list, then one query **per item**.

```sql
-- ❌ N+1: First query returns 100 employees, then 100 queries to get their departments
SELECT * FROM employees;   -- 1 query
-- Then for each employee:
SELECT * FROM departments WHERE id = ?;  -- N queries!

-- ✅ Fix: JOIN fetches everything in one query
SELECT e.*, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id;
```

In Java with JPA/Hibernate:

```java
// ❌ N+1 — LazyLoading triggers separate queries
List<Employee> employees = employeeRepo.findAll();
employees.forEach(e -> System.out.println(e.getDepartment().getName())); // N queries!

// ✅ Fix: JOIN FETCH
@Query("SELECT e FROM Employee e JOIN FETCH e.department")
List<Employee> findAllWithDepartment();

// ✅ Fix: @EntityGraph
@EntityGraph(attributePaths = {"department"})
List<Employee> findAll();
```

### Query Optimization Techniques

#### 1. Avoid `SELECT *`

```sql
-- ❌ Bad: fetches all columns, prevents covering index
SELECT * FROM employees WHERE department_id = 3;

-- ✅ Good: only fetch what you need
SELECT id, name, salary FROM employees WHERE department_id = 3;
```

#### 2. Avoid Functions on Indexed Columns in WHERE

```sql
-- ❌ Bad: index on hire_date is NOT used (function applied to column)
SELECT * FROM employees WHERE YEAR(hire_date) = 2023;

-- ✅ Good: range query uses index
SELECT * FROM employees
WHERE hire_date BETWEEN '2023-01-01' AND '2023-12-31';

-- ❌ Bad: index on email NOT used
SELECT * FROM employees WHERE LOWER(email) = 'user@example.com';

-- ✅ Good: store emails lowercase, or use case-insensitive collation
SELECT * FROM employees WHERE email = 'user@example.com';
```

#### 3. Use EXISTS instead of IN for subqueries

```sql
-- ❌ Slower: IN with correlated subquery evaluates full subquery
SELECT * FROM departments
WHERE id IN (SELECT department_id FROM employees WHERE salary > 80000);

-- ✅ Faster: EXISTS stops as soon as one match is found
SELECT * FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e
    WHERE e.department_id = d.id AND e.salary > 80000
);
```

#### 4. Pagination: Use Keyset Pagination for Large Datasets

```sql
-- ❌ OFFSET gets slower as offset increases (must skip N rows)
SELECT * FROM employees ORDER BY id LIMIT 20 OFFSET 10000;

-- ✅ Keyset pagination: always fast regardless of page
-- After page 1 (last seen id = 200):
SELECT * FROM employees WHERE id > 200 ORDER BY id LIMIT 20;
```

#### 5. Avoid Implicit Type Conversions

```sql
-- ❌ Bad: id is INT but comparing to string — forces type conversion, no index
SELECT * FROM employees WHERE id = '123';

-- ✅ Good
SELECT * FROM employees WHERE id = 123;
```

#### 6. Query Caching and Read Replicas

```sql
-- Application-level: cache frequently read, rarely changed data
-- e.g., department list, product catalog, configuration

-- Database-level: use read replicas for SELECT-heavy workloads
-- Primary DB → handles all writes
-- Read replicas → handle SELECT queries (scaled horizontally)
```

### Query Execution Order

Understanding execution order helps write better queries:

```sql
SELECT department_id, AVG(salary) AS avg_salary   -- 6. SELECT
FROM employees                                      -- 1. FROM
JOIN departments d ON e.department_id = d.id        -- 2. JOIN
WHERE salary > 30000                                -- 3. WHERE
GROUP BY department_id                              -- 4. GROUP BY
HAVING AVG(salary) > 50000                         -- 5. HAVING
ORDER BY avg_salary DESC                            -- 7. ORDER BY
LIMIT 10;                                           -- 8. LIMIT
```

**Key implications:**
- You **can't** use `SELECT` aliases in `WHERE` (WHERE runs before SELECT)
- You **can** use `SELECT` aliases in `ORDER BY` (ORDER BY runs after SELECT)
- `WHERE` filters rows before grouping — use `HAVING` to filter groups

---

## SQL Best Practices

### Schema Design Best Practices

#### Normalization

Normalization reduces redundancy and improves data integrity.

| Normal Form | Rule |
|-------------|------|
| **1NF** | Each cell contains a single value; no repeating groups |
| **2NF** | 1NF + no partial dependencies (non-key columns depend on the whole PK) |
| **3NF** | 2NF + no transitive dependencies (non-key columns depend only on PK) |

```sql
-- ❌ Violation of 1NF: multiple values in one cell
CREATE TABLE orders (
    id INT,
    products VARCHAR(500)  -- "laptop,mouse,keyboard" — BAD!
);

-- ✅ 1NF: proper table design
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);

-- ❌ Violation of 3NF: transitive dependency
-- department_name depends on department_id, not on employee_id
CREATE TABLE employees (
    id INT PRIMARY KEY,
    department_id INT,
    department_name VARCHAR(100)  -- Redundant! Move to departments table
);

-- ✅ 3NF: normalize it out
CREATE TABLE departments (id INT PRIMARY KEY, name VARCHAR(100));
CREATE TABLE employees (id INT PRIMARY KEY, department_id INT REFERENCES departments(id));
```

#### When to Denormalize

Sometimes intentional denormalization is appropriate for **read performance**:
- Reporting tables / data warehouses
- Read-heavy analytics queries
- When JOINs become too expensive

#### Naming Conventions

```sql
-- ✅ Consistent, readable naming
CREATE TABLE order_items (           -- plural, snake_case
    id BIGINT PRIMARY KEY,           -- always have a surrogate PK
    order_id BIGINT NOT NULL,        -- FK columns: referenced_table_id
    product_id BIGINT NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    unit_price DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP,
    
    CONSTRAINT fk_order_items_order FOREIGN KEY (order_id) REFERENCES orders(id),
    CONSTRAINT fk_order_items_product FOREIGN KEY (product_id) REFERENCES products(id),
    CONSTRAINT chk_quantity_positive CHECK (quantity > 0)
);
```

### Transaction Best Practices

```sql
-- Keep transactions short and focused
START TRANSACTION;
    UPDATE accounts SET balance = balance - 500 WHERE id = 1;
    UPDATE accounts SET balance = balance + 500 WHERE id = 2;
    INSERT INTO transaction_log(from_id, to_id, amount) VALUES (1, 2, 500);
COMMIT;

-- Use appropriate isolation level
-- READ COMMITTED: default in PostgreSQL — prevents dirty reads
-- REPEATABLE READ: default in MySQL — prevents non-repeatable reads
-- SERIALIZABLE: maximum isolation — prevents phantom reads (slowest)
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### Transaction Isolation Levels

| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|------------|---------------------|--------------|
| `READ UNCOMMITTED` | ✅ Possible | ✅ Possible | ✅ Possible |
| `READ COMMITTED` | ❌ Prevented | ✅ Possible | ✅ Possible |
| `REPEATABLE READ` | ❌ Prevented | ❌ Prevented | ✅ Possible |
| `SERIALIZABLE` | ❌ Prevented | ❌ Prevented | ❌ Prevented |

- **Dirty read**: reading uncommitted data from another transaction
- **Non-repeatable read**: same row gives different results in the same transaction
- **Phantom read**: a second query in the same transaction finds new rows that weren't there before

### General SQL Best Practices

```sql
-- ✅ Always use NOT NULL where possible — NULLs complicate queries
-- ✅ Use FOREIGN KEYS for referential integrity
-- ✅ Add indexes on frequently filtered/joined columns
-- ✅ Use ENUM or CHECK constraints for restricted values
-- ✅ Store timestamps in UTC
-- ✅ Use BIGINT for IDs on tables that will grow large
-- ✅ Add created_at / updated_at to every table

-- ❌ Never store comma-separated lists in a column
-- ❌ Never use SELECT * in production queries
-- ❌ Never DELETE without a WHERE clause (test with SELECT first!)
-- ❌ Avoid VARCHAR(MAX) on indexed columns
-- ❌ Don't put business logic in triggers (hard to debug)

-- Safe DELETE pattern: always verify with SELECT first
SELECT * FROM employees WHERE department_id = 99;  -- verify
DELETE FROM employees WHERE department_id = 99;     -- then delete
```

---

## NoSQL

### What is NoSQL?

NoSQL (Not Only SQL) databases provide a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. They are designed to handle large volumes of unstructured or semi-structured data.

### What are the types of NoSQL databases?

The types of NoSQL databases include:

- **Document-Oriented:** Stores data as documents (e.g., MongoDB).
- **Key-Value:** Stores data as key-value pairs (e.g., Redis).
- **Column-Family:** Stores data in columns rather than rows (e.g., Cassandra).
- **Graph:** Stores data in graph structures with nodes and edges (e.g., Neo4j).

### What is the difference between SQL and NoSQL databases?

The main differences between SQL and NoSQL databases are:

- **Data Model:** SQL databases use a structured, tabular data model, while NoSQL databases use various data models (document, key-value, column-family, graph).
- **Schema:** SQL databases have a fixed schema, while NoSQL databases have a flexible schema.
- **Scalability:** SQL databases are vertically scalable, while NoSQL databases are horizontally scalable.
- **Use Cases:** SQL databases are suited for complex queries and transactions, while NoSQL databases are suited for large-scale, distributed data storage.

## MongoDB

### What is MongoDB?

MongoDB is a **document-oriented NoSQL database** that stores data in flexible, JSON-like documents called BSON (Binary JSON). It's designed for scalability, high availability, and high performance.

**Key Characteristics:**

- **Document Model**: Stores data in flexible documents instead of rows and columns
- **Schema-less**: Documents in a collection can have different structures
- **Horizontal Scaling**: Built-in sharding for distributing data across multiple servers
- **Rich Query Language**: Supports complex queries, aggregations, and indexing
- **High Availability**: Replica sets provide automatic failover

### MongoDB Architecture

#### Basic Concepts

| SQL Concept | MongoDB Equivalent |
|-------------|--------------------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | _id |
| Index | Index |
| JOIN | $lookup (Aggregation) |

#### Document Structure

```javascript
// MongoDB Document (BSON)
{
    "_id": ObjectId("507f1f77bcf86cd799439011"),
    "name": "Mario Rossi",
    "email": "mario@example.com",
    "age": 30,
    "address": {
        "street": "Via Roma 1",
        "city": "Rome",
        "country": "Italy"
    },
    "hobbies": ["reading", "gaming", "cooking"],
    "orders": [
        { "orderId": 1001, "total": 150.00, "date": ISODate("2024-01-15") },
        { "orderId": 1002, "total": 75.50, "date": ISODate("2024-02-20") }
    ],
    "createdAt": ISODate("2024-01-01T10:00:00Z"),
    "active": true
}
```

#### Data Types

| Type | Description | Example |
|------|-------------|----------|
| String | UTF-8 string | `"Hello"` |
| Number | Integer or Double | `42`, `3.14` |
| Boolean | true/false | `true` |
| Date | Date/time | `ISODate("2024-01-01")` |
| ObjectId | 12-byte unique ID | `ObjectId("507f1f77bcf86cd799439011")` |
| Array | List of values | `[1, 2, 3]` |
| Object | Embedded document | `{"key": "value"}` |
| Null | Null value | `null` |
| Binary | Binary data | Binary files |

### CRUD Operations

#### Create (Insert)

```javascript
// Insert one document
db.users.insertOne({
    name: "Mario Rossi",
    email: "mario@example.com",
    age: 30
});

// Insert multiple documents
db.users.insertMany([
    { name: "Luigi Verdi", email: "luigi@example.com", age: 25 },
    { name: "Anna Bianchi", email: "anna@example.com", age: 35 }
]);
```

##### Read (Query)

```javascript
// Find all documents
db.users.find();

// Find with filter
db.users.find({ age: { $gte: 25 } });

// Find one document
db.users.findOne({ email: "mario@example.com" });

// Projection (select specific fields)
db.users.find(
    { age: { $gte: 25 } },
    { name: 1, email: 1, _id: 0 }  // 1 = include, 0 = exclude
);

// Query operators
db.users.find({
    $and: [
        { age: { $gte: 18 } },
        { age: { $lte: 65 } }
    ]
});

db.users.find({
    $or: [
        { city: "Rome" },
        { city: "Milan" }
    ]
});

// Array queries
db.users.find({ hobbies: "gaming" });  // Contains "gaming"
db.users.find({ hobbies: { $all: ["reading", "gaming"] } });  // Contains all
db.users.find({ hobbies: { $size: 3 } });  // Array has exactly 3 elements

// Nested document queries
db.users.find({ "address.city": "Rome" });

// Sorting and limiting
db.users.find().sort({ age: -1 }).limit(10).skip(20);
```

##### Update

```javascript
// Update one document
db.users.updateOne(
    { email: "mario@example.com" },
    { $set: { age: 31, updatedAt: new Date() } }
);

// Update multiple documents
db.users.updateMany(
    { active: false },
    { $set: { status: "inactive" } }
);

// Update operators
db.users.updateOne(
    { _id: ObjectId("...") },
    {
        $set: { name: "Mario" },           // Set field value
        $unset: { tempField: "" },         // Remove field
        $inc: { loginCount: 1 },           // Increment
        $push: { hobbies: "swimming" },    // Add to array
        $pull: { hobbies: "gaming" },      // Remove from array
        $addToSet: { tags: "premium" }     // Add to array if not exists
    }
);

// Replace entire document
db.users.replaceOne(
    { email: "mario@example.com" },
    { name: "Mario Rossi", email: "mario@example.com", age: 31 }
);

// Upsert (insert if not exists)
db.users.updateOne(
    { email: "new@example.com" },
    { $set: { name: "New User", age: 25 } },
    { upsert: true }
);
```

#### Delete

```javascript
// Delete one document
db.users.deleteOne({ email: "mario@example.com" });

// Delete multiple documents
db.users.deleteMany({ active: false });

// Delete all documents in collection
db.users.deleteMany({});
```

### Aggregation Pipeline

The aggregation pipeline processes documents through stages, each transforming the data.

#### Basic Aggregation

```javascript
db.orders.aggregate([
    // Stage 1: Filter
    { $match: { status: "completed" } },
    
    // Stage 2: Group and calculate
    { $group: {
        _id: "$customerId",
        totalSpent: { $sum: "$amount" },
        orderCount: { $sum: 1 },
        avgOrderValue: { $avg: "$amount" }
    }},
    
    // Stage 3: Sort
    { $sort: { totalSpent: -1 } },
    
    // Stage 4: Limit
    { $limit: 10 }
]);
```

#### Common Aggregation Stages

```javascript
// $project - Select/reshape fields
{ $project: {
    fullName: { $concat: ["$firstName", " ", "$lastName"] },
    year: { $year: "$createdAt" },
    totalWithTax: { $multiply: ["$total", 1.22] }
}}

// $lookup - Join with another collection
{ $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "userId",
    as: "userOrders"
}}

// $unwind - Deconstruct array field
{ $unwind: "$items" }

// $addFields - Add new fields
{ $addFields: {
    totalPrice: { $multiply: ["$quantity", "$price"] }
}}

// $bucket - Group into ranges
{ $bucket: {
    groupBy: "$age",
    boundaries: [0, 18, 30, 50, 100],
    default: "Other",
    output: { count: { $sum: 1 } }
}}

// $facet - Multiple pipelines in parallel
{ $facet: {
    "byStatus": [{ $group: { _id: "$status", count: { $sum: 1 } } }],
    "byCategory": [{ $group: { _id: "$category", total: { $sum: "$amount" } } }],
    "topProducts": [{ $sort: { sales: -1 } }, { $limit: 5 }]
}}
```

#### Advanced Aggregation Example

```javascript
// Sales report by month with comparison to previous month
db.sales.aggregate([
    { $match: { date: { $gte: ISODate("2024-01-01") } } },
    
    { $group: {
        _id: {
            year: { $year: "$date" },
            month: { $month: "$date" }
        },
        totalSales: { $sum: "$amount" },
        avgSale: { $avg: "$amount" },
        count: { $sum: 1 }
    }},
    
    { $sort: { "_id.year": 1, "_id.month": 1 } },
    
    { $setWindowFields: {
        sortBy: { "_id.year": 1, "_id.month": 1 },
        output: {
            prevMonthSales: {
                $shift: { output: "$totalSales", by: -1 }
            }
        }
    }},
    
    { $addFields: {
        growthRate: {
            $cond: {
                if: { $eq: ["$prevMonthSales", null] },
                then: null,
                else: {
                    $multiply: [
                        { $divide: [
                            { $subtract: ["$totalSales", "$prevMonthSales"] },
                            "$prevMonthSales"
                        ]},
                        100
                    ]
                }
            }
        }
    }}
]);
```

### Indexing in MongoDB

#### Index Types

```javascript
// Single field index
db.users.createIndex({ email: 1 });  // 1 = ascending, -1 = descending

// Compound index
db.users.createIndex({ lastName: 1, firstName: 1 });

// Unique index
db.users.createIndex({ email: 1 }, { unique: true });

// Text index (for full-text search)
db.articles.createIndex({ title: "text", content: "text" });
db.articles.find({ $text: { $search: "mongodb tutorial" } });

// TTL index (auto-delete after time)
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 });

// Partial index
db.users.createIndex(
    { email: 1 },
    { partialFilterExpression: { active: true } }
);

// Sparse index (only documents with field)
db.users.createIndex({ phone: 1 }, { sparse: true });
```

#### Index Management

```javascript
// List indexes
db.users.getIndexes();

// Drop index
db.users.dropIndex("email_1");

// Drop all indexes (except _id)
db.users.dropIndexes();

// Explain query (check index usage)
db.users.find({ email: "test@example.com" }).explain("executionStats");
```

### MongoDB with Spring Boot

#### Dependencies (pom.xml)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>

<!-- For reactive -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb-reactive</artifactId>
</dependency>
```

#### Configuration

```yaml
# application.yml
spring:
  data:
    mongodb:
      uri: mongodb://localhost:27017/mydb
      # Or individual properties:
      # host: localhost
      # port: 27017
      # database: mydb
      # username: user
      # password: pass
      # authentication-database: admin
```

#### Document Entity

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.index.CompoundIndex;

@Document(collection = "users")
@CompoundIndex(name = "name_email_idx", def = "{'name': 1, 'email': 1}")
public class User {
    
    @Id
    private String id;
    
    private String name;
    
    @Indexed(unique = true)
    private String email;
    
    private Integer age;
    
    @Field("addr")  // Custom field name in MongoDB
    private Address address;
    
    private List<String> hobbies;
    
    private LocalDateTime createdAt;
    
    // Getters and setters
}

public class Address {
    private String street;
    private String city;
    private String country;
}
```

#### Repository

```java
import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.data.mongodb.repository.Query;
import org.springframework.data.mongodb.repository.Aggregation;

public interface UserRepository extends MongoRepository<User, String> {
    
    // Query methods
    Optional<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
    List<User> findByAddressCity(String city);
    List<User> findByHobbiesContaining(String hobby);
    
    // Custom JSON query
    @Query("{ 'age': { $gte: ?0, $lte: ?1 } }")
    List<User> findByAgeBetween(int minAge, int maxAge);
    
    // Query with projection
    @Query(value = "{ 'active': true }", fields = "{ 'name': 1, 'email': 1 }")
    List<User> findActiveUsersProjected();
    
    // Aggregation
    @Aggregation(pipeline = {
        "{ $match: { 'active': true } }",
        "{ $group: { _id: '$address.city', count: { $sum: 1 } } }",
        "{ $sort: { count: -1 } }"
    })
    List<CityCount> countUsersByCity();
}

// For aggregation result
public record CityCount(String id, long count) {}
```

#### MongoTemplate for Complex Queries

```java
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Update;
import org.springframework.data.mongodb.core.aggregation.Aggregation;

@Service
public class UserService {
    
    private final MongoTemplate mongoTemplate;
    
    public List<User> findUsersByComplexCriteria(String city, int minAge, List<String> hobbies) {
        Query query = new Query();
        
        query.addCriteria(Criteria.where("address.city").is(city));
        query.addCriteria(Criteria.where("age").gte(minAge));
        query.addCriteria(Criteria.where("hobbies").all(hobbies));
        query.with(Sort.by(Sort.Direction.DESC, "createdAt"));
        query.limit(20);
        
        return mongoTemplate.find(query, User.class);
    }
    
    public void updateUserAge(String id, int newAge) {
        Query query = Query.query(Criteria.where("_id").is(id));
        Update update = new Update()
            .set("age", newAge)
            .set("updatedAt", LocalDateTime.now());
        
        mongoTemplate.updateFirst(query, update, User.class);
    }
    
    public List<AggregationResult> aggregateUserStats() {
        Aggregation aggregation = Aggregation.newAggregation(
            Aggregation.match(Criteria.where("active").is(true)),
            Aggregation.group("address.city")
                .count().as("userCount")
                .avg("age").as("avgAge"),
            Aggregation.sort(Sort.Direction.DESC, "userCount"),
            Aggregation.limit(10)
        );
        
        return mongoTemplate.aggregate(
            aggregation, "users", AggregationResult.class
        ).getMappedResults();
    }
}
```

#### Reactive MongoDB

```java
import org.springframework.data.mongodb.repository.ReactiveMongoRepository;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public interface ReactiveUserRepository extends ReactiveMongoRepository<User, String> {
    Flux<User> findByAgeGreaterThan(int age);
    Mono<User> findByEmail(String email);
}

@RestController
@RequestMapping("/api/users")
public class ReactiveUserController {
    
    private final ReactiveUserRepository repository;
    
    @GetMapping(produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<User> streamAllUsers() {
        return repository.findAll()
            .delayElements(Duration.ofMillis(100));
    }
    
    @GetMapping("/{id}")
    public Mono<ResponseEntity<User>> getUser(@PathVariable String id) {
        return repository.findById(id)
            .map(ResponseEntity::ok)
            .defaultIfEmpty(ResponseEntity.notFound().build());
    }
}
```

#### MongoDB Transactions (Replica Set Required)

```java
@Service
public class OrderService {
    
    private final MongoTemplate mongoTemplate;
    
    @Transactional
    public void createOrder(Order order) {
        // All operations in a single transaction
        mongoTemplate.insert(order);
        mongoTemplate.updateFirst(
            Query.query(Criteria.where("_id").is(order.getUserId())),
            new Update().inc("orderCount", 1),
            User.class
        );
    }
}
```

