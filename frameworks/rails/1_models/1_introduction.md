## Active Record
- Layer representing business data & logic (M in MVC)
- Facilitates creation & use of business objects whose data requires persistent storage
- Implementation of Active Record pattern (description of ORM system)
- Object Relationship Mapping (ORM) connects rich objects of application to RDBMS tables

## Schema Conventions
- Database tables should be in camelcase and pluralized
  - Class 'BookClub' should have table 'book_clubs'
- Keys
  - Foreign key should be singularized: order_id
  - Primary key will be 'id' column with type 'bigint' for postgres & mysql
- Optional columns managed by rails (are reserved keywords)
  - created_at
  - updated_at
  - lock_version: Adds optimistic locking to a model
  - type: For single table inheritance
  - (association)_type: For polymorphism
  - (association)_count: To cache the number of objects in association

## Models
- When generating an app, an abstract `ApplicationRecord` is created
  - Present in 'app/models/application_record.rb'
  - Base class for all model files to convert ruby class into active record model

```rb
class User < ApplicationRecord
end
```

## Crud
```rb
# Create
user = User.create(name: "David", occupation: "Code Artist")

user = User.new
user.name = "David"
user.occupation = "Code Artist"
user.save

user = User.new do |u|
  u.name = "David"
  u.occupation = "Code Artist"
end

# Read
users = User.all
users = User.first
david = User.find_by(name: 'David')
users = User.where(name: 'David', occupation: 'Code Artist').order(created_at: :desc)

# Update
user = User.find_by(name: 'David')
user.name = 'Dave'
user.save

user = User.find_by(name: 'David')
user.update(name: 'Dave')
User.update_all(max_login_attempts: 3, must_change_password: true)

# Destroy
user = User.find_by(name: 'David')
user.destroy

User.destroy_by(name: 'David')
User.destroy_all
```
