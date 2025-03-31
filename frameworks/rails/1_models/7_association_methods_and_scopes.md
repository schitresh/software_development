## Association Methods
```rb
# belongs_to and has_one
author
author = (objects)
build_author({...}) # Used to initialize a new author object
create_author({...})
create_author!({...})
reload_author
reset_author

# belongs_to
author_changed?
author_previously_changed?

# has_many, has_and_belongs_to_many
books
books << (object, ...)
books.delete(object, ...)
books.destroy(object, ...)
books = (objects)
book_ids
book_ids = (ids)
books.clear
books.empty?
books.size
books.find(...)
books.where(...)
books.exists?(...)
books.build({...}) # Used to initialize new book objects
books.create({...})
books.create!({...})
books.reload
```

## Dependent Options
- destroy
- delete
- destroy_async
- nullify
- restrict_with_exception
- restrict_with_error

```rb
has_many :books, dependent: :destroy
```

## Controlling Caching
- All the association methods are built around caching
- It keeps the result of most recent query available for further optimizations
- The cache is even shared across methods
- Call reload on the association to discard the cache

```rb
author.books.load # retrieves books from the database
author.books.size # uses the cached copy of books
author.books.empty? # uses the cached copy of books
author.books.reload.empty? # discards the cached copy of books
```

## Association Scope
```rb
# Models within different Modules
module Business
  class Supplier < ApplicationRecord
    has_one :account, class_name: 'Billing::Account'
  end
end

module Billing
  class Account < ApplicationRecord
    belongs_to :supplier, class_name: 'Business::Supplier'
  end
end

# Custom Foreign Key
class Book < ApplicationRecord
  belongs_to :writer, class_name: 'Author', foreign_key: 'author_id'
end

class Author < ApplicationRecord
  has_many :books, inverse_of: 'writer'
end

# Customize Query
class Book < ApplicationRecord
  belongs_to :author, -> { where(active: true) }
end

class Chapter < ApplicationRecord
  # Author belongs to Book
  # Not required if author was immediate association
  # Because they are eager-loaded automatically when needed
  belongs_to :book, -> { includes(:author) }
end
```
