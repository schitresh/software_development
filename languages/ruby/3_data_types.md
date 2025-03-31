## Data Types
- Each data type is based on a class
- `nil` is used to denote nothing
- Mutable: String, Array, Hash
- Immutable: Integer, Float, Boolean, Symbol

## Boolean
```rb
# Boolean
flag = true
in_process = false
```

## Numeric
```rb
# Integer
a = 10

# Float
b = 10.25
```

## String
```rb
# String
text = 'Hello World!'
text = "Hello World!"
para = 'Multiline'\
       'String'
```

## Symbol
```rb
# Symbol
# Immutable
# Light-weight strings
status = :draft
category = :electronics

# Each symbol has only once instance
a = :value
b = :value
a.object_id == b.object_id # true
```

## Array
- Bounded and iterable
- Can store elements with different data types (as opposed to C)

```rb
# Array
# Mutable (elements and size can be changed)
items = [1, 2.5, 'one', 'two', [3, 4]]
items[2] # 'one'
items.first # 1
items.second # 2.5
items.last # [3, 4]

# Range
# (a..b) includes b
# (a...b) excludes b
(5..8) # 5, 6, 7, 8
(5...8) # 5, 6, 7
(1..3).each { |x| print x } # Will print 1, 2, 3
(1..3).to_a # [1, 2, 3]
(8..5) # 8, 7, 6, 5
```

## Hash
```rb
# Hash
# Symbol keys
word_map = { one: 1, two: 2 }
word_map[:one] # 1

# Other keys
word_map = { 'one' => 1, 'two' => 2 }
word_map['one'] # 1

number_map = { 1 =>  'one', 2 => 'two' }
number_map[1] # 'one'

list_map = { [1, 2] =>  'one', [3, 4] => 'two' }
list_map[[1,2]] # 'one'
```

## Type Casting
### Implicit Casting
- When a language compiler/interpreter automatically converts an object
- Ruby is strongly typed
  - Doesn't allow automatic type conversion between unrelated data types
  - Examples
    - 1 + '2' will raise an error
    - 1 + 2.5 = 3.5
    - 1 + true will raise an error
- Object with lesser byte size is upgraded to match the larger byte size
  - In 1 + 2.5, 1 will be upgraded to Float as 1.0
  - This is because converting 2.5 to Integer will result in loss of data

### Explicit Casting
- Using built-in functions to perform explicit conversions
  - Such as string to integer

```rb
100.to_s # 100
10.5.to_s # '10.5'
[1, 2].to_s # '[1, 2]'
String(100) # String function can also be used

'100'.to_i # 100
10.5.to_i # 10
'Hello'.to_i # 0
Integer('100') # 100

100.to_f # 100.0
'10.25'.to_f # 10.25
Float(100) # 100.0

Array('Hello') # ['Hello']
Array(['Hello']) # ['Hello']
'Hello'.to_a # Raises error

Array([1, 2]) # [1, 2]
Array((2..4)) # [2, 3, 4]
(2..4).to_a # [2, 3, 4]
```
