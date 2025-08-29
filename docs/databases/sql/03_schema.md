# Schema

### List Tables & Columns
```sql
-- List all tables
show tables;

-- List all columns
show columns from db_name.table_name;
show columns from table_name;
```

### Create Table
```sql
create table table_name (
    column_name data_type constraint,
    column_name data_type constraint,
    ...
);
```

```sql
-- Create a table with constraints
create table employees (
    employee_id int not null unique,
    full_name varchar(50) not null,
    joining_date date default now(),
    manager_id int,
    primary key(employee_id),
    foreign key(manager_id) references employee(employee_id),
    check(joining_date <= now())
);

-- Create a table using a query
create table user_details as (
    select full_name
    from employees
    where joining_date >= '2023-01-01'
);

-- Create an empty table using the schema of another table
create table new_employees like employees;

-- Create a table by copying data from another table
select * into new_employees from employees;
```

### Alter Table
```sql
-- Rename table
alter table employees rename to new_employees;

-- Add column
alter table employees add (date_of_birth date, salary decimal(10, 3));

-- Modify column
alter table employees modify salary decimal(10, 2);

-- Drop column
alter table employees drop column old_employee_id;

-- Rename column
alter table employees rename column salary to salary_per_month;
```

### Constraints
```sql
-- List all constraints of a table
select *
from information_schema.table_constraints
where table_name = 'employees';

-- Removing a column with a constraint (e.g. foreign key)
alter table employees drop constraint manager_id;
alter table employees drop column manager_id;
```

#### Not Null
```sql
create table employees (
    date_of_joining date not null
);

alter table employees
modify date_of_joining date not null;
```

#### Unique
```sql
create table employees (
    employee_id int not null unique
);

alter table employees add unique (employee_id);
```

#### Primary Key
```sql
create table employees (
    employee_id int not null primary key
);

create table employees (
    employee_id int not null,
    primary key(employee_id, full_name)
);

alter table employees add primary key (employee_id);
```

#### Foreign Key
```sql
create table employees (
    address_id int references addresses(address_id)
);

create table employees (
    foreign key(address_id) references addresses(address_id)
);

alter table employees
add foreign key (employee_id) references addresses(address_id);
```
#### Default
```sql
create table employees (
    city varchar(255) default 'New York'
);

alter table employees
alter city drop default;
```

#### Check
- Imposes conditions on what type of data can be inserted

```sql
create table employees (
    check(date_of_joining) >= '2010-01-01'
);

alter table employees
modify date_of_joining check(date_of_joining >= '2010-01-01');

alter table employees
add constraint joined_after check(date_of_joining >= '2010-01-01');

alter table employees
drop check joined_after;
```

### Indexes
- Should be used when the column
    - Contains a wide range of values
    - Does not contain a large number of null values
    - Is used frequently in queries
- Should be avoided when
    - The table is small
    - The columns are not often used as conditions in queries
    - The columns are updated frequently
- Primary keys and foreign keys should be indexed

```sql
-- Create index
create index employees_city_index
on employees (city, state_code);

-- Create unique index
create unique index employees_city_index
on employees (city, state_code);

-- Drop index
alter table employees
drop index employees_city_index;

-- Rebuild index
alter index employees_city_index
on employees rebuild;
```

### Views
- Virtual tables created by selecting fields from one or more tables

```sql
create view salaries_for_employees_joined_after_2023 as (
    select employee_id, salary
    from employees
    where date_of_joining > '2023-01-01'
);

-- List all views
select *
from information_schema.views
where table_schema = "database_name";

-- Delete view
drop view view_name;

-- Update view
create or replace view view_name as (query);
```

### Temporary Table
```sql
-- Local temporary table
-- Available only for the current session
-- Prefixed by #
create table #employee_details (id int, name varchar(50));

-- Global temporary table
-- Visible to all connections and dropped when the last connection is closed
-- Prefixed by ##
create table ##employee_details (id int, name varchar(50));
```
