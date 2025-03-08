## Eager Loading
- Mechanism for loading associated records using as few queries as possible
- These mechanisms store the associated data in the form of rails objects
  - Joins do not store the associated data for future use
  - Joins may also give duplicate records
  - Joins do not provide associated data in rails objects

```rb
# N + 1 Queries Problem
# This executes 1 query to get books and one extra query per book to get author
# So 11 queries in total
Book.limit(10).map do |book|
  book.author.last_name
end
```

### Includes
- Generates two queries, one for the model and one for the relation
- If where is used, then it generates one query using left join
- If where is used with raw sql, then `references` should be used to force join

```rb
# SELECT books.* FROM books LIMIT 10
# SELECT authors.* FROM authors WHERE authors.id IN (1,2,3,4,5,6,7,8,9,10)
Book.includes(:author).limit(10).map do |book|
  book.author.last_name
end

Customer.includes(:orders, :reviews)
Customer.includes(orders: { books: [:supplier, :author] }).find(1)

# Conditions
# If there is a where condition, it generates a left join query
# SELECT authors.id AS t0_r0, ... books.updated_at AS t1_r5 FROM authors
# LEFT OUTER JOIN books ON books.author_id = authors.id
# WHERE (books.out_of_print = 1)
Author.includes(:books).where(books: { out_of_print: true })

# Conditions with SQL fragments
# If there is raw sql, we need to used `references` to force joined tables
Author.includes(:books).where("books.out_of_print = true").references(:books)
```

### Preload
- Loads each specified association using one query per association
- Not possible to specify conditions for preloaded associations

```rb
# SELECT books.* FROM books LIMIT 10
# SELECT authors.* FROM authors WHERE authors.id IN (1,2,3,4,5,6,7,8,9,10)
Book.preload(:author).limit(10).map do |book|
  book.author.last_name
end
```

### Eager Load
- Loads all specified associations using left join
- Can specify conditions like includes

```rb
# SELECT DISTINCT books.id FROM books
# LEFT OUTER JOIN authors ON authors.id = books.author_id
# LIMIT 10

# SELECT books.id AS t0_r0, books.last_name AS t0_r1, ... FROM books
# LEFT OUTER JOIN authors ON authors.id = books.author_id
# WHERE books.id IN (1,2,3,4,5,6,7,8,9,10)
Book.eager_load(:author).limit(10).map do |book|
  book.author.last_name
end
```

### Strict Loading
- Used to make sure no associations are lazy loaded

```rb
user = User.strict_loading.first

user = User.first
user.strict_loading!

user.address.city # raises an ActiveRecord::StrictLoadingViolationError
user.comments.to_a # raises an ActiveRecord::StrictLoadingViolationError
```
