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

## Index
| Type          | Purpose                 | Example                                               | How it Works / Analogy                                           | Trade-offs                                        |
| ------------- | ----------------------- | ----------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------- |
| B-Tree        | Equality & range        | `CREATE INDEX idx_name ON emp(name);`                 | Tree nodes → leaf nodes point to rows; like **dictionary pages** | Good range/equality; slows inserts/updates        |
| Hash          | Exact match             | `CREATE INDEX idx_hash ON emp(id) USING HASH;`        | Hash function → bucket → pointer; like **warehouse bin**         | Fast equality only; no range                      |
| Composite     | Multi-column            | `CREATE INDEX idx_comp ON emp(dept_id,salary);`       | Columns concatenated; order matters                              | Efficient multi-column queries; larger index size |
| Unique        | Enforce uniqueness      | `CREATE UNIQUE INDEX idx_uid ON emp(email);`          | Prevent duplicates                                               | Slower inserts                                    |
| Full-Text     | Text search             | `CREATE FULLTEXT INDEX idx_text ON emp(description);` | Index words; like **search engine index**                        | Storage-heavy; optimized search                   |
| Clustered     | Rows physically ordered | `CREATE CLUSTERED INDEX idx_clustered ON emp(id);`    | Table sorted by key; only 1 per table; like **phonebook**        | Fast range queries; slows inserts                 |
| Non-Clustered | Separate structure      | `CREATE NONCLUSTERED INDEX idx_noncl ON emp(name);`   | Index → pointer to row; like **library card catalog**            | Multiple allowed; extra storage                   |

| Q                                   | A                                                                                |
| ----------------------------------- | -------------------------------------------------------------------------------- |
| Clustered vs Non-Clustered?         | Clustered = table physically sorted; Non-clustered = separate pointer structure. |
| Max clustered indexes per table?    | 1                                                                                |
| Multiple non-clustered indexes?     | Yes                                                                              |
| How does B-Tree help range queries? | Sorted leaf nodes allow sequential traversal                                     |
| Can you index every column?         | Not recommended; overhead on storage and writes                                  |
| Real-world analogy?                 | Clustered = phonebook; Non-clustered = card catalog                              |
| Composite index?                    | Columns concatenated; order matters for query optimization                       |
| Hash vs B-Tree?                     | Hash = exact match only; B-Tree = equality + range                               |
| Full-text index?                    | Searching text-heavy columns efficiently                                         |

| Scenario                  | Index Choice       | Reason                           |
| ------------------------- | ------------------ | -------------------------------- |
| Exact match               | Hash               | Direct lookup O(1)               |
| Range query               | Clustered / B-Tree | Ordered traversal                |
| Multi-column filter       | Composite          | Efficient multi-column filtering |
| Uniqueness                | Unique             | Enforce constraint               |
| Text search               | Full-Text          | Efficient for LIKE/CONTAINS      |
| Frequently sorted queries | Clustered          | Avoids runtime sorting           |



## Truncate Delete Drop

| Operation | Scope                   | Speed / Logging    | WHERE | Triggers | Rollback  | Identity Reset | Notes                                |
| --------- | ----------------------- | ------------------ | ----- | -------- | --------- | -------------- | ------------------------------------ |
| TRUNCATE  | Remove all rows         | Fast, minimal      | ❌     | ❌        | ❌ Usually | ✅ Yes          | Efficient, cannot delete selectively |
| DELETE    | Remove rows selectively | Slower, row-by-row | ✅     | ✅        | ✅         | ❌              | Conditional delete; triggers fire    |
| DROP      | Remove table/database   | Immediate          | ❌     | ❌        | ❌         | ❌              | Deletes table & schema; irreversible |


| Q                           | A                                            |
| --------------------------- | -------------------------------------------- |
| Can TRUNCATE fire triggers? | Usually no                                   |
| DELETE vs DROP?             | DELETE = data only; DROP = table + data      |
| When to use TRUNCATE?       | Remove all rows quickly; triggers not needed |
| Rollback TRUNCATE?          | PostgreSQL yes; MySQL usually no             |
| Locks difference?           | DELETE = row-level; TRUNCATE = table-level   |


