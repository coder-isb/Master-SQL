# Master-SQL

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

