# Master-SQL

# SQL Bible – From Beginner to Advanced

A comprehensive SQL cheat sheet for developers, DBAs, and architects. Includes query types, examples, advanced topics, and best practices.

---

## 📑 Index

1. [SQL Query Types](#sql-query-types)
   - [DDL – Data Definition Language](#1-ddl---data-definition-language)
   - [DML – Data Manipulation Language](#2-dml---data-manipulation-language)
   - [DCL – Data Control Language](#3-dcl---data-control-language)
   - [TCL – Transaction Control Language](#4-tcl---transaction-control-language)
2. [SQL Joins](#sql-joins)
3. [Advanced Queries](#advanced-queries)
   - [CTE – Common Table Expressions](#cte--common-table-expressions)
   - [Recursive CTE](#recursive-cte)
   - [Window Functions](#window-functions)
   - [Pivot / Unpivot](#pivot--unpivot)
4. [Indexing & Optimization](#indexing--optimization)
5. [Database Design Tips](#database-design-tips)
6. [Best Practices](#best-practices)

---

## SQL Query Types




+------------------------------------------------------+
|                 SQL COMMANDS BIBLE                  |
+------------------------------------------------------+

      +---------+          +---------+          +---------+          +---------+
      |   DDL   |          |   DML   |          |   DCL   |          |   TCL   |
      +---------+          +---------+          +---------+          +---------+
      | CREATE  |          | SELECT  |          | GRANT   |          | COMMIT  |
      | ALTER   |          | INSERT  |          | REVOKE  |          | ROLLBACK|
      | DROP    |          | UPDATE  |                              | SAVEPOINT|
      | TRUNCATE|          | DELETE  |                              |         |
      +---------+          +---------+          +---------+          +---------+

Examples:
## 1. DDL – Data Definition Language
DDL (Data Definition Language) - structure/schema changes
--------------------------------------------------------
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50)
);

ALTER TABLE employees ADD COLUMN salary DECIMAL(10,2);

DROP TABLE old_employees;

TRUNCATE TABLE temp_data;

DML (Data Manipulation Language) - CRUD operations
---------------------------------------------------
-- Insert
INSERT INTO employees (id, name, department, salary)
VALUES (1, 'Alice', 'HR', 50000);

-- Update
UPDATE employees SET salary = salary * 1.10 WHERE department = 'HR';

-- Delete
DELETE FROM employees WHERE id = 1;

-- Select
SELECT id, name, salary FROM employees WHERE salary > 60000 ORDER BY salary DESC;

DCL (Data Control Language) - permissions
------------------------------------------
-- Grant privileges
GRANT SELECT, INSERT ON employees TO john;

-- Revoke privileges
REVOKE DELETE ON employees FROM john;

TCL (Transaction Control Language) - transactions
--------------------------------------------------
BEGIN TRANSACTION;

UPDATE account SET balance = balance - 100 WHERE id = 1;
UPDATE account SET balance = balance + 100 WHERE id = 2;

-- If error occurs
ROLLBACK;

-- If all good
COMMIT;

-- Optional savepoint
SAVEPOINT before_bonus_update;

