# Rows
### Insert
```sql
-- Insert all columns
insert into employees
values ('E1001', 'John Wick', ...);

-- Insert specific columns
insert into employees (id, name)
values ('E1001', 'John Wick');

-- Insert using a query
insert into employees (
  select employee_id, name
  from users
  where employee_id is not null
);
```

### Update
```sql
-- Update all rows
update employees
set age = datediff(date_of_birth, curdate());

-- Update specific rows
update employees
set salary = 10000, date_of_joining = '2023-01-01'
where name = 'John Wick';
```

### Delete
```sql
-- Drop table
drop table employees;

-- Delete all the data inside a table
truncate table employees;
delete from employees;

-- Delete data from a table
delete from employees
where joining_date < '2023-01-01';
```
