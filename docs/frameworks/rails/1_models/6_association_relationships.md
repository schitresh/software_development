## One to One
```rb
# managers table should have project_id
class Manager < ApplicationRecord
  belongs_to :project
end

class Project < ApplicationRecord
  has_one :manager
end

# Migration
add_reference :managers, :project, foreign_key: true
```

## One to Many
```rb
# books table should have author_id
class Book < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
  has_many :books
end

# Migration
add_reference :books, :author, foreign_key: true
```

## Many to Many
### Has Many Through
- Should be used if required to add logic to the joining model
- Recommended to create join table using composite primary key
  - Via `primary_key: [:physician_id, :patient_id]`

```rb
class Physician < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments
end

class Appointment < ApplicationRecord
  belongs_to :physician
  belongs_to :patient
end

class Patient < ApplicationRecord
  has_many :appointments
  has_many :physicians, through: :appointments
end

# Migration
create_table :appointments, primary_key: [:physician_id, :patient_id] do |t|
  t.belongs_to :physician
  t.belongs_to :patient
  t.datetime :appointment_date
  t.timestamps
end
```

### Has and Belongs to Many
- Should be used if the joining model is not required
- Recommended to create join table with no primary key via `id: false`
  - Use `create_join_table` to handle this implicitly

```rb
class Physician < ApplicationRecord
  has_and_belongs_to_many :patients
end

class Patient < ApplicationRecord
  has_and_belongs_to_many :physicians
end

# Migration
create_table :physicians_patients, id: false do |t|
  t.belongs_to :physician
  t.belongs_to :patient
  t.index [:physician_id, :patient_id]
  t.index [:patient_id, :physician_id]
end
# Or
create_join_table :physicians_patients, :parts do |t|
  t.index :physician_id
  t.index :patient_id
  t.index [:physician_id, :patient_id]
  t.index [:patient_id, :physician_id]
end
```

## Polymorphism
- A model can belong to multiple other models on a single association

```rb
# images table should have object_id & object_type
# Migration: `add_reference :images, :object, polymorphic: true`
# Add index on [:object_type, :object_id]
# For employee, object_type will be 'Employee' & object_id will be employee.id
class Image < ApplicationRecord
  belongs_to :object, polymorphic: true
end

class Employee < ApplicationRecord
  has_many :pictures, as: :object
end

class Product < ApplicationRecord
  has_many :pictures, as: :object
end

# Migration
create_table :images do |t|
  t.references :object, polymorphic: true
  t.timestamps
end
```

## Self Joins
```rb
class Employee < ApplicationRecord
  has_many :subordinates, class_name: 'Employee', foreign_key: 'manager_id'
  belongs_to :manager, class_name: 'Employee', optional: true
end

# Migration
add_reference :employees, :manager, foreign_key: { to_table: :employees }
```

## Composite Primary Key
```rb
class Author < ApplicationRecord
  self.primary_key = [:first_name, :last_name]
  has_many :books, query_constraints: [:first_name, :last_name]
end

class Book < ApplicationRecord
  belongs_to :author, query_constraints: [:author_first_name, :author_last_name]
end
```

## Single Table Inheritance (STI)
- To share fields and behavior between different models
- For example, we want to share color and price fields in Car, Bike, Bus
  - Create vehicles table with 'type' column
  - type column will store the name of the entity like Car, Bike, or Bus

```rb
class Vehicle < ApplicationRecord
end

class Car < Vehicle
end

# There will be only one table 'vehicles', Car objects will be stored in the same
# Will be saved to vehicles tables with type = 'Car' and the mentioned details
Car.create(color: 'Red', price: 10000)
```

### Delegate Type
- STI can bloat the vehicles table
- Columns specific to Car will need to be included in the same table
- This can be solved by using delegated type

```rb
module VehicleEntity
  extend ActiveSupport::Concern

  included do
    has_one :vehicle, as: :vehicle_entity, touch: true
  end
end

class Vehicle < ApplicationRecord
  delegated_type :vehicle_entity, types: %w[Car Bike], dependent: :destroy
end

class Car < ApplicationRecord
  include VehicleEntity
end
```
