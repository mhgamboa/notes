# MYSQL

I'm following [planetscale's](https://planetscale.com/learn/courses/mysql-for-developers/introduction/course-introduction) tutorial. Thank you guys so much!

## Schema's and Data Types

### Schemas

- Pick the smallest data type that will hold all of your data
- Pick the simplest column type that accurately reflects your data (e.g. use a numeric type for numbers, not a string)
- Ensure your schema accurately reflects the reality of your data (e.g. don't make a non-nullable column nullable)

#### Generated Columns

- If you want a column to generate data based on another column you'd use the following code:

```
CREATE TABLE emails (
  email VARCHAR(255)
  domain VARCHAR(155) AS (formula goes here) STORED --Forumla could be SUBSTRING_INDEX(email, "@", -1). Can be VIRTUAL or STORED. Virtual is default
)
```

#### Schema Migrations

- tl;dr a schema migration is a folder full of sql statements

### Data Types

#### Integers

- Use the smallest integer type possible
  - **TINYINT:** Can store values from 0 to 255 (or -128 to 127, if negative numbers are supported).
  - **SMALLINT:** Can store values from 0 to 65,535 (if negative numbers aren't supported).
  - **MEDIUMNINT:** Can store values from 0 to 16,777,215 (if negative numbers aren't supported).
  - **INT:** Can store values from 0 to 4,294,967,295 (if negative numbers aren't supported).
  - **BIGINT:** Can store values from 0 to 18,446,744,073,709,551,615 (if negative numbers aren't supported).

#### Decimals

- **DECIMAL:** an **exact** data type that stores exact values. (Think money, needs to be exact)
  - You can specify the exact amount of integers that should come before before and after the decimal as such: `Decimal(x,y)`
  - `x` is the total number of digits
  - `x` is the number of digits after the decimal
- **NUMERIC:** an alias for DECIMAL, the two are the same thing in MySQL.
- **FLOAT:** a floating-point data type that stores **approximate** values.
- **DOUBLE:** a floating-point data type that stores larger and more precise values than FLOAT.

#### Strings

- **CHAR** is for a fixed number of characters
  - `CHAR(5)` means you can have up to 5 characters, but will always occupy the max size bytes
- **VARCHAR** is for a varible number of characters
  - `VARCHAR(5)` means you can have up to 5 characters, and will always occupy the minimum bytes size possible
- Pretty much always use the utf8mb4 character set (Already the default for planetscale)

##### Long Strings

- **TEXT**- Huge amount of text as depicted by characters
  - **TINYTEXT** - 255 max characters (use varchar instead)
  - **TEXT** - 65,000 max characters
  - **MEDIUMTEXT** - 16 mb of characters
  - **LONGTEXT** - 4 gb of characters
- **BLOB** - Huge amount of text as depicted by binary
  - **TINYBLOB** - 255 max characters (use varchar instead)
  - **BLOB** - 65,000 max characters
  - **MEDIUMBLOB** - 16 mb of characters
  - **LONGBLOB** - 4 gb of characters (Can be used to store files.. not sure if you should though)
- An **ENUM** is a strings, but every possible string is pre-defined. It is both readable like a string, but compact in querying

#### Dates

- **DATE** - Use this if you only need to store the date, and not the time
- **DateTime** - Dates/Times range from 1000 to 9999. **JUST USE DATETIME OVER TIMESTAMP**
- **TimeStamp** - Dates/Times only range form 1970 to 2038. MYSQL Will convert to UTC upon Storage, and the local timezone upon retrival. **DON'T USE TIMESTAMP**

#### JSON

MySQL has support for JSON
