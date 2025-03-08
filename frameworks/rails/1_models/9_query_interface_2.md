## Finding by SQL
```rb
# Returns an array of object
Customer.find_by_sql(
  "SELECT * FROM customers
  INNER JOIN orders ON customers.id = orders.customer_id
  ORDER BY customers.created_at desc"
)

# Returns ActiveRecord::Result
# Calling 'to_a' on the result will return an array of hashes
# [{"first_name"=>"Rafael", "created_at"=>"2012-11-10 23:23:45.281189"}]
Customer.connection.select_all(
  "SELECT first_name, created_at FROM customers WHERE id = '1'"
).to_a

# Select
# Returns ActiveRecord objects, need to map to get original values
Customer.select(:id).map(&:id)
Customer.select(:id, :first_name).map { |c| [c.id, c.first_name] }
Customer.select(:first_name, :last_name).map(&:name) # name is a model method

# Pluck
# Selects and returns array of values (cannot be chained further)
Customer.pluck(:id, :first_name)
Book.where(out_of_print: true).pluck(:id)
Order.distinct.pluck(:status)
Order.joins(:customer, :books).pluck('orders.created_at, customers.email, books.title')

Customer.pick(:id) # Short form for Customer.pluck(:id).first
Customer.ids
```

## Joining Tables
- There are two joins available: joins, left_joins

```rb
# Raw SQL Joins
Author.joins("INNER JOIN books ON books.author_id = authors.id
  AND books.out_of_print = FALSE")

# Single Joins
Book.joins(:reviews)
Book.joins(:author, :reviews)

# Nested Joins
Book.joins(reviews: :customer)
Author.joins(books: [{ reviews: { customer: :orders } }, :supplier])

# Conditions
Customer.joins(:orders).where(orders: { created_at: time_range }).distinct
Customer.joins(:orders).find_by(orders: { created_at: time_range }) # Gets first record

# Left Join
Customer.left_joins(:reviews).distinct
        .select('customers.*, COUNT(reviews.*) AS reviews_count')
        .group('customers.id')

# Association Presence
# Customers at have at least one review
Customer.where.associated(:reviews)
# Customers at have no review
Customer.where.missing(:reviews)
```

## Locking Records
- Used to prevent race conditions when updating records in the database
- Ensures atomic updates

### Optmistic Locking
- Allows multiple users to access the same record for edits
  - Checks if another process has made changes since the record was open
  - If so, the update is ignored by raising stale object error
- To use this locking, table needs to have `lock_version` column of type integer
  - It's incremented each time the record is updated
  - If an update request is made with a lower value than current lock_version
    - The update request fails with stale object error

```rb
c1 = Customer.find(1)
c2 = Customer.find(1)

c1.first_name = "Sandra"
c1.save

c2.first_name = "Michael"
c2.save # Raises an ActiveRecord::StaleObjectError
```

### Pessimistic Locking
- Uses a locking mechanism provided by underlying database
- An exclusive lock is obtained on selected rows using `lock`
- Relations using `lock` are usually wrapped in a transaction to prevent deadlock

```rb
Book.transaction do
  book = Book.lock.first
  book.title = 'Algorithms'
  book.save!
end
# Equivalent SQL
# BEGIN
#   SELECT * FROM books LIMIT 1 FOR UPDATE
#   UPDATE books SET updated_at = '2009-02-07 18:05:56', title = 'Algorithms'
#   WHERE id = 1
# COMMIT

# If there is already an instance
book = Book.first
book.with_lock do
  # This block is called within a transaction
  book.increment!(:views)
end

# Can also pass raw SQL to the lock method for different types of locks
Book.transaction do
  book = Book.lock("LOCK IN SHARE MODE").find(1)
  book.increment!(:views)
end
```

## Scopes
- Used to specify commonly used queries in method calls
- All scope bodies should return ActiveRecord::Relation or nil to allow furthur chaining
  - That's why scopes are preferred over class methods

```rb
class Book < ApplicationRecord
  scope :out_of_print, -> { where(out_of_print: true) }
  scope :out_of_print_and_expensive, -> { out_of_print.where("price > 500") }
  scope :costs_more_than, ->(amount) { where("price > ?", amount) }
  scope :created_before, ->(time) { where(created_at: ...time) if time.present? }
end

Book.out_of_print
Book.out_of_print.costs_more_than(500)
Book.in_print.merge(Book.out_of_print)
Book.in_print.joins(:author).merge(Author.active)


class Book < ApplicationRecord
  default_scope { where(out_of_print: false) }
end

Book.all # Applies default scope
Book.unscoped.all # To skip default scope
Book.new # Applies out_of_print = false by default
Book.unscoped.new # To skip the default scope value
```

## Enums
- Defines an array of values allowed for an attribute
- Creates scopes and instance methods to get, set & query values

```rb
class Order < ApplicationRecord
  enum :status, [:shipped, :being_packaged, :complete, :cancelled]
end

Order.shipped
Order.not_shipped
order.shipped?
order.complete?
order.shipped!
```
