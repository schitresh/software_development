# Examples
### Project wise count of employees sorted in decreasing order
```sql
select project, count(emp_id) emp_count
from details
group by project
order by emp_count desc;
```

### Duplicates in details
```sql
select emp_id from details
group by emp_id, project, salary
having count(*) > 1;
```

### Remove duplicates in details
```sql
delete from details
where emp_id in (
  select emp_id from details group by emp_id, project, salary having count(*) > 1
);
```

### Fetch odd rows
```sql
select emp_id, project, salary
from (
  select *, row_number() over (order by emp_id) as details_row_number
  from details
)
where details_row_number % 2 = 1;
```

### Employees joined in 2016
```sql
select * from employees where year(joining_date) = "2016";
```

### Nth highest salary
```sql
select salary from employees order by salary desc limit n-1, 1;
-- or
select salary
from (select salary from employees order by salary desc limit n)
order by salary asc
limit 1;
```

### Remove underscore from names
```sql
update employees set full_name = replace(full_name, '_', ' ')
```

### Get employee salaries along with their department's average salary, ordered by salary within department
```sql
select
  full_name, salary,
  avg(salary) over (partition by department order by salary) as department_average_salary
from employees
join details on details.emp_id = employees.emp_id;
```
