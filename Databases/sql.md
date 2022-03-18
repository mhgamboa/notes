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

A list of accepted SQL commands can be found [here](https://www.postgresql.org/docs/14/datatype.html)

All commands are made through the SQL Shell (psql).

If running the server locally you can just use all the defaults upon launching the Shell:

1. `server [localhost]:` ---> Press `enter`
2. `Database [postgres]:` ---> Press `enter`
3. `Port [5432]:` ---> Press `enter` (Assuming you set this up as default in the installation)
4. `username [postgres]:` ---> Press `enter`
5. `Password for user postgress:` ---> Enter the password you established on installation. Press `enter`

You can also use the GUI: pgAdmin 4

## psql Shell Commands

**All commands start with a backslash `\`**

1. View current version of psql -> `SELECT version();`
2. The "help" command -> `\?`

### Database Commands

1. List all databases -> `\l`
2. Create a database -> `create database [DB Name];` OR `CREATE DATABASE [DB Name];`
   - Tutorial prefers UPPERCASE to differentiate between SQL and non-SQL syntax
   - **The Semicolon `;` is required!!**
3. Connect to a Database -> `\c [DB Name];`
4. Delete a database -> `DROP DATABASE [DB Name];`
   - **Very Dangerous, be careful when implementing**

### Table Commands

1. List all tables -> `\d`
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

3. Connect to a Database -> ``
4. Delete a table -> `DROP TABLE [DB Name];`
