## Callbacks
- Active Record provides hooks into the object life cycle (create, update, destroy)
- These can be triggered before or after an alteration of an object's state
- Avoid calls to update, save, etc. in callbacks to avoid side-effects
- Triggered by
  - create, destroy, save, save(validate: false), update (& their bang counterparts)
  - destroy_all, destroy_by, touch, update_attribute
- Skipped by
  - insert, insert!, insert_all, insert_all!, upsert, upsert_all
  - update_column, update_columns, update_all
  - delete, delete_all, delete_by, touch_all

```rb
class Product < ApplicationRecord
  before_validation :assign_default_category, on: :create, if: :category_required
  before_create -> { self.name = name.capitalize }
  after_save do
    notify_sellers
  end

  private

    def assign_default_category
      # Avoid calls to update, save, etc. in callbacks to avoid side-effects
      self.category = 'default' if category.blank?
    end
end
```

## Execution
- After registering callbacks for a model, they are queued for execution
  - It includes validations, callbacks, database operations
- The whole callback chain is wrapped in a transaction
  - If any callback raises an exception, the execution chain get halted
  - And a ROLLBACK is issued
- To intentionally stop a chain, use `throw :abort`

## Available Callbacks
- Listed in the order in which they are called
- Create & Update
  - before_validation
  - after_validation
  - before_save
  - around_save
  - before_create / before_update
  - around_create / around_update
  - after_create / after_update
  - after_save
  - after_commit, after_rollback
- Destroy
  - before_destroy
  - around_destroy
  - after_destroy
  - after_commit, after_rollback
- Others
  - after_find (triggered by find methods)
  - after_initialize (triggered by 'new')
  - after_touch (triggered by 'touch')
- Association
  - before_add
  - before_remove
  - after_add
  - after_remove

## Callback Behavior
- after_save, after_create, after_update
  - Called before committing data to the database
  - If any error occurs in the callback, the transaction will be rolled back
  - And the data will not be persisted
- after_commit
  - Called after committing data to the database
  - If any error occurs in the callback, data will still be persisted
  - Should be used when model needs to interact with external system
    - That is not part of database transaction
    - Like notifications, emails, async third party calls
- before_destroy
  - Ensure that it executes before records are deleted
  - Place before `dependent: :destroy` associations or use `prepend: true`
- Order
  - Callbacks run in the order they are defined
  - But transactional (commit/rollback) callbacks could be reversed
    - Depends on `run_after_transaction_callbacks_in_order_defined` flag in config
    - Default value from rails 7.1 is true for this flag

## Association Callbacks
```rb
class Author < ApplicationRecord
  has_many :books, before_add: :check_credit_limit
  # Stacking callbacks
  has_many :books, before_add: [:check_credit_limit, :calculate_shipping_charges]

  def check_credit_limit(book)
    throw(:abort) if limit_reached?
  end
end

# Triggers `before_add` callback
author.books << book
author.books = [book, book2]

# Does not trigger the `before_add` callback
book.update(author_id: 1)
```

## Conditional Callbacks
```rb
before_save :normalize_card_number, if: :paid_with_card?
before_save :normalize_card_number, if: -> { paid_with_card? }
before_save :filter_content, if: [:filters_enabled?, :untrusted_author?]

after_commit :normalize_card_number, on: :create
after_commit :send_notification # Runs on create, update, destroy

# Aliases for after_commit
# Cannnot run both after_create_commit & after_update_commit on the same method
after_create_commit :method
after_update_commit :method
after_save_commit :method # create & update
after_destroy_commit :method
```

## Callback Classes
```rb
class FileDestroyerCallback
  def self.after_destroy(file)
    File.delete(file.filepath) if File.exist?(file.filepath)
  end
end

class PictureFile < ApplicationRecord
  after_destroy FileDestroyerCallback
end

# If an instantiated object is required to track state, use instance method
class FileDestroyerCallback
  def after_destroy(file)
    File.delete(file.filepath) if File.exist?(file.filepath)
  end
end

class PictureFile < ApplicationRecord
  after_destroy FileDestroyerCallback.new
end
```
