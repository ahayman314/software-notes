In [[SQL]] DDL, we are talking about table management, not querying:

| Operation        | Details                    |
| ---------------| -------------------------|
| CREATE           | Create & define table      |
| INSERT           | Insert a tuple             |
| DELETE           | Delete tuples              |
| DROP             | Removes entire table       |
| ALTER TABLE ADD  | Add new attribute & domain |
| ALTER TABLE DROP | Remove an attribute        |
### Creating Tables
This is the most important operation because it defines our schema.
```sql
create table instructor (
ID char(5),
name varchar(20) not null,
dept_name varchar(20),
salary numeric(8,2),
primary key (ID),
foreign key (dept_name) references department);
```
### Inserting Data (rows) in Tables
### Updating Data (column values) in Tables
This is similar to [[SQL DML Querying (Data Manipulation)]] where instead of `SELECT`, we use `UPDATE`
```sql
UPDATE instructor 
SET salary = salary * 1.05
WHERE salary IS NOT NULL
```
### Deleting Data in Tables