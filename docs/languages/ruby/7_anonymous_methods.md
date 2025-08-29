## Blocks
- Consists of chunks of code
- Invoked by using the yield statement
- Only entity in ruby which are not objects
  - It's just a part of the syntax of a method call

### Using Yield
```rb
def test
  puts 'Method Started'
  yield # Invokes block
  puts 'Method Ended'
end

test do
  puts 'Block Executed'
end
# Method Started
# Block Executed
# Method Ended

# Alternatives
# test { puts 'Block Executed' }
# test {
#   puts 'Block Executed'
# }

def test
  puts 'Method Started'
  yield 'Hello' # Invokes block
  yield 'World' # Invokes block
  puts 'Method Ended'
end

test do |text|
  puts "Block Executed: #{text}"
end
# Method Started
# Block Executed: Hello
# Block Executed: World
# Method Ended
```

### Using Reference
```rb
def test(&block)
  puts 'Method Started'
  block.call # Invokes block
  puts 'Method Ended'
end

test do
  puts 'Block Executed'
end
# Method Started
# Block Executed
# Method Ended
```

## Proc
- A proc object is an encapsulation of a block of code
  - Which can be stored in a local variable, passed to a method or another proc
  - And can be called

```rb
adder = Proc.new { |a, b| a + b }
adder.call

multiplier = Proc.new { |x| x * 2 }
[1, 2, 3].map(&multiplier)

# Returns from current context
def process
  adder = Proc.new { |a, b| return a + b }
  adder.call(1, 2)
  print('Hello')
end
process # Returns 3, 'Hello' won't be printed
```

## Lambda
- An implementation of proc, returns a proc object

```rb
adder = ->(a, b) { a + b }
adder = lambda { |a, b| a + b }
adder.call(10, 20)
# Other calling methods
adder.(10, 20)
adder[10, 20]

multiplier = ->(x) { x * 2}
[1, 2, 3].map(&multiplier)

# Returns like a normal method
def process
  adder = ->(a, b) { return a + b }
  adder.call(1, 2)
  puts('Hello')
end
process # Returns nil, 'Hello' will be printed
```

## Closure
- Concept that proc & lambda carry the context
  - Variables & methods are referenced from the context where they are defined

```rb
def test(printer)
  counter = 100
  printer.call
end

counter = 1
printer = Proc.new { puts counter }
test(printer) # Prints 1 (and not 100)
```
