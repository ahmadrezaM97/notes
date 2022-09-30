


By far the simplest and most common technique for adding a primary key in Postgres is by using the `SERIAL` or `BIGSERIAL` data types when `CREATING` a new table. As indicated in the [official documentation](https://www.postgresql.org/docs/8.1/datatype.html#DATATYPE-SERIAL), `SERIAL` is not a true data type, but is simply shorthand notation that tells Postgres to create a auto incremented, unique identifier for the specified column.

```sql
CREATE TABLE IF NOT EXISTS tweet (
    id SERIAL PRIMARY KEY,
    txt VARCHAR(256) NOT NULL,
    created_at TIMESTAMP NOT NULL 
);
```

-> `\d` for geting list of tables



This command will create two files with up and down at the suffix of the created sql files

As we enabled `seq` flag the files are created with version number

```bash
migrateup:
	migrate -path ./internals/db/migrations -database "postgresql://psql:pass@localhost:5432/psql?sslmode=disable" -verbose up

migratedown:
	migrate -path ./internals/db/migrations -database "postgresql://psql:pass@localhost:5432/psql?sslmode=disable" -verbose down

addmigration:
	migrate create -ext sql -dir ./internals/db/migrations -seq playground_schema

```


#### POSTGRESQL BASICS


#### postgresql important data types
1. Character types (char, varchar and text)
	1.` varchar(n)`: variable-length with limit
	2. `text`: unlimited
	3. `char(n)`, fixed-length, black padded
2. Numberic types( integer, floating-point)
	1. integer(4 bytes) | smallint(2 bytes) | bigint(8b ytes)
	2. real (4 bytes)
	3. serial (4 bytes) -> auto incrementing integer
	4. bigserial ( 8 bytes) -> auto incrementing integer
3. Boolean
	1. boolean (1byte)
4. ENUM
	1. `CREATE TYPE week as ENUM ('SAT','TUE')`
5. Geometric
6. Network Address
7. Text Search
8. UUID 
9. Array
10. JSON

### Some operations
```sql
CREATE DATABASE [IF EXISTS] dbname;
DROP DATABASE [IF EXISTS] dbname;

CREATE TABLE table_name (
	col1 datatype,
	col2 datatype,
);

INSERT INTO table_name(col1, col2, .. coln) values(val1, DEFAULT, .. ,valn);
SELECT col1, col2 from tablename;
SELECT * FROM COMPANY WHERE salary = 1000;
SELECT COUNT(*) AS "RECORDS" FROM COMPANY;
SELECT * FROM COMPANY WHERE AGE IS NOT NULL;
SELECT * FROM COMPANY WHERE AGE > 10 AND AGE < 30;
SELECT * FROM COMPANY WHERE NAME LIKE "GOO%";
SELECT * FROM CAMPNY WHERE AGE IN (24, 26);
SELECT * FROM CAMPNY WHERE NOT AGE IN (24, 26);
SELECT * FROM CAMPNY WHERE  AGE BETWEEN 24 AND 26;

SELECT * FROM COMPANY WHERE EXIST (SELECT AGE FROM CAOMPNY WHERE SALARY > 20);
UPDATE COMPANY SET NAME = 'DIVAR' WHERE ID = 1;

DELETE FROM COMAPNY WHERE ID = 29;

SELECT * FROM table_name WHERE LIMIT [n of row]

SELECT * FROM table_name where condition order by col1,col2,.. [ASC | DESC]

```

* Get available database with `\l`
* connect to a database `\c testdb`
* `psql -h localhost -p 5432 -U postgress testdb`
* list tables `\d`
* describe table `\d dbname`

foregnkey
```sql

CREATE TABLE categories (
	category_id serial PRIMARY KEY,
	category_name VARCHAR (255) NOT NULL
);

CREATE TABLE products (
	product_id serial PRIMARY KEY,
	product_name VARCHAR (255) NOT NULL,
	category_id INT NOT NULL,
	FOREIGN KEY (category_id) REFERENCES categories (category_id)
);

-- natural join
SELECT * FROM products NATURAL JOIN categories;

```

https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-foreign-key/

#### JOINS
* CROSS JOIN
* INNER JOIN
* LEFT OUTER JOIN
* RIGHT OUTER JOIN
* FULL JOIN


#TODO: schema | view
#TODO: change this note
```ad-note
title: TEXT vs VARCHAR

the only difference between TEXT and VARCHAR(n) is that you can limit the maximum length of a varchar


```

### Constraints
1. `NOT NULL`  
	1. ensures that a column cannot have `NULL` value.
2. `UNIQUE`
	1. ensures that all values in a column are different
3. `PRIMARY Key`
	1. Uniquely identifies each row/record in a table
4. `FOREIGN Key`
	1. Constraint data based on columns in other table
5. `CHECK`
	1. The check constraint ensures that all values in a column satisfy certain conditions.
6. `EXCLUSION`
	1. ensures that if any two rows are compared on the specified column(s) or expression(s) using the specified operator(s), not all of the comparisons  will return true

``` sql
CREATE TABLE NAME (
	ID SERIAL PRIMARY KEY NOT NULL,
	AGE INT NOT NULL,
	NAME TEXT NOT NULL UNIQUE,
	SALARY REAL
);


CREATE TABLE HOME (
	ID SERIAL PRIMARY KEY NOT NULL,
	ADDRESS CHAR(20),
	OWNER INT references NAME(ID)
);

```



Constraints are the rules enforced on data columns on table
these are used to prevent invalid data from being interned  into database
-> this ensures the accuracy and reliability of the data in the database.