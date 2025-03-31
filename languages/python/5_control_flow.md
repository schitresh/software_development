## If Else
- Any non-zero and non-null values are evaluated to True

```py
if marks < 30:
  result = 'Failed'
elif marks > 75:
  result = 'Distinction'
else:
  result = 'Passed'

# Inline if
result = 'Failed' if marks < 30 else 'Passed'
```

## Match Case
```py
# Case values can be any literal, even list
match category:
  case 'A': return 'Distinction'
  case 'B': return 'Passed'
  case 'C': return 'Failed'
  case _: return 'Unknown' # Default case
```

## Loops
### For loop
```py
for item in words:
  print(item)

for i in range(10):
  print(i)

for key, val in data.items():
  print(key, val)

for key in data:
  print(key)

# For Else loop
# Executes else when the loop terminates
# Not executed when terminated by 'break' statement
# Will print 1, 2, list ended
for i in range(1, 3):
  print(i, end=', ')
else:
  print('list ended')
```

### While loop
```py
while i < 10:
  i += 1

# While Else loop
while i < 10:
  i += 1
else:
  print('Loop ended')
```

## Jump Statements
```py
words = ['hello', 'world']
# break
# Will print 'hello'
for item in words:
  if item == 'world': break
  print(item, end=' ')

# continue
# Will print 'world'
for item in words:
  if item == 'hello': continue
  print(item, end=' ')

# pass
# Does nothing
# Ellipses (...) can also be used instead of pass keyword
# Will print 'hello world'
for item in words:
  if item == 'hello': pass # do nothing
  print(item, end=' ')
```
