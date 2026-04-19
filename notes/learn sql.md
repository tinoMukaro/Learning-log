**SQL - structured query language**

it is a querying language used to communicate with databases

##### **DDL- data definition language**

used to define structures, tables, schemas and constraints

1\. CREATE - _to create a table_

CREATE TABLE users(

id INT

name VARCHAR

email VARCHAR);

common data types - INT, VARCHAR(n), TEXT, DATE, TIMESTAMP, BOOLEAN

2\. **ALTER** - _to alter/modify a table or column_

ALTER TABLE users ADD COLUMN age INT,

ALTER TABLE users DROP COLUMN age INT,

ALTER TABLE users ALTER COLUMN name TYPE VARCHAR(150),

3\. DROP - _DELETE everything table and data_

DROP TABLE users;

DROP DATABSE mydb;

4\. TRANCATE - _DELETE data inside the table_

TRANCATE TABLE users;

--CONSTRAINTS

PRIMARY KEY
FOREIGN KEY REFERENCES table(column)
UNIQUE
NOT NULL
CHECK (age >= 18)
DEFAULT

##### **DML - DATA MANIPULATION LANGUAGE**

1\. INSERT

INSERT INTO users(id, name, email)

VALUES(1, "TINO", "TINO@TINO.COM"),

     (2, "TIOO", "TIOO@TINO.COM"),

2\. SELECT

SELECT \* FROM users;

SELECT name, email FROM users;

3\. UPDATE

UPDATE users

SET email = 'new@tino.com'

WHERE id = 1;

4\. DELETE

DELETE FROM users WHERE id = 1

#####

##### **QUERIES**

1\. WHERE - _filters rows_

SELECT \* FROM users WHERE age > 26

SELECT \* FROM users WHERE name LIKE 'ti%',

2\. ORDER BY

SELECT \* FROM users ORDER BY created_at DESC

3\. LIMIT/OFFSET

SELECT \* FROM users LIMIT 10 OFFSET 20

4\. AGGREGATIONS

SELECT COUNT(\*) FROM USERS

SELECT SUM(\*) FROM USERS

SELECT AVG(age) FROM USERS

SELECT MAX(\*) FROM USERS

5\. GROUP BY

SELECT age, COUNT(\*)

FROM users

GROUP BY age

6\. HAVING - _filters groups_

SELECT age, COUNT(\*)

FROM users

GROUP BY age

HAVING COUNT(\*) > 2;

7\. JOINS

SELECT u.name, o.total

FROM users u

INNER JOIN orders o ON u.id = o.id

--types

&nbsp; INNER JOIN - matching rows only

&nbsp; LEFT JOIN - all left + matching

&nbsp; RIGHT JOIN - all right + matching

&nbsp; FULL JOIN - everything

8\. SUBQUERIES

SELECT \* FROM users

WHERE id IN (SELECT user_id FROM orders WHERE total > 100)

9\. EXIST

SELECT \* FROM users u

WHERE EXISTS (SELECT 1 FROM orders O WHERE o.user_id = u.id)

10\. INDEXES

CREATE INDEX idx_users_email ON users(email)

11\. TRANSACTIONS

BEGIN;

UPDATE accounts SET balance = balane - 100 WHERE id = 1;

UPDATE accounts SET balance = balane + 100 WHERE id = 23;

COMMIT

--or ROLLBACK
