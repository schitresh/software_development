## Users
```sql
alter user 'root'@'localhost' identified with mysql_native_password by 'new_password';
```

## Database
```sql
show databases;
create database development;
use database development;
alter database development modify name = staging;
drop database staging;
```

## Schema
```sql
show tables;
show columns from development.employees;
show columns from employees;
```

## Table
### Create
```sql
create table employees(
  employee_id int not null unique,
  full_name varchar(50) not null,
  joining_date date default now(),
  manager_id int,
  primary key(employee_id),
  foreign key(manager_id) references employee(employee_id),
  check(joining_date <= now())
);

-- Create table from query
create table user_details as (
  select full_name from employees where joining_date >= '2023-01-01'
);

-- Create empty table by copying structure from another table
create table new_employees like employees;

-- Create new table with data copied from another table
select * into new_employees from employees;
```

### Alter
```sql
alter table new_employees rename to employees;

alter table employees add (date_of_birth date, salary decimal(10, 3));
alter table employees modify salary decimal(10, 2);
alter table employees drop column old_employee_id;
alter table employees rename column salary to salary_per_month;
```

### Insert
```sql
insert into employees('E1001', 'John Wick', ...);
insert into employees (id, name) values ('E1001', 'John Wick');
insert into employees (
  select employee_id, name from users where employee_id is not null
);
```

### Update
```sql
update employees set age = datediff(date_of_birth, curdate());

update employees
set salary = 10000, date_of_joining = '2023-01-01'
where name = 'John Wick';
```

### Delete
```sql
drop table employees;

-- Delete all the data inside a table
truncate table employees;
delete from employees;

-- Delete data from a tablee
delete from employees where joining_date < '2023-01-01';
```

## Constraints
```sql
-- Not Null
create table employees(date_of_joining date not null);
alter table employees modify date_of_joining date not null;

-- Unique
create table employees(employee_id int not null unique);
alter table employees add unique (employee_id);

-- Primary Key
create table employees(employee_id int not null primary key);
create table employees(employee_id int not null, primary key(employee_id, full_name));
alter table employees add primary key (employee_id);

-- Foreign Key
create table employees(address_id int references addresses(address_id));
create table employees(foreign key(address_id) references addresses(address_id));
alter table employees add foreign key (employee_id) references addresses(address_id);

-- Removing a column with a constraint like foreign key
select * from information_schema.table_constraints where table_name = 'employees';
alter table employees drop constraint manager_id;
alter table employees drop column manager_id;

-- Default
create table employees(city varchar(255) default 'New York');
alter table employees alter city drop default;

-- Check
-- Impose conditions on what type of data can be inserted
create table employees(check(date_of_joining) >= '2010-01-01');
alter table employees modify date_of_joining check(date_of_joining >= '2010-01-01');
alter table employees add constraint joined_after check(date_of_joining >= '2010-01-01');
alter table employees drop check joined_after;
```

## Indexes
- Should be used when the column
  - Contains wide range of values
  - Does not contain a large number of null values
  - Is used frequently in queries
- Should be avoided when
  - The table is small
  - The columns are not often used as conditions in queries
  - The columns are updated frequently
- Primary keys and foreign keys should be indexed

```sql
create index employees_city_index on employees (city, state_code);
create unique index employees_city_index on employees (city, state_code);
alter table employees drop index employees_city_index;
alter index employees_city_index on employees rebuild;
```

## Views
- Virtual tables created by selecting fields from one or more tables

```sql
create view salaries_for_employees_joined_after_2023 as
select employee_id, salary
from employees
where date_of_joining > '2023-01-01';

-- List all views
select * from information_schema.views where table_schema = "database_name";
-- Delete view
drop view view_name;
-- Update view
create or replace view view_name as (...);
```

## Temporary Table
```sql
-- Local temp table is available only for the current session
-- Prefixed by #
create table #employee_details (id int, name varchar(50));

-- Global temp table is visible to all connections and dropped when the last connection is closed
-- Prefixed by ##
create table ##employee_details (id int, name varchar(50));
```
