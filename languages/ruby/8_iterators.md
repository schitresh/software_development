## Iterators
```rb
# Works on collections like arrays and hashes

# Inline Block
array.each { |x| puts x }
# Multiline Block
array.each do |x|
  puts x
end

# & is used to pass lambda or proc as a block to a method
# For example: `array.map(&lambda_or_proc)`
# & when combined with an object calls to_proc on the object
# Explicit calling will be like `:to_s.to_proc.call(x)`
# When combined with & like `&:to_s` pass it as a block
# This is equivalent to array.map { |x| x.to_s }
array.map(&:to_s)

# Bang operator
# Updates the original array
# Without bang iterators return a new array: array.map(&:to_s)
array.map!(&:to_s)

# With index
array.each_with_index { |item, index| p(item, index) }
array.each.with_index { |item, index| p(item, index) }
```

## Destructuring
```rb
# Hash
hash.each { |key, value| p("#{key}: #{value}") }
hash.each { |item| p("#{item[0]}: #{item[1]}") } # item = [key, value]

# Array within array
[[1, 2], [3,4]].each { |a, b| p("pair of #{a} and #{b}") }
```

## Types
```rb
10.times { |x| p(x) }
(5..10).step(2) { |x| p(x) }
array.all?(&:even?)
array.any?(&:even?)

# Performs operation on each item and returns the original array
array.each { |x| x.some_operation }
(5..10).each { |x| p(x) }

# Manipulates each item and returns the new values of array
# Aliases: collect
array.map { |x| x * 2 }

# Selects the items and returns a new array with those items
# Aliases: filter
array.select { |x| x.even? }
# Rejects the even items
array.reject { |x| x.even? }

# Finds and returns the first matched value, use find_all for all matches
# Aliases: detect
array.find { |x| x = 'hello' }
array.find_all { |x| x = 'hello' }

# Aggregates the values into a variable
# Need to return the aggregator
# Aliases: inject
array.reduce { |sum, x| sum += x; sum }
array.reduce(5) { |sum, x| sum += x; sum } # initial value of sum will be 5
array.reduce(&:+)

# Custom sort
# Similar iterators: reverse_by, min_by, max_by, group_by
array.sort_by { |word| word.length }

# With initial object
array.each_with_object({}) { |x, obj| obj[x] = x * 2 }
array.each_with_object([]) { |x, obj| obj.push(x * 2) }

[1, 2].zip([3, 4]) # [[1, 3], [2, 4]]
```
