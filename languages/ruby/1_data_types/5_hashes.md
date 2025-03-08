## Hashes
```rb
subjects = { phy: 'Physics', chem: 'Chemistry', math: 'Mathematics' } # Symbol keys
numbers = { 1 => 'one', 2 => 'two' } # Number keys
words = { 'one' => 1, 'two' => 2 } # String keys
lists = { [1, 2] =>  'one', [3, 4] => 'two' } # Array keys

[['a', 100], ['b', 200]].to_h # From list of lists: { 'a' => 100, 'b' => 200 }
{**hash1, **hash2}
```

### Accessing
```rb
# If key is not present, it will return nil
subjects[:math] # Mathematics
numbers[1]
words['one'] # 1
lists[[1, 2]]

subjects[:math] = 'Algebra'
subjects[:stat] = 'Statistics'
subjects.delete(:math)

subjects.to_a # Array of pairs
subjects.each { |key, value| p(key, value) }
subjects.include?(:phy) # true
```

## General Methods
```rb
empty? # nil?
# present? & blank? are rails methods
each_key(&block)
each_value(&block)
each_pair(&block)
```

## Instance Methods
```rb
# Return view objects that are refreshed dynamically whenever anything changes
# Need to typecast to get a list from them
keys
values
entries # [[key1, val1], [key2, val2]]
dig(:key1, :key2, ...) # Gets value from nested hash

default # Returns default value
default = value # Sets default value

# Updates the existing keys from dict2 and adds new keys
# List of lists or tuples, or keywords can also be used instead of dict2
merge(hash2) # merge!
merge(key1: val1, key2: val2, ...)
merge(h2, h3)
merge(h2, h3) { |key, old_val, new_val| old_val + new_val }
transform_keys(&block) # Updates all keys as specified in the block
transform_values(&block) # Updates all values as specified in the block
dup

compact # Removes nil elements
delete(:key) # Removes the key and returns the value
except(**keys) # Get a new hash with given keys
```
