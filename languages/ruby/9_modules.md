## Modules
- Groups together methods, classes, constants
- Provides a namespace and prevents name clashes
- Implements mixin

```rb
module OrderDetail
  DEFAULT_STATUS = 'draft'
  NEXT_STATUSES = { 'draft': 'processing' }

  def next_status(current_status)
    NEXT_STATUSES[current_status]
  end
end

OrderDetail::DEFAULT_STATUS

module Order
  module Detail
    class Payment
      def process
      end
    end
  end
end

Order::Detail::Payment.process
```

## Importing
- `require './order_detail'`
  - '.rb' is not required
  - Other declarations
    - `require '/home/package/order_detail'`
    - `require 'order_detail'` if it is present in $LOAD_PATH
      - For example, gems are already loaded in $LOAD_PATH
  - Loads files only once
  - Changes made in file are not reflected while the program is running
  - Should be used to include gems
- `require_relative 'order_detail'`
  - Other declarations
    - `require_relative '../package/order_detail'`
  - Local files relative to the current directory
  - Prefixing `./` for current directory is not required
- `load './order_detail.rb'`
  - '.rb' is required
  - Reloads the code every time
  - Picks up any changes made in file while the program is running
  - May become overhead if the file is being used at many places
- `autoload :OrderDetail, './order_detail.rb'`
  - Lazy load, won't load the library that is not being used

## Mixins
- Ruby doesn't support multiple inheritance
  - But mixins can be used to emulate the behavior

### Include
- Imports entities as instance methods & variables
- Can be included in class or another module
- Inserts entitiess into ancestory chain between the class and superclass

```rb
module OrderDetail
  def process
  end
end

class Order
  include OrderDetail
  include Package::Payment # Nested modules
end

Order.new.process
Order.process # Raises error

# Module methods can be overrided in class
# Even if module is included after the method in class
# [Order, OrderDetail, ...]
Order.ancestors
Order.process # Preference: Order method, OrderDetail method

# Can be nested (applies to extend & prepend as well)
module OrderCategory
  include OrderDetail
end
```

### Extend
- Imports entities as class methods & variables
- Can be included in class or another module
```rb
module OrderDetail
  def process
  end
end

class Order
  extend OrderDetail
end

Order.process
Order.new.process # Raises error

# Module methods can be overrided in class
# Even if module is included after the method in class
# [Order, ...] since they are extended as class
Order.ancestors
Order.process # Preference: Order method, OrderDetail method
```

### Prepend
- Similar to include but inserted at the top of ancestory chain

```rb
module OrderDetail
  def process
  end
end

class Order
  prepend OrderDetail
end

Order.new.process
Order.process # Raises error

Order.new.process
Order.process # Raises error

# Module methods cannot be overrided in class
# Even if module is included after the method in class
# [OrderDetail, Order, ...]
Order.ancestors
Order.process # Preference: OrderDetail method, Order method
```

## Module Methods
```rb
# methods defined with self are specific to module
module OrderDetail
  def self.process
  end
end
OrderDetail.process

# Alternative way
module OrderDetail
  extend self

  def process
  end
end

# Module methods cannot be extended
class Order
  include OrderDetail
  # Or, extend OrderDetail
  # Or, prepend OrderDetail
end

Order.new.process # Raises error
Order.process # Raises error
```

### Initializers
```rb
module OrderDetail
  def self.included(target)
    # Executes when module is included in target
  end

  def self.extended(target)
    # Executes when module is included in target
  end

  def self.prepended(target)
    # Executes when module is included in target
  end
end

class Vehicle
  def self.inherited(child_class)
    # Executes when Vehicle class is inherted into child class
  end
end
```
