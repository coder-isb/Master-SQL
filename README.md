# Master-SQL

# ZERO TO HERO – From Beginner to Advanced

A comprehensive SQL cheat sheet for developers, DBAs, and architects. Includes query types, examples, advanced topics, and best practices.

## 📑 Index

1. [SQL Query Types](#sql-query-types)
   - [DDL – Data Definition Language](#1-ddl---data-definition-language)
   - [DML – Data Manipulation Language](#2-dml---data-manipulation-language)
   - [DCL – Data Control Language](#3-dcl---data-control-language)
   - [TCL – Transaction Control Language](#4-tcl---transaction-control-language)
   - [Q&A](#query-type-q&a)
2. [Data Types](#sql-key-data-types)
3. [SQL Functions/Procedures/Triggers](#sql-other-imp-objects)
4. [SQL Keys](#keys-in-sql)
5. [SQL Keys Databricks Snowflake](#keys-databricks-&-snowflake)
2. [SQL Joins](#sql-joins)
3. [Advanced Queries](#advanced-queries)
4. [Indexing & Optimization](#indexing--optimization)
5. [ACID properties](#acid-properties)
6. [Database Design Tips](#database-design-tips)
7. [SQL Best Practices](#sql-best-practices)
8. [SQL INTERVIEW](#sql/README_SQL.md)

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
      

| Type | Purpose             | Example                                       | Notes                                    |
| ---- | ------------------- | --------------------------------------------- | ---------------------------------------- |
| DDL  | Define schema       | `CREATE TABLE emp(id INT, name VARCHAR(20));` | Changes schema; may not be rollback-able |
| DML  | Manipulate data     | `INSERT INTO emp VALUES(1,'Alice');`          | CRUD operations; can rollback            |
| DCL  | Manage access       | `GRANT SELECT ON emp TO user1;`               | Controls permissions                     |
| TCL  | Transaction control | `COMMIT; ROLLBACK;`                           | Ensures ACID compliance                  |


| Questions                       | Answers                                                |      
| ------------------------------- | ------------------------------------------------------ |
| Difference between DDL and DML? | DDL changes schema; DML manipulates data.              |
| Can DDL be rolled back?         | Usually no; depends on DBMS transactional DDL support. |
| Difference between DCL and TCL? | DCL = permissions; TCL = transaction management.       |
| Examples of DML commands?       | INSERT, UPDATE, DELETE, MERGE.                         |

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

## Query-Type-Q&A
# Truncate-Delete-Drop

| Operation | Scope                   | Speed / Logging    | WHERE | Triggers | Rollback  | Identity Reset | Notes                                |
| --------- | ----------------------- | ------------------ | ----- | -------- | --------- | -------------- | ------------------------------------ |
| TRUNCATE  | Remove all rows         | Fast, minimal      | ❌     | ❌        | ❌ Usually | ✅ Yes          | Efficient, cannot delete selectively |
| DELETE    | Remove rows selectively | Slower, row-by-row | ✅     | ✅        | ✅         | ❌              | Conditional delete; triggers fire    |
| DROP      | Remove table/database   | Immediate          | ❌     | ❌        | ❌         | ❌              | Deletes table & schema; irreversible |


| Questions                   | Answers                                      |
| --------------------------- | -------------------------------------------- |
| Can TRUNCATE fire triggers? | Usually no                                   |
| DELETE vs DROP?             | DELETE = data only; DROP = table + data      |
| When to use TRUNCATE?       | Remove all rows quickly; triggers not needed |
| Rollback TRUNCATE?          | PostgreSQL yes; MySQL usually no             |
| Locks difference?           | DELETE = row-level; TRUNCATE = table-level   |

## SQL-Key-Data-Types

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


| Questions                                 | Answers                                                                   |
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

## Keys in SQL

# 1. Primary Key

**WHAT:**
Uniquely identifies each row in a table; cannot contain NULL. Usually implemented as clustered index (SQL Server) or unique constraint.

**HOW (Common Query / Use Case):**

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT
);

**Use cases:** Row identification, relationships with foreign keys

**WHEN:**

Every table should have a primary key for data integrity

**TRADEOFFS:**

Pros: Enforces uniqueness, improves lookup performance

Cons: Cannot have NULL; clustered PK may affect insert performance

**Must-Know:**

Auto-increment columns often used for surrogate PK

Only one PK per table

**High-Value Interview Q&A**

Difference between natural key and surrogate key?
Solution:

Natural key: meaningful column(s) (e.g., email)

Surrogate key: system-generated (e.g., IDENTITY/serial)

Can PK be composite?
Solution: Yes, multiple columns can form a composite PK

# 2. Foreign Key

**WHAT:**
References primary key in another table; ensures referential integrity.

**HOW (Common Query / Use Case):**

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);

**Use cases**: Maintain relationships between tables

**WHEN:**

For enforcing integrity across related tables

**TRADEOFFS:**

Pros: Prevents orphan records, enforces consistency

Cons: Insert/update/delete performance overhead

**Must-Know:**

Can cascade updates/deletes (ON DELETE CASCADE)

Supports single or composite references

**High-Value Interview Q&A**

Difference between PK and FK?
Solution: PK: unique row identifier; FK: links to PK in another table

Can FK reference non-PK column?
Solution: Yes, but must reference a UNIQUE key

Cascading options in FK?
Solution: ON DELETE/UPDATE CASCADE, SET NULL, SET DEFAULT, NO ACTION

# 3. Unique Key

**WHAT:**
Ensures values in a column or set of columns are unique; can accept NULL (except in some DBs like SQL Server, one NULL treated as unique).

**HOW (Common Query / Use Case):**

CREATE TABLE employees (
    email VARCHAR(100) UNIQUE
);

**Use cases:** Email, SSN, or any business-unique field

**WHEN:**

To enforce business-level uniqueness apart from PK

**TRADEOFFS:**

Pros: Enforces uniqueness without being primary key

Cons: Extra index may affect insert performance

**Must-Know:**

Can have multiple unique constraints per table

Can combine columns to form composite unique key

**High-Value Interview Q&A**

Difference between PK and Unique Key?
Solution: PK: cannot be NULL, only one per table, usually clustered; Unique: can have multiple per table, allows NULL

Composite Unique Key example?
Solution:

UNIQUE (first_name, last_name)

# 4. Candidate Key

**WHAT:**
Any column or combination that can uniquely identify rows. PK is chosen from candidate keys.

**HOW (Common Query / Use Case):**

Usually defined conceptually; implement as UNIQUE or PK in SQL

**WHEN:**

During database design to identify potential primary keys

**TRADEOFFS:**

Pros: Helps choose PK

Cons: Conceptual; not directly enforced unless implemented

**Must-Know:**

A table can have multiple candidate keys; one becomes PK

**High-Value Interview Q&A**

Candidate key vs PK vs Unique key?
Solution:

Candidate key: potential unique identifier

PK: chosen candidate key

Unique key: enforces uniqueness but not PK

# 5. Alternate Key

**WHAT:**
Candidate key not chosen as primary key; implemented as unique constraint.

**HOW (Common Query / Use Case):**

CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);

PK: employee_id, Alternate Key: email

**WHEN:**

Maintain uniqueness for secondary identifiers

**TRADEOFFS:**

Pros: Enforces additional uniqueness

Cons: Extra index overhead

**Must-Know:**

Each alternate key is implemented as UNIQUE constraint

**High-Value Interview Q&A**

Alternate key vs Unique key?
Solution: Alternate key is a candidate key not chosen as PK; implemented via UNIQUE

# 6. Super Key

**WHAT:**
Any set of columns that uniquely identifies a row (may contain extra columns).

**HOW (Common Query / Use Case):**

Conceptual; includes PK + any additional columns

**WHEN:**

Used conceptually during normalization and design

**TRADEOFFS:**

Pros: Ensures uniqueness

Cons: May include redundant columns

**Must-Know:**

PK is minimal super key

High-Value Interview Q&A

Super Key vs Candidate Key?
Solution: Candidate key = minimal super key (no extra columns)

# 7. Composite / Compound Key

**WHAT:**
Primary key or unique key formed from multiple columns.

**HOW (Common Query / Use Case):**

CREATE TABLE enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
);

**Use cases:** Many-to-many relationships

**WHEN:**

When a single column is insufficient for uniqueness

**TRADEOFFS:**

Pros: Enforces row uniqueness across multiple attributes

Cons: More complex joins, indexing considerations

**Must-Know:**

Can be PK or UNIQUE

Used frequently in junction tables

**High-Value Interview Q&A**

Difference between composite and single-column PK?
Solution: Composite PK combines columns for uniqueness; single-column PK uses only one column

| Key Type  | Can be NULL | Unique? | Number per Table | Notes                               |
| --------- | ----------- | ------- | ---------------- | ----------------------------------- |
| Primary   | No          | Yes     | 1                | Clustered by default                |
| Foreign   | Yes         | No      | Multiple         | Enforces referential integrity      |
| Unique    | Yes         | Yes     | Multiple         | Allows NULL (DB-specific)           |
| Candidate | Yes         | Yes     | Multiple         | PK chosen from candidate keys       |
| Alternate | Yes         | Yes     | Multiple         | Candidate key not chosen as PK      |
| Super     | Yes         | Yes     | Conceptual       | May have extra columns              |
| Composite | Depends     | Yes     | Multiple         | PK or Unique using multiple columns |

## Keys Databricks & Snowflake

# 1. Databricks / Delta Lake Keys

**WHAT:**

Databricks (Delta Lake) does not enforce primary or foreign key constraints at the storage level.

Constraints can be declared for documentation/optimization, but enforcement is not guaranteed.

**HOW (Common Usage / Workarounds):**

-- Declare a primary key (informational only)
CREATE TABLE employees (
  employee_id INT NOT NULL,
  name STRING,
  PRIMARY KEY (employee_id)
) USING DELTA;

To enforce uniqueness: Use MERGE INTO or deduplicate using ROW_NUMBER()

Foreign key references must be handled via application logic or join validation

**WHEN:**

Large distributed tables; enforcement at write time is expensive

Ensuring uniqueness or referential integrity via ETL pipelines

**TRADEOFFS:**

Pros: Faster writes, scalable distributed architecture

Cons: No built-in FK enforcement; user must handle data integrity

**Must-Know:**

Delta Lake supports NOT NULL constraints

Uniqueness and referential integrity must be enforced programmatically

Optimizations: Z-ordering, partitioning for fast lookups

**High-Value Interview Q&A**

Does Databricks support primary key enforcement?
Solution: No, PK is declarative only. Use deduplication or MERGE for enforcement.

How to enforce foreign key constraints in Delta Lake?
Solution: Application-level enforcement or use SQL joins to validate data before insert.

What about uniqueness constraints?
Solution: Delta Lake supports NOT NULL; UNIQUE constraints are not enforced natively.

# 2. Snowflake Keys

**WHAT:**

Snowflake allows declaration of primary, foreign, and unique keys but does not enforce them.

Keys are mainly informational for query optimization, documentation, and DDL semantics.

**HOW (Common Usage / Example):**

CREATE TABLE employees (
  employee_id INT PRIMARY KEY,
  email VARCHAR(100) UNIQUE,
  department_id INT REFERENCES departments(department_id)
);

Snowflake does not prevent duplicate values in PK or Unique columns

Foreign key constraints do not enforce referential integrity

**WHEN:**

Declarative constraints help maintain schema semantics

Enforcement handled at ETL or application level

**TRADEOFFS:**

Pros: Lightweight; no runtime constraint overhead

Cons: Data integrity must be enforced outside Snowflake

**Must-Know:**

Use clustered keys or materialized views to optimize queries instead of relying on constraints

PK/FK used for metadata, documentation, and query planning only

**High-Value Interview Q&A**

Does Snowflake enforce primary or foreign keys?
Solution: No, PK/FK are informational only.

How to ensure uniqueness in Snowflake?
Solution: Use SELECT DISTINCT or deduplication during ETL / MERGE operations

Why declare keys if not enforced?
Solution: Helps documentation, understanding schema relationships, query optimizer may leverage metadata

# 3. Comparative Notes – Keys in Databricks vs Snowflake

| Feature              | Databricks / Delta Lake         | Snowflake                                 |
| -------------------- | ------------------------------- | ----------------------------------------- |
| Primary Key          | Declarative only, not enforced  | Declarative only, not enforced            |
| Foreign Key          | Not enforced; use ETL/app logic | Not enforced; metadata only               |
| Unique Key           | Not enforced; use deduplication | Not enforced; use ETL/app logic           |
| NOT NULL             | Enforced                        | Enforced                                  |
| Performance impact   | None at write time              | None at write time                        |
| Use case             | Distributed tables, big data    | Cloud data warehouse, analytics pipelines |
| Enforcement strategy | Application / ETL validation    | Application / ETL validation              |


**Key Takeaways for Interviews:**

Databricks: Keys are mostly for documentation; integrity must be handled programmatically.

Snowflake: Keys are declarative; no enforcement; rely on ETL or business logic.

NOT NULL is the only truly enforced constraint on both platforms.

Always emphasize data integrity via ETL or merge operations during interviews.


## SQL Other Important Objects

## Views
| Type              | Purpose        | Example                                                                                    | Notes                                     |
| ----------------- | -------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------- |
| View              | Virtual table  | `CREATE VIEW emp_view AS SELECT id,name FROM emp;`                                         | Always up-to-date; read-only if complex   |
| Materialized View | Physical table | `CREATE MATERIALIZED VIEW emp_mv AS SELECT dept_id,SUM(salary) FROM emp GROUP BY dept_id;` | Needs refresh; improves query performance |

| Q                              | A                                         |
| ------------------------------ | ----------------------------------------- |
| Can you update a view?         | Only if updatable (no aggregates, joins). |
| When to use materialized view? | Performance on large aggregates.          |
| View vs Table?                 | View = virtual; Table = physical data.    |
| REFRESH MATERIALIZED VIEW?     | Updates stored results.                   |

| Type      | Purpose          | Example                                                                                           | Notes                  |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------- | ---------------------- |
| Procedure | Performs actions | `CREATE PROCEDURE GetEmpByDept(IN deptId INT) BEGIN SELECT * FROM emp WHERE dept_id=deptId; END;` | May not return value   |
| Function  | Returns value    | `CREATE FUNCTION GetTotalSalary() RETURNS INT BEGIN RETURN (SELECT SUM(salary) FROM emp); END;`   | Can be used in queries |

| Questions                                   | Answers                                                      |
| ------------------------------------------- | ------------------------------------------------------------ |
| Procedure vs Function?                      | Functions return value; procedures may not.                  |
| Can procedures return multiple result sets? | Yes, depending on DBMS.                                      |
| When to use stored procedures?              | Encapsulate logic, reduce network calls, enforce security.   |
| Deterministic function?                     | Always returns same output for same input; affects indexing. |


## SQL Joins

| Join       | Description                                   |
| ---------- | --------------------------------------------- |
| INNER      | Only matching rows                            |
| LEFT       | All left rows + NULLs for unmatched right     |
| RIGHT      | All right rows + NULLs for unmatched left     |
| FULL OUTER | All rows; unmatched sides NULL                |
| CROSS      | Cartesian product                             |
| NATURAL    | Joins on columns with same name automatically |


| Questions                   | Answers                                                                        |
| --------------------------- | ------------------------------------------------------------------------------ |
| NATURAL JOIN vs INNER JOIN? | NATURAL automatically joins on same-named columns; INNER requires explicit ON. |
| INNER vs FULL OUTER JOIN?   | INNER = only matching; FULL OUTER = all rows, unmatched sides NULL.            |
| LEFT vs RIGHT?              | LEFT = all left rows, unmatched right NULL; RIGHT = opposite.                  |
| CROSS JOIN use case?        | Cartesian product; generating combinations.                                    |
| How do NULLs behave?        | NULL does not match anything; appears as NULL in OUTER joins.                  |



## Indexing
Indexes speed queries like a book index. Clustered (reorders table), non-clustered (separate). Privileges: GRANT/REVOKE for access. SQL Injection: Malicious input exploiting queries.

**Best Practices:**

Index frequently queried columns, but not all—updates slow them.
Use prepared statements to prevent injection.
Least privilege principle for users.

**Examples**
Index:

CREATE INDEX idx_name ON customers (name);

| Type          | Purpose                 | Example                                               | How it Works / Analogy                                           | Trade-offs                                        |
| ------------- | ----------------------- | ----------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------- |
| B-Tree        | Equality & range        | `CREATE INDEX idx_name ON emp(name);`                 | Tree nodes → leaf nodes point to rows; like **dictionary pages** | Good range/equality; slows inserts/updates        |
| Hash          | Exact match             | `CREATE INDEX idx_hash ON emp(id) USING HASH;`        | Hash function → bucket → pointer; like **warehouse bin**         | Fast equality only; no range                      |
| Composite     | Multi-column            | `CREATE INDEX idx_comp ON emp(dept_id,salary);`       | Columns concatenated; order matters                              | Efficient multi-column queries; larger index size |
| Unique        | Enforce uniqueness      | `CREATE UNIQUE INDEX idx_uid ON emp(email);`          | Prevent duplicates                                               | Slower inserts                                    |
| Full-Text     | Text search             | `CREATE FULLTEXT INDEX idx_text ON emp(description);` | Index words; like **search engine index**                        | Storage-heavy; optimized search                   |
| Clustered     | Rows physically ordered | `CREATE CLUSTERED INDEX idx_clustered ON emp(id);`    | Table sorted by key; only 1 per table; like **phonebook**        | Fast range queries; slows inserts                 |
| Non-Clustered | Separate structure      | `CREATE NONCLUSTERED INDEX idx_noncl ON emp(name);`   | Index → pointer to row; like **library card catalog**            | Multiple allowed; extra storage                   |

| Questions                           | Answers                                                                          |
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








## Advanced-Queries

| Feature             | Purpose                   | Example                                                                                                       | Notes                                          |
| ------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| Subquery            | Query inside query        | `SELECT name FROM emp WHERE dept_id=(SELECT id FROM dept WHERE name='HR');`                                   | Returns scalar or table                        |
| Correlated Subquery | References outer query    | `SELECT e1.name FROM emp e1 WHERE e1.salary>(SELECT AVG(e2.salary) FROM emp e2 WHERE e1.dept_id=e2.dept_id);` | Runs per outer row                             |
| Aggregate           | SUM, COUNT, AVG, MIN, MAX | `SELECT COUNT(*) FROM emp;`                                                                                   | Used with GROUP BY                             |
| GROUP BY / HAVING   | Aggregate grouping        | `SELECT dept_id, COUNT(*) FROM emp GROUP BY dept_id HAVING COUNT(*)>5;`                                       | HAVING filters after aggregation               |
| Window Functions    | Analytics per partition   | See below                                                                                                     | ROW_NUMBER, RANK, DENSE_RANK, NTILE, LEAD, LAG |
| CTE                 | Common Table Expression   | `WITH cte AS (SELECT * FROM emp) SELECT * FROM cte;`                                                          | Improves readability, supports recursion       |

# 1. CTE – Common Table Expressions

**WHAT:**
A named temporary result set for modular, readable queries. Supports recursion for hierarchical data.

**HOW (Common Query / Use Case):**

WITH active_emps AS (
    SELECT employee_id, department_id, salary
    FROM employees
    WHERE status='active'
)
SELECT * FROM active_emps;

**Use cases:** Employee hierarchy, top-N per department, ETL intermediate transformations.

**WHEN:**

Complex multi-step queries

Reusing intermediate results

Recursive queries (hierarchies, graphs)

**TRADEOFFS:**

**Pros:** Readable, modular, supports recursion

**Cons:** Non-materialized CTEs recompute on multiple references → slower for large datasets

**Must-Know:**

Chain multiple CTEs

Recursive CTEs for tree traversal

Supported in most SQL engines

**High-Value Interview Q&A**

CTE vs Subquery vs Temp Table – when to use which?
Solution:

CTE: readability, recursion

Subquery: simple inline filtering

Temp Table: materialized, can be indexed, persists for session

When can CTE hurt performance?
Solution: Multiple references to non-materialized CTE → recomputation

Recursive CTE infinite loop prevention?
Solution: Use MAXRECURSION or depth limit; track path to detect cycles

# 2. Recursive CTE

**WHAT:**
CTE referencing itself to traverse unknown-depth hierarchies or graphs.

**HOW (Common Query / Use Case):**

WITH RECURSIVE r AS (
    SELECT id, parent_id, 0 AS level FROM employees WHERE parent_id IS NULL
    UNION ALL
    SELECT e.id, e.parent_id, r.level + 1
    FROM employees e
    JOIN r ON e.parent_id = r.id
)
SELECT * FROM r;

**Use cases:** Employee hierarchy, BOM expansion, folder structures, project task dependencies

**WHEN:**

Unknown-depth hierarchies

Multi-level aggregation

**TRADEOFFS:**

Pros: Elegant for hierarchy queries

Cons: Risk of infinite loops, deep recursion slow, limited by engine max depth

**Must-Know:**

Add a path column to detect cycles

Can combine with ranking functions for hierarchical order

**High-Value Interview Q&A**

Detect cycles in hierarchy?
Solution: Track path in recursion, filter duplicates

Recursive CTE vs procedural loop?
Solution: Recursive CTE: declarative, portable; Loop: procedural, verbose

Performance tips for recursive queries?
Solution: Index parent-child columns, limit recursion depth

**3. Window Functions**

**WHAT:**
Compute metrics over rows related to the current row without collapsing rows.

**HOW (Common Query / Use Case):**

ROW_NUMBER() OVER(PARTITION BY department ORDER BY performance DESC) AS rank
SUM(salary) OVER(PARTITION BY department) AS dept_total_salary
LAG(salary) OVER(PARTITION BY department ORDER BY join_date) AS prev_salary


**Use cases:** Top-N per group, running totals, lead/lag, sessionization, percentiles

**WHEN:**

Analytics without losing row-level detail

Ranking and cumulative metrics

**TRADEOFFS:**

Pros: Powerful, flexible

Cons: Sorting large datasets is resource-intensive

**Must-Know:**

ROW_NUMBER vs RANK vs DENSE_RANK

ROWS vs RANGE frame

Combine with PARTITION BY and ORDER BY for granular control

**High-Value Interview Q&A**

Difference between ROW_NUMBER, RANK, DENSE_RANK?
Solution:

ROW_NUMBER: unique per row

RANK: same rank for ties, skips numbers

DENSE_RANK: same rank for ties, no skipped numbers

How to sessionize using LAG?
Solution: Compare current row timestamp with previous row using LAG, start new session if gap > threshold

Performance tips?
Solution: Index ORDER BY columns, minimize large frame calculations

# 4. Pivot / Unpivot

**WHAT:**
Pivot: convert rows → columns; Unpivot: columns → rows.

**HOW (Common Query / Use Case):**

-- Pivot
SELECT product,
       SUM(CASE WHEN month='Jan' THEN amount END) AS Jan,
       SUM(CASE WHEN month='Feb' THEN amount END) AS Feb
FROM sales
GROUP BY product;

-- Unpivot
SELECT product, month, amount
FROM sales_table
UNPIVOT (amount FOR month IN (Jan, Feb, Mar)) u;

Use cases: Sales/Revenue reports, KPI dashboards, wide-to-long data transformation

**WHEN:**

Reporting, dashboard aggregation

**TRADEOFFS:**

Pros: Structured reporting, easy to read

Cons: Memory heavy, dynamic pivot requires dynamic SQL

**Must-Know:**

Conditional aggregation can replace pivot

Use for small fixed sets of pivot columns

**High-Value Interview Q&A**

Pivot vs conditional aggregation?
Solution: Pivot is syntactic sugar; both produce same results

Dynamic pivoting?
Solution: Build SQL dynamically using procedural code (EXEC in SQL Server, PL/pgSQL)

Performance tips?
Solution: Limit pivot columns, avoid wide tables, consider pre-aggregated data

# 5. CASE / SWITCH Logic

**WHAT:**
Conditional expression to implement IF/ELSE logic in SQL.

**HOW (Common Query / Use Case):**

CASE
  WHEN salary > 100000 THEN 'High'
  WHEN salary > 50000 THEN 'Medium'
  ELSE 'Low'
END AS salary_band

**Use cases:** Salary bands, KPI classification, conditional metrics, data cleaning

**WHEN:**

Categorization, conditional aggregation

**TRADEOFFS:**

Pros: Flexible, widely used

Cons: Can prevent index usage in WHERE

**Must-Know:**

NULL handling

Nested CASE statements

**High-Value Interview Q&A**

NULL handling in CASE?
Solution: Use WHEN col IS NULL explicitly

Conditional aggregation example?
Solution:

SELECT SUM(CASE WHEN status='Active' THEN 1 ELSE 0 END) AS active_count
FROM employees;

# 6. Subqueries / Correlated Subqueries

**WHAT:**
Nested queries inside another query. Correlated subqueries reference outer query columns.

**HOW (Common Query / Use Case):**

-- Simple
SELECT name FROM employees WHERE department_id IN (
  SELECT id FROM departments WHERE location='NY'
);

-- Correlated
SELECT e.name FROM employees e
WHERE e.salary > (
  SELECT AVG(salary) FROM employees WHERE department_id = e.department_id
);


**Use cases:** Top performers, filtering by aggregated metrics, validation

**WHEN:**

Row-level dependent filters, dynamic thresholds

**TRADEOFFS:**

Correlated subqueries expensive; often replaced by JOIN/CTE

**Must-Know:**

EXISTS vs IN vs ANY/ALL

**High-Value Interview Q&A**

Correlated vs uncorrelated subquery?
Solution: Correlated executes per row; uncorrelated executes once

Optimize correlated subquery?
Solution: Replace with JOIN or CTE; ensure indexes

EXISTS vs IN?
Solution: EXISTS often faster for correlated checks; IN can be slower with large sets

# 7. Aggregate Functions

**WHAT:**
SUM, COUNT, AVG, MIN, MAX — summarize data across rows

**HOW (Common Query / Use Case):**

SELECT COUNT(*) AS emp_count, AVG(salary) AS avg_salary FROM employees;
SELECT department_id, SUM(salary) AS total_salary FROM employees GROUP BY department_id;

**Use cases:** Department metrics, revenue totals, operational reports

**WHEN:**

Reporting, dashboard metrics, ETL aggregation

**TRADEOFFS:**

Aggregate collapses rows; use window functions for row-level detail

**Must-Know:**

COUNT(*) vs COUNT(col)

NULL handling

AVG/SUM skips NULL

**High-Value Interview Q&A**

COUNT(*) vs COUNT(col)?
Solution: COUNT(*) counts all rows; COUNT(col) counts non-NULL only

Aggregate vs window functions?
Solution: Aggregate collapses rows; window preserves row-level

# 8. GROUP BY / HAVING

**WHAT:**
GROUP BY aggregates rows; HAVING filters aggregated results

**HOW (Common Query / Use Case):**

SELECT department_id, COUNT(*) AS emp_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

**Use cases:** Department metrics, KPI filtering, dashboards

**WHEN:**

Summarized reporting, pre-aggregation

**TRADEOFFS:**

Mixing non-aggregated columns causes errors

Large datasets → performance cost

**Must-Know:**

WHERE filters rows before aggregation; HAVING filters after aggregation

**High-Value Interview Q&A**

WHERE vs HAVING?
Solution: WHERE → row-level; HAVING → aggregated group-level

Selecting non-aggregated columns without GROUP BY?
Solution: SQL error; must include in GROUP BY

Performance tips?
Solution: Index grouping columns, pre-aggregate if possible

## Window Functions

| Function     | Purpose                  | Example                                                        | Notes                                      |
| ------------ | ------------------------ | -------------------------------------------------------------- | ------------------------------------------ |
| ROW_NUMBER() | Unique sequential number | `ROW_NUMBER() OVER(PARTITION BY dept_id ORDER BY salary DESC)` | Unique per row; ties get different numbers |
| RANK()       | Ranking with gaps        | `RANK() OVER(PARTITION BY dept_id ORDER BY salary DESC)`       | Ties same rank; gaps after tie             |
| DENSE_RANK() | Ranking without gaps     | `DENSE_RANK() OVER(PARTITION BY dept_id ORDER BY salary DESC)` | Ties same rank; no gaps                    |
| NTILE(n)     | Divide into n buckets    | `NTILE(4) OVER(ORDER BY salary)`                               | Quartiles / percentiles                    |
| LEAD()       | Access next row          | `LEAD(salary) OVER(ORDER BY salary)`                           | Compare with next row                      |
| LAG()        | Access previous row      | `LAG(salary) OVER(ORDER BY salary)`                            | Compare with previous row                  |

| Questions                      | Answers                                                   |
| ------------------------------ | --------------------------------------------------------- |
| RANK() vs DENSE_RANK()?        | RANK has gaps after ties; DENSE_RANK has none.            |
| ROW_NUMBER() vs RANK()?        | ROW_NUMBER always unique; RANK may tie.                   |
| NTILE() use case?              | Dividing into quartiles/quintiles.                        |
| LEAD/LAG advantage?            | Compare rows without self-join.                           |
| Scalar vs correlated subquery? | Scalar = single value; correlated = depends on outer row. |

## ACID
ACID Properties – SQL & Modern Platforms (Databricks/Snowflake)
1. Atomicity

What:

Transaction is all-or-nothing; either all operations succeed or none.

How / Example:

BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;


If any step fails → ROLLBACK occurs.

When: Multi-step operations like bank transfers, multi-table updates.

Tradeoffs: Logging overhead, slightly slower writes.

2. Consistency

What:

Database moves from one valid state to another, obeying all constraints, rules, and business logic.

Simple Example:

Total money in bank before and after a transfer remains constant.

Inventory stock never goes negative.

When: Always, especially in multi-user environments.

Tradeoffs: Strict consistency can slow distributed or bulk operations.

High-Value Interview Q&A:

Consistency vs constraints?

Constraints enforce rules; ACID Consistency ensures all rules + business logic remain valid after transaction.

Banking example: Total system balance remains constant after transfers.

3. Isolation

What:

Ensures concurrent transactions do not interfere; results are as if executed sequentially.

Isolation Levels

| Level            | Description                      | Dirty Read | Non-Repeatable Read | Phantom Read | Use / Tradeoff                    |
| ---------------- | -------------------------------- | ---------- | ------------------- | ------------ | ----------------------------------|
| Read Uncommitted | Reads uncommitted data           | ✅          | ✅                   | ✅            | Fastest, but risky            |
| Read Committed   | Reads only committed data        | ❌          | ✅                   | ✅            | Default, safer                |
| Repeatable Read  | Same row reads consistent        | ❌          | ❌                   | ✅            | Prevents non-repeatable reads |
| Serializable     | Full isolation; serial execution | ❌          | ❌                   | ❌            | Safest, lowest concurrency    |

Key Idea:

Dirty read → read uncommitted data

Non-repeatable → row changes

Phantom → new/deleted rows appear

Platform Notes:

Databricks / Delta Lake: Optimistic concurrency control

Snowflake: Multi-Version Concurrency Control (MVCC)

Higher isolation → less concurrency, more overhead

High-Value Q&A:

Explain phantom read. → New rows appear in a repeated query.

How does MVCC help? → Each transaction sees a snapshot; prevents dirty/non-repeatable reads.

4. Durability

What:

Committed transactions persist permanently, even after a crash.

How / Platform Notes:

Traditional RDBMS: Write-ahead logs, disk commit

Databricks / Delta Lake: Transaction logs + atomic file operations

Snowflake: Persistent cloud storage + automatic replication

When: Always; ensures reliability.

Tradeoffs: Slight commit latency; storage overhead for logs.

High-Value Q&A:

How is durability ensured in cloud platforms?

Delta Lake: transaction logs + replicated storage

Snowflake: cloud storage with replication/failover

| Property    | Key Idea                     | Platform Notes                                                 |
| ----------- | ---------------------------- | -------------------------------------------------------------- |
| Atomicity   | All or nothing               | RDBMS: rollback; Delta: MERGE; Snowflake: transactional commit |
| Consistency | Database rules always valid  | Constraints + business logic                                   |
| Isolation   | Concurrent transactions safe | MVCC / OCC; know isolation levels & anomalies                  |
| Durability  | Committed data persists      | Logs, atomic ops, replication                                  |

## SQL Best Practices

SQL Best Practices – Extended
1. SQL Writing Best Practices

Key Guidelines:

**Avoid SELECT *** → fetch only needed columns

Use aliases for readability (emp e instead of employees)

Avoid unnecessary subqueries → prefer JOINs or CTEs

Use CTEs for complex queries → improves readability and maintainability

Use parameterized queries → prevents SQL injection

Avoid functions on indexed columns → e.g., WHERE YEAR(date) = 2025 → use range filter

Common Pitfalls:

SELECT * → performance issues

Functions in WHERE → prevents index usage

Multiple nested subqueries → slow execution

High-Value Interview Q&A:

*Why avoid SELECT ?

Reduces I/O, improves query performance, fetches only needed data.

CTE vs Temp Table?

CTE is recomputed; temp table persists and can be indexed for repeated use.

2. Joins – Best Practices

Key Guidelines:

Prefer INNER JOIN unless outer join is required

Use ON conditions correctly; avoid Cartesian products

For large tables: consider broadcast join (Databricks) or small dimension table

Use EXISTS instead of IN for large subqueries

Common Pitfalls:

Forgetting join condition → Cartesian product

Joining large tables without proper filtering → slow queries

Using functions on join columns → prevents index usage

High-Value Interview Q&A:

Difference between INNER, LEFT, RIGHT, FULL OUTER JOIN?

INNER: only matching rows

LEFT/RIGHT: keep all from left/right, match if exists

FULL: keep all from both, NULL if no match

How to optimize join on large tables?

Filter before join, use appropriate join type, consider partitioning/clustering.

3. GROUP BY / Aggregation Best Practices

Key Guidelines:

Aggregate only required columns

Avoid GROUP BY on high-cardinality columns unless necessary

Use HAVING for post-aggregation filtering, WHERE for pre-aggregation filtering

Use window functions if aggregation per group is needed without collapsing rows

Common Pitfalls:

GROUP BY on functions → slow, prevents index usage

HAVING instead of WHERE → filters applied after aggregation

High-Value Interview Q&A:

Difference between WHERE and HAVING?

WHERE → filter before aggregation

HAVING → filter after aggregation

When to use window functions vs GROUP BY?

Window: keep row-level data while calculating aggregates

GROUP BY: collapses rows into aggregated result

4. Debugging Slow Queries

Key Steps:

Check execution plan (EXPLAIN / DESCRIBE)

Identify table scans, missing indexes, or expensive joins

Look for unnecessary full table scans

Check joins

Ensure small table broadcasted in distributed systems

Filter before join

Check indexes

Missing or non-optimal indexes cause scans

Check aggregations

GROUP BY on high-cardinality columns → slow

Use pre-aggregated tables if possible

Check large IN / OR conditions → consider JOIN or EXISTS

Platform-Specific Tips:

Databricks:

Use OPTIMIZE, Z-ORDER, cache intermediate results

Avoid shuffling large tables unnecessarily

Snowflake:

Check micro-partitioning, clustering keys

Use materialized views for repeated heavy queries

High-Value Interview Q&A:

How do you find slow queries in a production DB?

Use EXPLAIN, execution stats, query profiling, logs.

How to optimize large table JOIN in Databricks?

Broadcast small table, filter before join, cache intermediate result.

Why is GROUP BY slow on large dataset?

High-cardinality, no partitioning, aggregations require shuffling.

5. Summary – SQL Writing & Performance Tips
Area	Best Practices	Common Pitfalls



