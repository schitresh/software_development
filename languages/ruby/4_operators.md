## Arithmetic Operators
- Includes regular operators like +, -, *, /

```rb
# Floor Division
# 11 / 4 = 2
# 11.0 / 4 = 2.75
# 11 / 4.0 = 2.75
a / b

a % b # Modulus
a ** b # Exponent
```

## Logical Operators
```rb
a && b
a and b # 'and' has lower precedence than '&&'
# 'and' has even lower precedence than '='

# 'b' will be assigned because ('a' && 'b') is evaluated first
value = 'a' && 'b' # value = ('a' && 'b')
# 'a' will be assigned because (value = 'a') is evaluated first
value = 'a' and 'b' # (value = 'a') and ('b')

a || b
a or b # 'or' has lower precedence than '||'
# 'or' has even lower precedence than '='

# nil will be assigned because (nil || 'b') is evaluated first
value = nil || 'b' # value = ('a' || 'b')
# 'b' will be assigned because (value = nil) is evaluated first
value = nil or 'b' # (value = 'a') or ('b')

!a
not a
```

## Membership Operators
```rb
a = 10
b = [10, 20]

b.include?(a) # True
!b.include?(a) # False
{ 10: 'ten' }.includes(a) # True
```

## Identity Operators
```rb
[10, 20].is_a?(Array)
'Hello'.is_a?(String)
person_object.is_a?(PersonClass)

[10, 20].instance_of?(Array)
[10, 20].kind_of?(Array)
```

## Other Operators
- Comparison: ==, !=, >, <, >=, <=
- Assignment: =, +=, -=, *=, /=, etc.
- Bitwise: &, |, ^, ~, <<, >>

## Equality Operators
```rb
# Generic or value equality
a == b

a = 1
b = 1
a == b # true
1 == 1.0 # true

# Hash equality
# If both have same type & equal values
a.eql?(b)

a = 1
b = 1
a.eql?(b) # true
1.eql?(1) # true
1.eql?(1.0) # false

# Case equality
# Used in 'when' clauses of 'case'
# Useful implementations in range, regex, proc
a === b

2 === 2.0 # true
2.0 === 2 # true
(1..5) === 2 # true
2 === (1..5) # false
/[a-z]*/ === 'hello' # true
Integer === 5 # true

# Identity Comparison
a.equal?(b) # If both have same object id

a = 10
b = 10
a.equal?(b) # false
a.equal?(a.dup) # false
1.equal?(1)
```
