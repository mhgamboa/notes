# SQL

I'm following [this](https://youtu.be/qw--VYLpxG4) tutorial. Thank you FCC and Nelson from Amigoscode!

1. Read about [entity relationship diagrams](https://github.com/mhgamboa/notes/blob/main/Databases/entity-relationship-diagrams.md) to understand the following basics:
   - **Entities**
   - **Attributes**
   - **Records**
   - **Primary Keys**
   - **Foreign Keys**
   - **Composite Primary Keys**
   - **Entity Relationship Diagrams (ERD)**

A list of accepted SQL commands can be found [here](https://www.postgresql.org/docs/current/datatype.html)

- All commands are made through the SQL Shell (psql).
- **sql commands use single quotes**

If running the server locally you can just use all the defaults upon launching the Shell:

1. `server [localhost]:` ---> Press `enter`
2. `Database [postgres]:` ---> Press `enter`
3. `Port [5432]:` ---> Press `enter` (Assuming you set this up as default in the installation)
4. `username [postgres]:` ---> Press `enter`
5. `Password for user postgress:` ---> Enter the password you established on installation. Press `enter`

You can also use the GUI: pgAdmin 4

## General psql Shell Commands/Basics

**All commands start with a backslash `\`**

1. View current version of psql -> `SELECT version();`
2. The "help" command -> `\?`

## Database Commands

1. List all databases -> `\l`
2. Create a database -> `create database [DB Name];` OR `CREATE DATABASE [DB Name];`
   - Tutorial prefers UPPERCASE to differentiate between SQL and non-SQL syntax
   - **The Semicolon `;` is required to finish all commands!!**
3. Connect to a Database -> `\c [DB Name];`
4. Delete a database -> `DROP DATABASE [DB Name];`
   - **Very Dangerous, be careful when implementing**

## Table Commands

1. List all tables -> `\dt`
   - Get details of a tables and their relations (Like BIGSERIAL): `\d table_name`
   - Get details of a table: `\d table_name`
2. Create a table -> `CREATE TABLE table_name (Column_Name data_type constraints_if_any,);`. Example:

```
// BIGSERIAL === autoincrementing eight-byte integer
// VARCHAR(X) === variable-length character string

CREATE TABLE person (
   id BIGSERIAL NOT NULL PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
   gender VARCHAR(6) NOT NULL,
   date_of_birth DATE NOT NULL
);
```

4. Delete a table -> `DROP TABLE [DB Name];`
5. Select all columns in a Table -> `SELECT * FROM table_name;`
6. Select one column in a Table -> `SELECT column_name FROM table_name;`
7. Select two or more columns in a Table -> `SELECT first_name, last_name FROM table_name;`

## Record Commands

### Inserting Records

```
/* If we use BIGSERIAL as the data type for the PK, we
don't need to worry about the PK, as psql will handle it for us */

// Email is optional as we didn't define the schema as NOT NULL

INSERT INTO table_name ( first_name, last_name, gender, date_of_birth)

VALUES (John, Doe, Male, DATE '1990-31-12'); //Date must be YEAR-MONTH-DATE
```

### Querying Records

Aggregate query functions can be found [here](https://www.postgresql.org/docs/current/functions-aggregate.html).

1.  **Sorting Records** -> `SELECT * FROM person ORDER BY date_of_birth DESC;` (wILL Default to Ascending order if not specified)
    - Sort by DOB then last_name -> `SELECT * FROM person ORDER BY date_of_birth, last_name DESC;`
2.  **Get Distinct (Not duplicate) records** -> `SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;`
3.  **Filter Records** -> Use the `WHERE` clause:
    - `SELECT * FROM person WHERE gender = 'Female';`
4.  **Filter with multiple parameters** -> Use the `AND` or ``OR` clause:
    - `SELECT * FROM person WHERE gender = 'Male' AND country_of_birth = Poland;`
    - `SELECT * FROM person WHERE gender = 'Male' AND (country_of_birth = Poland OR country_of_birth = 'China');`
5.  **Limit the amount of Records returned** -> Use `LIMIT` or `FETCH` clase:
    - `SELECT * FROM person FETCH FIRST 10 ROW ONLY;` (The OG. Works with all SQL DBMS)
    - `SELECT * FROM person LIMIT 10;` (Not an official SQL clause, but works with psql)
6.  **Skip X amount of Records returned** -> Use `OFFSET` clause:
    - `SELECT * FROM person OFFSET 5 LIMIT 10;` (Skips first 5 records returned. In my testing order didn't matter between OFFSET and LIMIT)
7.  **Shorthand Method to Query** -> Use `IN` clause:
    - `SELECT * FROM person WHERE country_of_birth = 'China' OR country_of_birth = 'Brazil' OR country_of_birth = 'France';` (Longhand Method)
    - `SELECT * FROM person WHERE country_of_birth IN ('China', 'Brazil', 'France');` (Shorthand Method with `IN`)
8.  **Filter data between two ranges** -> Use `BETWEEN` clause (which is inclusive of upper and lower limits):
    - `SELECT * FROM person WHERE date_of_birth BETWEEN DATE '2021-01-01' AND '2021-31-12';`
9.  **Use regex like searches to filter Data** -> Use `LIKE` or `ILIKE` clause:
    - `SELECT * FROM person WHERE email LIKE '_____@gmail.%'`
      - `%` is 0+ wild card characters
      - `_` is exactly 1 wild card character
    - `SELECT * FROM person WHERE country_of_birth ILIKE 'p%'`
      - `ILIKE` makes query case insensitive
10. **Aggregate/Group data from multiple records** -> Use `GROUP BY` clause with special table functions:
    - `SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth ORDER BY country_of_birth;` (`COUNT()` is the special table function)
    - `HAVING` allows you to filter your aggregations
      - `SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) > 5 ORDER BY country_of_birth;`
        - (Shows all countries that appear more than 5 times in the Table, and show their count)

`SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) BETWEEN 6 AND 100 ORDER BY country_of_birth;`

## Comparson Operators

Basic Comparisons:

```
SELECT 1 = 1; //Returns "t" for true
SELECT 1 < 1; //Returns "f" for false
SELECT 1 > 1; //Returns "f" for false
SELECT 1 <= 1; //Returns "t" for true
SELECT 1 >= 1; //Returns "t" for true
SELECT 1 <> 1; //Returns "f" for false (1 is not equal to 1)

```

1. Comparison operators can be used on **any** data type. (numbers, strings, dates etc.)
