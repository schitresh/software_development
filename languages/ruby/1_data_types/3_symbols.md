## Symbols
-
```rb
status = :draft
status[1] # 'r'
status[-4] # 'r'
# status + 'world' raises error
status.to_s + 'status' # 'draft status'

# Each symbol has only once instance
a = :value
b = :value
a.object_id == b.object_id # true
```

## Slicing
- Similar to strings

## Instance Methods

```rb
# General
length
to_s

# Find and replace
start_with(prefix)
end_with(suffix)

# Case
casecmp?(sym2) # Compares symbol by ignoring case
downcase
upcase
capitalize # Capitalizes the first letter of symbol
```
