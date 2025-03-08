## Date and Time
```sql
now() -- Current Date and Time
curdate() -- Current Date
curtime() -- Current Time
date(created_at) -- Extracts date

-- Units: second, minute, hour, day, week, month, quarter, year
datediff(date1, date2)
date_add(date, interval 10 day)
date_sub(date, interval 10 day)
date_format(date, format)
extract(year from date)
```

## String
```sql
length(full_name)
concat(first_name, ' ', last_name)
concat_ws(' ', first_name, middle_name, last_name) -- concat with the first given string
lower(full_name) -- Lower case
upper(full_name)
trim(full_name)
replace(full_name, '_', ' ')
```

## Numeric
```sql
abs(-100) -- Absolute
ceil(10.25)
floor(10.25)
round(10.25)
truncate(10.251250, 2)
greatest(1, 2, 3, 4, 5)
least(1, 2, 3, 4, 5)
power(10, 2)
```

## JSON
- '$' references entire object
- '$.details' references a particular key 'details'
- '$[4]' references the fourth object in an array
- '$.details[4].name'

```sql
json_value(data, path)
json_query(data, path)
json_contains(data, to_search, path)
json_search(data, 'one'/'all', to_search)

-- Flatten json as rows
select * from
table_1, json_table(data, path columns (name varchar(50) path '$.name')) details
```

## SQL Injection
- Technique used to extract user data by injecting web page inputs as statements through SQL commands
- Solutions
  - Avoid string concatenations or string injections for queries
  - Use parameterized queries provided by the backend framework
  - Sanitize and escape queries
  -

```ruby
# Example 1
user_query = "select * from users where username = '#{username}'"
# If the user passes the following username from frontend or api paramenters, it will return all users
username = "application' or 1 = 1 --"
resultant_query = "select * from users where username = 'application' or 1 == 1 --'"
# Since the ' is commented using --, it will execute and return all users

# Example 2
products = "select name, description from products where category = '#{category}'"
category = "gifts' union select username, password from users --"
resultant_query = "select name, description from products where category = 'gifts'
  union select username, password from users --'"
```
