## Basics
```
whoami                          Display username
whatis cmd                      What is cmd
whereis cmd                     Where is cmd
locate file                     Find file by quick search of system index
man cmd                         Manual of cmd
cmd --help                      Usage of cmd
history                         History of commands used
watch -n t 'cmd'                Execute cmd & output every t seconds
```

## Disk
```
df                              Disk info
du                              Disk usage
```

## Shell
```
reset                           Reload the shell
env                             Show env variables
echo $KEY                       Value of KEY
export KEY=value                Set value to KEY
$PATH                           Executable search path
$HOME                           Home directory
$SHELL                          Current shell
```

## Shortcuts
```
CTRL + C                        Stop current command
CTRL + Z                        Sleep program
CTRL + A                        Start of line
CTRL + E                        End of line
CTRL + U                        Cut from start of line
CTRL + K                        Cut to end of line
```

## Last Command
```
!!                              Repeat last command
!abc                            Run last command starting with abc
!abc:p                          Print last command starting with abc
!$                              Last argument of previous command
ALT + .                         Last argument of previous command
!*                              All arguments of previous command
^abc^123                        Run previous command, replacing abc with 123
```

## Input Output
```
Data streams: stdin(0), stdout(1), stderr(2)
cmd < file                      Input file to cmd
cmd << delimiter                Append input to cmd till delimiter
cmd > file                      Output to file
cmd >> file                     Append output to file
cmd > /dev/null                 Discard output
cmd 2 > file                    stderr to file
cmd 1 > &2                      stdout to stderr
cmd 2 > &1                      stderr to stdout
cmd & > file                    Every stdout of cmd to file
```

## Pipes, Nesting, etc
```
cmd1 | cmd2                     Pass output of cmd1 to cmd2
cmd1 | &cmd2                    Pass error of cmd1 to cmd2
cmd1 | xargs cmd2               Pass output of cmd1 as argument to cmd2

cmd1; cmd2                      Run cmd1, then cmd2
cmd1 && cmd2                    Run cmd2 if cmd1 is successful
cmd1 || cmd2                    Run cmd2 if cmd1 not successful
cmd &                           Run cmd in subshell
```

## Directory
```
basename path                   Directory name
dirname path                    Parent Directory name
pwd                             Present working directory
mkdir dir                       Make directory
cd dir                          Change directory
cd ..                           Go back
ls                              List
  -a                              all
  -R                              recursive
  -r                              reverse
  -t                              last modified
  -S                              sort by file size
  -l                              long listing format
  -1                              one file per line
  -m                              comma seperated
  -Q                              quoted
```

## Search
```
grep pattern file1 file2              Search for pattern in file1 & file2
grep -i pattern file                  Case insensitive
grep -I pattern file                  Ignore binary files
grep -m n pattern file                Stop after n searches
grep -n pattern file                  Show line number
grep -o pattern file                  Show only matched part of file
grep -q pattern file                  Quiet, do not show output
grep -r pattern .                     Search recursively in sub-directories
grep -v pattern file                  Inverted search (not matching)
grep -w pattern file                  Search as a word

find . -name pattern                  Find file with name as pattern
find . -iname pattern                 Case insensitive name
find . -name pattern -delete          Delete matched files
find . -name pattern -depth 2         Directory depth
find . -name pattern -user uname      Owned by user uname
find . -name pattern -type f          Search only files
find . -name pattern -type d          Search only directories
```

## Files
```
touch file                      Create file
cat file1 file2                 Concatenate files and stdout
less file                       View and paginate file
file file                       Get type of file
cp file1 file2                  Copy
mv file1 file2                  Move
rm file                         Remove
head file                       Show first 10 lines
tail file                       Show last 10 lines
tail -n 2 file                  Show last 2 lines
tail -F file                    stdout last lines of file as it changes
```

## Permissions
```
chmod 777 file                  777: <owner><group><everyone>
chmod -R 777 folder             Recursively change mode in folder
chown user:group file           Change file owner to user and group to group

4: read(r)
2: write(w)
1: execute(x)

Calculate permission number by adding these numbers
7: rwx
6: rw
5: rx
4: w
2: r
1: x
```

## Processes
```
ps                              Show snapshot of processes
top                             Show real time processes
kill pid                        Kill process with id
pkill name                      Kill process with name
killall name                    Kill all processes with names starting with name
```

## Manipulation
```
cut -d 'delimiter' -f n         Split by delimiter and print the field at nth position
cut -d ' ' -f 2                 Split by space & print the second field
cut -w -f 2                     Split by whitespaces
cut -c n                        Show till n characters
cut -c n-                       Show after n characters
cut -c m-n,r-s,t-               Show from m to n and r to s and after t characters

awk
awk '{print $1, $2}'            Print the first and second field

echo 'hello' |
  tr 'e' 'f'                    Replace e with f
  tr -d 'e'                     Remove e
```

## Info
```
iwconfig                        Network Interface
lscpu                           CPU
sensors                         Sensors and temperature
watch -n 2 sensors
```

## Services
`service --status-all`
