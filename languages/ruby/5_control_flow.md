## If Else
- Non nil values are evaluated to true
- 0 is also evaluated to true

```rb
# If
if marks < 30
  result = 'Failed'
elsif marks > 75
  result = 'Distinction'
else
  result = 'Passed'
end

# Inline if
result = 'Failed' if marks < 30
result = marks < 30 ? 'Failed' : 'Passed'

# Unless
unless marks > 30
  result = 'Failed'
else
  result = 'Passed'
end

# Inline unless
result = 'Failed' unless marks > 30
```

## Match Case
- Case values can be any literal, even list
- Executes the first matched condition and returns

```rb
case category:
when 'A'
  'Distinction'
when 'B'
  'Passed'
when 'C'
  'Failed'
else # Default case
  'Unknown'
end
```

## Loops
### For loop
```rb
for item in words
  puts(item)
end

for i in (1..10)
  puts(i)
end

for key, val in data
  print(key, ':', val, "\n")
end

for key in data:
  p(key) # [key, val]
```

### While loop
```rb
while i < 10
  i += 1
end

i += 1 while i < 10
```

### Until loop
```rb
# Equivalent to while not
# while i < 10
until i >= 10
  i += 1
end

i += 1 until i >= 10
```

## Jump Statements
```rb
words = ['hello', 'world']
# break
# Will print 'hello'
for item in words
  break if item == 'world'
  puts(item)
end

# next
# Will print 'world'
for item in words
  next if item == 'hello'
  puts(item)
end

# redo
# Repeats the current iteration
for i in (1..5)
  redo if i == 2 # Repeats iteration for i = 2
end

# retry
# Restart from the beginning
begin
  p('hello')
rescue
  retry # Restarts from beginning
end

for i in (1..5)
  retry if condition # Restarts from i = 1
end
```
