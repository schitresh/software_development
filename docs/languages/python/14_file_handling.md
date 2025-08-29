## Access Modes
- r: read only (default)
  - File pointer is at the beginning of the file
- w: write only
  - File pointer is at the beginning of the file
  - Overwrites the file if already exists
- r+: read & write
- w+: read & write
- a: append
  - File pointer is at the end of the file
- a+: read & append
- +: read & write
- x: exclusive creation, fails if file already exists

## Read
```py
file1 = open(file_name, access_mode = 'r')
# Seek is used to move the cursor position
# file.seek(offset, whence = 0)
# whence can have 0, 1, or 2 as values
# 0 means absolute position
# 1 means relative to current position
# 2 means relative to the file's end
file1.seek(10, 0)
file1.tell() # Returns current position

file1.read() # Reads till the end of file
file1.read(10) # Reads 10 bytes of data
file1.readline() # Reads one line
file1.readline(10) # Reads ten lines

file1.next() # Returns next line
```

## Write
```py
file1 = open(file_name, 'w')
file1.write('hello world')
file1.writelines(['hello', 'world']) # Writes the sequence to file
file1.close()
```

## Read & Write
```py
file1 = open(file_name, 'a+')
file1.seek(10, 1)
file1.write('hello world')
file1.close()
```

## Files and Directories
```py
import os

# Files
os.rename('test1.txt', 'test2.txt')
os.remove('test1.txt')

# Directories
os.mkdir('new_dir')
os.rmdir('old_dir')
os.getcwd()
os.chdir('new_dir') # Change current directory
os.chdir('/home/new_dir')
```
