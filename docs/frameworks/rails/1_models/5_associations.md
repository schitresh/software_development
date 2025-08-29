## Associations
- Connections between Active Record models to make common operations simpler
- Makes it easier to perform operations and query
- Types
  - belongs_to
  - has_one
  - has_one through
  - has_many
  - has_many through
  - has_and_belongs_to_many

## Belongs To
- Each instance belongs to one instance of the associated model
- Must use singular term

```rb
class Book < ApplicationRecord
  # Requires to add author_id column in books table
  belongs_to :author
  belongs_to :author, optional: true
end
```

## Has One
- The associated model has a reference to this model
- Must use singular term

```rb
class Supplier < ApplicationRecord
  # Required to add supplier_id in accounts table
  has_one :account
end
```

## Has One Through
```rb
class Supplier < ApplicationRecord
  has_one :account
  has_one :account_history, through: :account
end

class Account < ApplicationRecord
  belongs_to :supplier
  has_one :account_history
end

class AccountHistory < ApplicationRecord
  belongs_to :account
end
```

## Has Many
- Each instance has zero or more instances of associated model
- Must use plural term

```rb
class Author < ApplicationRecord
  has_many :books
end
```

## Has Many Through
```rb
class Document < ApplicationRecord
  has_many :sections
  has_many :paragraphs, through: :sections
end

class Section < ApplicationRecord
  belongs_to :document
  has_many :paragraphs
end

class Paragraph < ApplicationRecord
  belongs_to :section
end
```

## Has and Belongs to Many
- Each instance has zero or more instances of associated model
- And each associated instance has zero or more instances of current model

```rb
class Assembly < ApplicationRecord
  has_and_belongs_to_many :parts
end

class Part < ApplicationRecord
  has_and_belongs_to_many :assemblies
end
```
