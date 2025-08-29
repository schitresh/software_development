## Validations
- Ensures that only valid data is saved into the database
  - Database agnostic and cannot be bypassed by end users
  - Convenient to test and maintain
- Triggered by
  - create, create!, save, save!, update, update!
  - Object is saved only if there is no error
  - Bang methods also raise an exception
- Skipped by
  - insert, insert!, insert_all, insert_all!, upsert, upsert_all
  - update_all, update_attribute, update_column, update_columns
- Validations can be triggered manually using `valid?` or `invalid?`
- Validations can also be skipped using `save(validate: false)`
- If any validation fails, the errors are stored in `errors` attribute

```rb
class Product < ApplicationRecord
  validates :name, presence: true, length: { maximum: 100 }
end
```

## Helpers
```rb
# Acceptance
# Should be accepted like terms or service, checks for truthy values
# Triggers if not nil
validates :terms, acceptance: true
validates :terms, acceptance: { message: 'must be abided' }
validates :terms, acceptance: { accept: 'yes' }
validates :terms, acceptance: { accept: ['yes', 'accepted'] }

# Confirmation
# Where there is a confirmation field, like email or password
# Triggers if not nil (requires separate presence check)
validates :email, confirmation: true
validates :email, confirmation: { case_sensitive }
validates :email_confirmation, presence: true, if: :email_changed?

# Comparison
# equal_to, greater_than, greater_than_or_equal_to
# less_than, less_than_or_equal_to, other_than
validates :end_date, comparison: { greater_than: :start_date }

# Format
# with or without
validates :legacy_code, format: { with: /\A[a-zA-Z]+\z/, message: 'only letters' }

# Inclusion & Exclusion
validates :size, inclusion: { in: %w(small medium large) }
validates :size, exclusion: { in: %w(small medium large) }

# Length
validates :name, length: { minimum: 2 }
validates :bio, length: { maximum: 500 }
validates :password, length: { in: 6..20 }
validates :registration_number, length: { is: 6 }

# Numericality
# equal_to, greater_than, greater_than_or_equal_to
# less_than, less_than_or_equal_to, other_than, in, even, odd
validates :points, numericality: true
validates :games_played, numericality: { only_integer: true }

# Presence & Absence
validates :name, :login, :email, presence: true
validates :name, :login, :email, absence: true

# Uniqueness
validates :email, uniqueness: true
validates :name, uniqueness: { case_sensitive: false}
validates :department, uniqueness: { scope: :company }

# Associated
# Validates association every time the object is saved
# Should not be used in both models, will create infinite loop
has_many :books
validates_associated :books
```

## Common Attributes
```rb
# allow_nil, allow_blank
validates :size, inclusion: { in: %w(small medium large), allow_nil: true
validates :title, length: { is: 5 }, allow_blank: true

# message
validates :name, presence: { message: "must be given please" }
# Dynamically available attributes: value, attribute, model
validates :age, numericality: { message: "%{value} seems wrong" }

# on
validates :email, uniqueness: true, on: :create # on: :update
validates :name, presence: true # Validates on both create and update
validates :email, uniqueness: true, on: :account_setup # Custom context
validates :title, presence: true, on: [:update, :ensure_title]

# if, unless
validates :card_number, presence: true, if: :paid_with_card?
validates :password, confirmation: true, unless: -> { password.blank? }
# Grouping
with_options if: :is_admin? do |admin|
  admin.validates :password, length: { minimum: 10 }
  admin.validates :email, presence: true
end
```

## Custom Validation
```rb
# Validates Each
validates_each :name, :surname do |record, attr, value|
  record.errors.add(attr, 'must start with upper case') if /\A[[:lower:]]/.match?(value)
end

# Validates With
# Initialized only once for the whole application cycle
# And not on each validation run
class CategoryValidator < ActiveModel::Validator
  def validate(record)
    if record.category == 'Deprecated'
      record.errors.add(:base, 'This category is no longer supported')
    end
  end
end

class DeprecatedValidator < ActiveModel::Validator
  def validate(record)
    if options[:fields].any? { |field| record.send(field) == "Deprecated" }
      record.errors.add(:base, 'This is no longer supported')
    end
  end
end

validates_with CategoryValidator
validates_with CategoryValidator, OrderValidator, on: :create
validates_with DeprecatedValidator, fields: %i[category tag type]

# Validate with Methods
validate :discount_cannot_be_greater_than_amount
validate :check_expiration_date, on: :create, if: :expiration_date

def discount_cannot_be_greater_than_amount
  errors.add(:discount, "cannot be greater than amount") if discount > amount
end
```

## Errors
```rb
person.valid?
person.errors.full_messages
person.errors.first.details
person.errors[:name]
```
