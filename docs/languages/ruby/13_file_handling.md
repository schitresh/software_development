## Access Modes
- r: read only (default)
  - File pointer is at the beginning of the file
- w: write only
  - Overwrites the file if already exists
  - File pointer is at the beginning of the file
- r+: read & write
- w+: read & write
- a: append
  - File pointer is at the end of the file
- a+: read & append

## Read
```rb
File.open(file_name, 'r') do |file|
end

File.foreach(file_name) { |line| p line }

file = File.open(file_name, 'r')

# Seek is used to move the cursor position
file.seek(offset)
file.pos # Returns current position

file.read # Reads till the end of file
file.read(10) # Reads 10 bytes of data
file.readline # Reads one line
file.readline(10) # Reads ten lines

file.close
```

## Write
```rb
file = File.open(file_name, 'w')
file.write('hello world')
file.close
```

## Read & Write
```rb
file = open(file_name, 'a+')
file.seek(10)
file.write('hello world')
file.close
```

## Files and Directories
```py
# Files
File.rename('test1.txt', 'test2.txt')
File.delete('test1.txt')

# Directories
Dir.mkdir('new_dir')
Dir.delete('old_dir')
Dir.pwd
Dir.chdir('new_dir') # Change current directory
Dir.chdir('/home/new_dir')
```
