## Strings
```rb
text = 'hello'
text[1] # 'e' (Single char is also a string)
text[-4] # 'e'
text + 'world' # 'hello world'
text * 3 # 'hellohellohello'

text = "Hello World!"
para = 'Multiline'\
       'String'

'hello'.include?('el') # True

'hello\nworld' # Doesn't add new line
"hello\nworld" # Adds new line
```

## Interpolation
```rb
"Employee name is #{name}"
"Category code is #{get_code(product)}"
"Category code is #{category || DEFAULT_CATEGORY)}"
"Pecentage: #{(value / 20) * 100}"
```

## Slicing
```rb
# From left, the indexes are 0, 1, 2, ...
# From right, the indexes are ..., -3, -2, -1
# Can use [range] or slice(range)

text = 'hello world'
text[..4] # 'hello', the range (..4) alone is not valid
text[...5] # 'hello'
text[6..] # 'world'
text[..-7] # 'hello'

text[2..4] # 'llo'
text[-5..-3] # 'wor'
text[6..-3] # 'wor'

text[6..][..2] # 'wor'

text[2..3] = 'kk' # 'hekko world'
text[2..4] = 'kk' # 'hekk world'
text[2..3] = 'asdf' # 'heasdfo world'
```

## Instance Methods
- Applied on string like `string.method_name(*args, **kwargs)`

### General
```rb
length
empty? # nil?
# present? & blank? are rails methods
to_sym

str << str2 # Appends str2
insert(index, str2)
reverse # reverse!
```

### Find and replace
```rb
index(substr) # rindex
count(substr) # Counts occurence of substring

tr(substr, replace_with)
sub(regex_or_string, replace_with) # Replaces the first matching regex or string
sub('foo', hash) # Replaces foo with bar, hash = { 'foo' => 'bar' }
sub(regex_or_string) { |match| some_action }
gsub(regex_or_string, replace_with) # Replaces all the matching regex or string

start_with(prefix)
end_with(suffix)
delete_prefix(prefix)
delete_suffix(suffix)
```

### Split and Join
```py
strip # Removes trailing whitespaces
# lstrip, rstrip
split(delimiter_or_regex) # Deletes delimiter, splits and returns list of subsrings
partition(sep) # Splits the string in 3 tuples on occurence of separator
# lpartition, rpartition
array.join(str) # Joins the array using the string
```

### Case
```py
casecmp?(str2) # Compares string by ignoring case
# Use with ! to edit in place
downcase
upcase
titleize # Titlecase
capitalize # Capitalizes the first letter of string
```
