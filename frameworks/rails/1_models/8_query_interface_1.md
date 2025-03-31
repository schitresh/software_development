## Retrieve Object
```rb
Customer.find(id) # Raises error if not found
Customer.find(ids)
Customer.first # last
Customer.find_by(attributes)
Customer.find_by!(attributes) # Raises error if not found
Customer.where(attributes).first
```

## Retrieve Multiple Objects
- When it's required to iterate over a large set of records
  - Using `Customer.where(...).each` may seem straightforward
  - But it becomes impractical as the table size increases
- This is because it instructs Active Record to fetch the entire data in single pass
  - Build a model object per row
  - Keep the entire array of objects in memory
  - May exceed the amount of memory available
- Rails provides two methods to address this: find_each, find_in_batches
  - This divides records into memory friendly batches for processing

```rb
# Find Each
# Retrieves records in batches and then yields each one to the block
Customer.where(weekly_subscriber: true).find_each do |customer|
  NewsMailer.weekly(customer).deliver_now
end

find_each(batch_size: 5000)
find_each(start: 2000) # First ID
find_each(start: 2000, finish: 10000) # First & last ID
find_each(order: :desc)

# Find in Batches
# Similar to find_each, but it yields batches to the block as an array of model
# Give add_customers an array of 1000 recently active customers at a time
Customer.recently_active.find_in_batches do |customers|
  export.add_customers(customers)
end

find_in_batches(batch_size: 5000)
find_in_batches(start: 2000) # First ID
find_in_batches(start: 2000, finish: 10000) # First & last ID
```

## Conditions
- Building conditions as pure strings can be vulnerable to SQL injection
  - For example, `Book.where('title LIKE '%#{params[:title]}%'')` is not safe
  - The preferred way to handle conditions is using rails conditions

```rb
# Array Conditions
Book.where('title = ? AND out_of_print = ?', params[:title], false)
# Placeholder Conditions
Book.where(
  'created_at >= :start_date AND created_at <= :end_date',
  start_date: params[:start_date], end_date: params[:end_date]
)
# Sanitizing Conditions (Required for wild cards like '%')
Book.where('title LIKE ?', Book.sanitize_sql_like(params[:title]) + '%')

## Hash Conditions
Book.where(out_of_print: true)
Author.joins(:books).where(books: { author: author })
Book.where([:author_id, :id] => [[15, 1], [15, 2]])
Book.where(created_at: (Time.now.midnight - 1.day)..Time.now.midnight)
Customer.where(orders_count: [1, 3, 5])

# Multiple Conditions
Customer.where(last_name: 'Smith', orders_count: [1, 3, 5])
Customer.where(last_name: 'Smith').where(orders_count: [1, 3, 5])

# Logical Conditions
Customer.where.not(orders_count: [1, 3, 5]) # nil values won't be returned
Customer.where(last_name: 'Smith').or(Customer.where(orders_count: [1, 3, 5]))
Customer.where(id: [1, 2]).and(Customer.where(id: [2, 3]))
```

## Query Operations
```rb
# Order
Book.order(title: :asc, created_at: :desc)
Book.order(:title, created_at: :desc) # asc by default
Book.order('title ASC, created_at DESC')
Book.order('title ASC', 'created_at DESC')

# Select
Book.select(:isbn, :out_of_print)
Book.select('isbn, out_of_print')
Book.select(:title).distinct

# Limit
Customer.limit(5)
Customer.limit(5).offset(30)

# Group
Order.group(:status).count
Order.select('created_at').group('created_at')

# Having
Order.group('created_at').having('sum(total) > ?', 200)
Order.select('created_at, sum(total) as total_price')
     .group('created_at')
     .having('sum(total) > ?', 200)

# Null Relation
# Returns a chainable relation with no records
# Useful where a Active Record relation is required for further operations
Book.none

# Read-only Objects
Customer.readonly.first
```

## Find or Build
```rb
customer = Customer.find_or_initialize_by(first_name: 'Nina')
customer.persisted? # false
customer.new_record? # true
customer.save # true

customer = Customer.find_or_create_by(first_name: 'Andy')
customer = Customer.find_or_create_by!(first_name: 'Andy')

# If an attribute (say 'locked') should not be included in the query
# But should be assigned if a new record is created
Customer.create_with(locked: false).find_or_create_by(first_name: 'Andy')
Customer.find_or_create_by(first_name: 'Andy') { |c| c.locked = false }
```

## Calculations
```rb
Customer.count
Customer.where(first_name: 'Ryan').count
Customer.includes(:orders).where(first_name: 'Ryan', orders: { status: 'shipped' }).count

Order.average("subtotal")
Order.minimum("subtotal")
Order.maximum("subtotal")
Order.sum("subtotal")
```

## Existence of Objects
```rb
Customer.exists?(1) # SELECT 1 FROM customers WHERE id = 1 LIMIT 1
Customer.exists?(id: [1, 2, 3])
Customer.exists?(first_name: ['Jane', 'Sergei'])
Customer.where(first_name: 'Ryan').exists?

Order.any? # SELECT 1 FROM orders LIMIT 1
Order.shipped.any?
Book.where(out_of_print: true).any?
Customer.first.orders.any?

Order.many? # SELECT COUNT(*) FROM (SELECT 1 FROM orders LIMIT 2)
Order.shipped.many?
Book.where(out_of_print: true).many?
Customer.first.orders.many?
```

## Explain
- Returns query plan for a given query

```rb
Customer.where(id: 1).joins(:orders).explain
Customer.where(id: 1).includes(:orders).explain
Customer.where(id: 1).joins(:orders).explain(:analyze, :verbose)
```
