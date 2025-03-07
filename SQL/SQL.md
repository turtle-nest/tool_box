# SQL debut

## Introduction to Databases

### 1. What is a Database?
A database is an organized system that allows efficient storage, management, and retrieval of data. Data is typically structured in tables with rows (records) and columns (fields).

**Example:** A library database might contain a table of books with the following columns:
| ID  | Title            | Author          | Year |
|-----|-----------------|----------------|------|
| 1   | 1984             | George Orwell  | 1949 |
| 2   | Brave New World  | Aldous Huxley  | 1932 |


### 2. What is a Relational Database?
A relational database organizes data into tables linked by relationships. These relationships are often based on keys:
- **Primary Key:** A unique identifier for a row in a table.
- **Foreign Key:** A column referencing the primary key of another table.

**Example:** A table of authors and a table of books linked by a foreign key:
| Author_ID | Name           | Country |
|-----------|----------------|---------|
| 1         | George Orwell  | UK      |
| 2         | Aldous Huxley  | UK      |

| Book_ID | Title            | Author_ID |
|---------|------------------|-----------|
| 1       | 1984              | 1         |
| 2       | Brave New World   | 2         |


## SQL and MySQL Basics

### 3. What is SQL?
SQL (Structured Query Language) is a standardized language used to interact with relational databases. It allows:
- Creating and modifying databases and tables
- Inserting, updating, and deleting data
- Retrieving and manipulating data through queries


### 4. What is MySQL?
MySQL is an open-source Relational Database Management System (RDBMS) based on SQL. It’s widely used for web applications and projects requiring robust databases.


## SQL Commands

### 5. Creating a Database in MySQL
```sql
CREATE DATABASE library;
```

Selecting a database:
```sql
USE library;
```


### 6. DDL and DML Definitions
- **DDL (Data Definition Language):** Defines and modifies the structure of databases and tables.
  - `CREATE`: Creates databases and tables.
  - `ALTER`: Modifies table structure.
  - `DROP`: Deletes databases or tables.

- **DML (Data Manipulation Language):** Manipulates data stored in tables.
  - `INSERT`: Adds records.
  - `UPDATE`: Modifies records.
  - `DELETE`: Removes records.


### 7. Creating and Modifying a Table
Creating a table:
```sql
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(50),
    year INT
);
```

Adding a column:
```sql
ALTER TABLE books ADD genre VARCHAR(20);
```

Modifying a column type:
```sql
ALTER TABLE books MODIFY year YEAR;
```

Dropping a column:
```sql
ALTER TABLE books DROP COLUMN genre;
```


### 8. Selecting Data
Selecting all columns:
```sql
SELECT * FROM books;
```

Selecting specific columns:
```sql
SELECT title, author FROM books;
```

Filtering results:
```sql
SELECT * FROM books WHERE year > 1950;
```

Sorting results:
```sql
SELECT * FROM books ORDER BY year DESC;
```

Grouping results:
```sql
SELECT author, COUNT(*) FROM books GROUP BY author;
```


### 9. Inserting, Updating, and Deleting Data
Inserting data:
```sql
INSERT INTO books (title, author, year)
VALUES ('1984', 'George Orwell', 1949);
```

Updating data:
```sql
UPDATE books
SET year = 1950
WHERE title = '1984';
```

Deleting data:
```sql
DELETE FROM books WHERE year < 1930;
```


### 10. Subqueries
A subquery is a query nested inside another query:
```sql
SELECT title FROM books
WHERE author IN (
    SELECT author FROM books WHERE year > 1950
);
```


## MySQL Functions

### 11. Aggregation Functions
- `COUNT()`: Counts rows.
- `SUM()`: Calculates sum.
- `AVG()`: Calculates average.
- `MIN()`: Finds minimum value.
- `MAX()`: Finds maximum value.

```sql
SELECT COUNT(*) FROM books;
SELECT AVG(year) FROM books;
```


### 12. Text Functions
- `CONCAT()`: Concatenates strings.
- `UPPER()` / `LOWER()`: Changes case.
- `SUBSTRING()`: Extracts part of a string.

```sql
SELECT CONCAT(title, ' by ', author) AS description FROM books;
```


### 13. Date Functions
- `NOW()`: Current date and time.
- `YEAR()`, `MONTH()`, `DAY()`: Extract date parts.

```sql
SELECT NOW();
SELECT YEAR(NOW());
```


## MySQL User Management

### 14. Creating a New User
```sql
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'password123';
```


### 15. Managing User Privileges
Granting privileges:
```sql
GRANT SELECT, INSERT, UPDATE ON library.* TO 'test_user'@'localhost';
```

Revoking privileges:
```sql
REVOKE INSERT ON library.* FROM 'test_user'@'localhost';
```

Applying changes:
```sql
FLUSH PRIVILEGES;
```


## Database Constraints

### 16. Primary Key
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100),
    PRIMARY KEY (id)
);
```


### 17. Foreign Key
```sql
CREATE TABLE orders (
    id INT AUTO_INCREMENT,
    user_id INT,
    product VARCHAR(100),
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```


### 18. NOT NULL and UNIQUE Constraints
```sql
CREATE TABLE products (
    id INT AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    product_code VARCHAR(50) UNIQUE,
    PRIMARY KEY (id)
);
```


## Advanced Queries

### 19. Joining Tables
```sql
SELECT users.name, orders.product
FROM users
JOIN orders ON users.id = orders.user_id;
```


### 20. Subqueries
```sql
SELECT name
FROM users
WHERE id IN (
    SELECT user_id
    FROM orders
    WHERE product = 'Computer'
);
```


### 21. JOIN vs UNION
**JOIN:** Combines rows from two tables based on a common condition.
```sql
SELECT users.name, orders.product
FROM users
JOIN orders ON users.id = orders.user_id;
```

**UNION:** Combines results of two queries, removing duplicates.
```sql
SELECT name FROM users
UNION
SELECT client_name FROM clients;
```


## Conclusion
Mastering SQL and MySQL is essential for database management. Practice creating databases, manipulating tables, and writing complex queries to fully understand these powerful tools.

