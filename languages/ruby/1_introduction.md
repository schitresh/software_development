## Introduction
- Interpreted language
  - No need to compile the program before executing
  - Processed at runtime by interpreter line by line
- Dynamically typed
- Strongly typed
- Automatic garbage collection
- Various external libraries are provided through gems
- Everything in ruby is an object except the blocks

## Basics
```rb
# Single line comment
= begin
Multiline comment
= end

# Standard Input
value = gets # Adds newline by default after input
value = gets.chomp # To remove the newline

# Prints next to each other
print 'Hello'
print val1, val2 # No space
print val1, ' ', val2 # If space is required
print "Hello #{name}" # String interpolation
print [1, 2] # 12

# Prints in new line
puts 'Hello'
puts val1
puts val1, val2 # val1 and val2 will be printed in separate lines
p [1, 2] # 1 & 2 will be printed in separate lines

# Raw print
p [1, 2] # [1, 2]

# Freezing object
# Frozen object cannot be modified
# Constants are usually freezed
obj.freeze
'hello'.freeze
[1, 2, 3].freeze

# Safe Navigation
person&.name&.display

# Shell for interpreter: irb (Interactive Ruby)
```
