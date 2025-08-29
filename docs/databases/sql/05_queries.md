# Queries
### Select
```sql
-- Select all columns
select * from employees;

-- Select specific columns
select employee_id, full_name, date_of_joining from employees;

-- Alias column names
select
  full_name as name,
  datediff(date_of_birth, curdate()) as age
from employees;

-- Select distinct values
select distinct(city) from employees;
```

### Order and Limit
```sql
select * from employees order by date_of_joining;
select * from employees order by date_of_joining, salary desc;
select * from employees order by date_of_joining desc, salary desc;

select * from employees limit 10;

-- Select 10 random employees
select * from employees order by rand() limit 10;

-- Select employees having the 10th to 15th highest salary
select * from employees order by salary limit 5 offset 10;
select * from employees order by salary limit 5, 10;
```

### Where Conditions
```sql
-- Comparison operators
select * from employees where city is null;
select * from employees where city is not null;
select * from employees where city = 'New York';
select * from employees where city != 'New York';

-- Logical operators
select * from employees where city = 'New York' and salary >= 10000;
select * from employees where city = 'New York' or salary >= 10000;

-- String operators
select * from employees where name like '%wick';
-- Contains h at third position
select * from employees where name like '__h%';

-- Range operators
select * from employees where salary between 5000 and 10000;

-- Membership operators
select * from employees where city in ('New York', 'Los Angeles');
select * from employees where city not in ('New York', 'Los Angeles');
select * from employees where city in (
  select city from cities where state_code = 'NY'
);
```

### Conditionals
```sql
-- If
select
  full_name,
  if(salary < 10000, 'Employee', 'Executive') as category
from employees;

-- Case
select
  full_name,
  (case
    when salary < 10000 then 'Employee'
    when salary < 20000 then 'Executive'
    else 'Senior Executive'
  end) as category
from employees;
```

### Sets
- Applied between two 'select' queries where both the queries
    - Must have the same number of columns
    - Must have the same data type
    - Must have the columns of the same order

```sql
-- 'Union' returns distinct values
(select city from employees) union (select city from students);

-- 'Union all' allows duplicates
(select city from employees) union all (select city from students);

-- 'Intersect' returns common values
(select city from employees) intersect (select city from students);

-- 'Minus' returns distinct values
(select city from employees) minus (select city from students);
```

### Group
- Placed after the 'where' clause and before the 'order' & 'having' clause
- The 'having' clause is used for filtering the grouped data
- In case of multiple rows, the data from the first row is shown

```sql
select
    city,
    count(*) as num_of_employees
from employees
where date_of_joining >= '2023-01-01'
group by city
order by num_of_employees;

select
    city,
    year(date_of_joining) as year_of_joining,
    count(*) as num_of_employees
from employees
group by city, year(date_of_joining);

select
    city,
    count(*) as num_of_employees
from employees
group by city
having sum(salary) > 100000 and num_of_employees < 5;
```

### Aggregators
- Count, Sum, Avg, Max, Min

```sql
select sum(salary) from employees where department_id = 5;
select count(*) from employees group by department_id;
select * from employees group by department_id having count(*) > 10;
```

### Joins
- inner join (or join)
- left join (or left outer join)
- right join (or right outer join)
- full join (or full outer join)
  - MySQL does not explicitly support a full outer join
  - Instead, we can achieve it by combining a left join, a right join, a union operator

```sql
select * from employees
join addresses on employees.address_id = addresses.id;

-- Self Join
select
  emps.full_name as emp_name,
  manager.full_name as manager_name,
from employees as emps
join employees as managers on managers.id = emps.manager_id;

-- Full Outer Join
-- MySQL does not explicitly support a full outer join
select * from employees left join departments on employees.department_id = departments.id
union
select * from employees right join departments on employees.department_id = departments.id;

-- Update with join
update employees
join employees as managers on managers.id = emps.manager_id
set employees.manager_name = managers.full_name;

-- Delete with join
delete employees
from employees
join employees as managers on managers.id = emps.manager_id
where managers.full_name 'John Wick';
```

## Partition By
- 'group by' shows only the top rows
- But 'partition by' shows all the rows along with the partitioned values

```sql
-- For each employee, show the average salary of the employee's department
-- Does not group the employees, but selectively gets the average
select
  full_name,
  salary,
  avg(salary) over (partition by department) as department_average_salary
from employees;
```

## Over
- Apply function over by ordering rows with or without partition

```sql
select
  full_name,
  salary,
  rank() over (order by salary) as salary_rank
from employees;

select
  full_name,
  salary,
  rank() over (partition by department order by salary) as salary_rank_within_department
from employees;
```
