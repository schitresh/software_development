## Migration
- Used to alter database schema over time in a consistent way
  - It can be thought of as a new version of the app's database
  - Each migration is wrapped in a transaction
- File is appended with timestamp to track which migration to run & their order
  - For example: 20240101120000_create_products.rb
  - Stored in 'db/migrate' directory
- Running migrations generates or updates `db/schema/rb`
  - Captures the current state of the database schema
- Active Record way claims that intelligence belongs in models & not database
  - Triggers, constraints, validations in database is not recommended
  - Database contraints make testing & maintenance more difficult
  - Though some constraints that don't depend on models can be kept in database
    - For faster processing and reduce overhead in rails

## Commands
- `rails db:create`
- `rails db:schema:load`
- `rails db:seed`
- `rails db:setup` runs create, schema:load & seed
- `rails db:prepare` is similar to setup but idempotent
- `rails db:drop`
- `rails db:reset` runs dropm setup
- `rails db:migrate` runs pending migrations
- `rails db:migrate:up`, `rails db:migrate:down` for specific method
- `rails db:migrate VERSION=20240101120000` for specific version
- `rails db:migrate RAILS_ENV=test` to run in specific environment
- `rails db:rollback` rollbacks last migration
- `rails db:rollback STEP=2` rollbacks last 2 migrations

## Create Table
```rb
# Create Table
# rails generate migration CreateProducts
# rails generate migration CreateProducts name:string description:text
# rails generate model Product name:string description:text
class CreateProducts < ActiveRecord::Migration[7.1]
  def change
    create_table :products do |t|
      t.string :name
      t.text :description

      t.timestamps # Adds created_at, updated_at (managed by rails)
    end
  end
end

# Create Join Table
create_join_table :products, :categories, table_name: :categorization

create_join_table :products, :categories do |t|
  t.index :product_id
  t.index :category_id
end
```

## Change Table
```rb
change_table :products do |t|
  t.remove :description, :name
  t.string :part_number
  t.index :part_number
  t.rename :upccode, :upc_code
end
```

## Add Columns
```rb
# rails generate migration AddCategoryToProducts price:decimal:index
class AddPriceToProducts < ActiveRecord::Migration[7.1]
  def change
    add_column :products, :price, :decimal
    add_index :products, :price
  end
end

# Details
# rails generate migration AddDetailsToProducts 'price:decimal{5,2}'
add_column :products, :price, :decimal, precision: 5, scale: 2

# Null & Default
# Cannot be specified in command line
add_column :products, :available, :boolean, null: false, default: true
add_column :products, :available, :boolean, null: true

# Association
# rails generate migration AddUserRefToProducts user:references
add_reference :products, :user, foreign_key: true
# rails generate migration AddDetailsToProducts supplier:references{polymorphic}
add_reference :products, :supplier, polymorphic: true
```

## Remove Column
```rb
# rails generate migration RemoveCategoryFromProducts category:string
class RemoveCategoryFromProducts < ActiveRecord::Migration[7.1]
  def change
    remove_column :products, :category, :string
  end
end
```

## Change Columns
```rb
change_column :products, :part_number, :text
```

## Reversal
```rb
# Generally change method is used to rollback a migration
# But if active record doesn't know how to reverse a migration
# Up & down methods can be used to specify it
class ChangeProductsPrice < ActiveRecord::Migration[7.1]
  def up
    add_column :products, :price, :string
    # If required, run some queries to update data
  end

  def down
    # If required, run some queries to update data
    remove_column :products, :price, :string
  end
end
```