## Data Types
| Category          | SQL Standard                                      | Snowflake                                | Databricks / Spark SQL                    | Notes / Analogies                                      |
| ----------------- | ------------------------------------------------- | ---------------------------------------- | ----------------------------------------- | ------------------------------------------------------ |
| Integer           | `INT`, `INTEGER`, `BIGINT`, `SMALLINT`, `TINYINT` | `NUMBER(38,0)` or `INTEGER`, `BIGINT`    | `INT`, `BIGINT`, `SMALLINT`, `TINYINT`    | Whole numbers; BIGINT for large IDs, TINYINT for flags |
| Decimal / Numeric | `DECIMAL(p,s)`, `NUMERIC(p,s)`                    | `NUMBER(p,s)`                            | `DECIMAL(p,s)`                            | Precise fixed-point; used for money                    |
| Float / Real      | `FLOAT`, `REAL`, `DOUBLE`                         | `FLOAT`, `DOUBLE`                        | `FLOAT`, `DOUBLE`                         | Approximate values; caution for money                  |
| String / Text     | `CHAR(n)`, `VARCHAR(n)`, `TEXT`                   | `VARCHAR`, `STRING`                      | `STRING`                                  | Fixed/variable length; use VARCHAR for variable data   |
| Boolean           | `BOOLEAN`, `BIT`                                  | `BOOLEAN`                                | `BOOLEAN`                                 | TRUE / FALSE                                           |
| Date / Time       | `DATE`, `TIME`, `DATETIME`, `TIMESTAMP`           | `DATE`, `TIMESTAMP_NTZ`, `TIMESTAMP_LTZ` | `DATE`, `TIMESTAMP`                       | Timestamps may include timezone                        |
| Binary            | `BINARY`, `VARBINARY`                             | `BINARY`                                 | `BINARY`                                  | Storing raw bytes                                      |
| Semi-Structured   | –                                                 | `VARIANT`, `OBJECT`, `ARRAY`             | `MAP`, `ARRAY`, `STRUCT`                  | JSON / nested data types; flexible schemas             |
| UUID / GUID       | –                                                 | `STRING` or `UUID`                       | `STRING`                                  | Universally unique IDs                                 |
| Auto-Increment    | `SERIAL`, `BIGSERIAL`                             | `IDENTITY`                               | `AUTO_INCREMENT` (via sequences or Delta) | Auto-generated primary keys                            |


| Q                                         | A                                                                         |
| ----------------------------------------- | ------------------------------------------------------------------------- |
| Difference between INT, BIGINT, SMALLINT? | Size and storage; BIGINT for large numbers, SMALLINT for smaller ranges.  |
| When to use DECIMAL vs FLOAT?             | DECIMAL = precise, monetary values; FLOAT = approximate/scientific.       |
| Snowflake TIMESTAMP_NTZ vs TIMESTAMP_LTZ? | NTZ = no timezone; LTZ = local time zone aware.                           |
| Databricks ARRAY vs MAP vs STRUCT?        | ARRAY = list, MAP = key-value, STRUCT = nested record.                    |
| VARCHAR vs TEXT?                          | VARCHAR has length limit; TEXT is variable and large (depending on DBMS). |
| UUID / GUID usage?                        | Globally unique identifiers for distributed systems.                      |
| Can Boolean store NULL?                   | Yes, represents unknown/undefined.                                        |
| Auto-increment equivalents?               | Snowflake = IDENTITY, Databricks = sequences / Delta table AUTOINCREMENT. |
| When to use VARIANT/OBJECT?               | Flexible JSON-like structures; dynamic schema.                            |


| Feature         | Snowflake              | Databricks / Spark SQL            |
| --------------- | ---------------------- | --------------------------------- |
| Semi-structured | VARIANT, OBJECT, ARRAY | MAP, ARRAY, STRUCT                |
| Timezone-aware  | TIMESTAMP_LTZ          | TIMESTAMP with UTC by default     |
| Auto-increment  | IDENTITY               | AUTOINCREMENT via Delta sequences |
| Boolean storage | BOOLEAN                | BOOLEAN                           |
| String type     | VARCHAR / STRING       | STRING                            |
| Numeric         | NUMBER(p,s)            | DECIMAL(p,s), DOUBLE, FLOAT       |


