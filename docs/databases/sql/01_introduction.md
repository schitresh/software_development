# Introduction
- SQL (Structured Query Language)
    - Standard database language used to access and manipulate data
- Uses
    - Data definition
    - Data retrieval
    - Data manipulation
    - Access control
    - Data sharing

## Operators
- Arithmetic Operators: `+`, `-`, `/`, `*`, `%`
- Comparison Operators: `=`, `<>` or `!=`, `<`, `>`, `<=`, `=>`
- Logical Operators: `and`, `or`, `not`
- Special Operators: `all`, `any`, `some`, `exists`, `between`, `in`, `distinct`, `unique`

## User
```sql
-- Change password
alter user 'root'@'localhost'
identified with mysql_native_password
by 'new_password';
```

## Database
```sql
-- List all databases
show databases;

-- Create database
create database db_name;

-- Use database
use database db_name;

-- Rename database
alter database db_name
modify name = new_db_name;

-- Drop database
drop database db_name;
```
