# SQL

I'm following [this](https://youtu.be/qw--VYLpxG4) tutorial. Thank you FCC and Nelson from Amigoscode!!

1. Read about [entity relationship diagrams](https://github.com/mhgamboa/notes/blob/main/Databases/entity-relationship-diagrams.md) to understand the following basics:
   - **Entities**
   - **Attributes**
   - **Records**
   - **Primary Keys**
   - **Foreign Keys**
   - **Composite Primary Keys**
   - **Entity Relationship Diagrams (ERD)**

All commands are made through the SQL Shell (psql).

If running the server locally you can just use all the defaults upon launching the Shell:

1. `server [localhost]:` ---> Press `enter`
2. `Database [postgres]:` ---> Press `enter`
3. `Port [5432]:` ---> Press `enter` (Assuming you set this up as default in the installation)
4. `username [postgres]:` ---> Press `enter`
5. `Password for user postgress:` ---> Enter the password you established on installation. Press `enter`

You can also use the GUI: pgAdmin 4

## psql commands

All commands start with a backslash `\`

1. The "help" command -> `\?`
2. List all databases -> `\l`
3. Create a dabase -> `create database [database Name]` OR `CREATE DATABASE [database Name]`
