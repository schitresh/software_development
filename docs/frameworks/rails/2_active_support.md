## Active Support
- Provides extension methods to ruby libraries and classes

```rb
# Variable
blank?
present?
presence # a.present? ? a : nil
in?(array)
dup
deep_dup
try(:method)
# hash.try(:[], :key)
# person.try { |x| ... }
to_param # Converts string, array, hash to url params

# Array
including(items)
excluding(items)

# Object
to_json
index_by(block) # Generates a hash with the returned item as values
index_with(block) # Generates a hash with the returned item as keys
pluck(keys)
stringify_keys
stringify_keys!
symbolize_keys
symbolize_keys!
except(keys)

# String
squish # Strips whitespaces and removes multiple whitespaces
truncate(10) # Replaces long text with ...
constantize
singularize
pluralize
titleize
camelcase
underscore
humanize
to_sentence
html_safe?
html_safe
to_date
to_time
to_datetime

# Class & Modules
delegate :name, to: :profile
alias_attribute

# Date
current
beginning_of_week
end_of_week
monday
next_week
prev_week
weeks_ago(num)
weeks_since(num)
```
