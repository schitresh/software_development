# VIM

### Basics

```
.                   Repeat the last edit command
u                   Undo
U                   Undo line
CTRL + R            Redo
CTRL + O            Execute one command in insert mode
CTRL + [            Exit insert mode
```

### Navigation

#### Line

```
gg                  First line of file
G                   Last line of file
ngg, nG, :n         Line number n

H                   Highest line on screen
M                   Middle line on screen
L                   Lowest line on screen
```

#### Cursor

```
h                   Left
j                   Down
k                   Up
l                   Right
```

#### Scroll

```
CTRL + F            Scroll forward
CTRL + B            Scroll backward

CTRL + D            Scroll forward by half screen
CTRL + U            Scroll backward by half screen
```

#### Reposition

```
CTRL + E            Scroll forward without moving cursor
CTRL + Y            Scroll backward without moving cursor

zt                  Reposition line to top of screen
zz                  Reposition line to middle of screen
zb                  Reposition line to bottom of screen
```

### Jumps

-   Lowercase letters `b, e, w, ge` jump to puntuations like comma (not space)
-   Uppercase letters `B, E, W, gE` jump over puntuations like comma (not space)

#### Word

```
b                   Beginning of current word
e                   End of current word
w                   Beginning of next word
ge                  End of previous word
```

#### Line

```
0                   Beginning of line
$                   End of line
_                   Beginning of line without whitespace
g_                  End of line without whitespace
gm                  Middle of line
```

#### Paragraph

```
%                   Matching bracket

(                   Previous sentence
)                   Next sentence

{                   Previous paragraph
}                   Next paragraph

[[                  Previous section
]]                  Next section
```

### Motions

```
[frequency][operation][motion]
or [operation][frequency][motion]

# Operations
a                   around
i                   inside

# Motions
w                   word
s                   sentence
p                   paragraph
b, )                ()
B, {}               {}
t, <>               <>
"                   ""
```

### Operations

#### Join

```
J                   Join two lines
:x,yj               Join lines x to y
```

#### Insert

```
i, a                Insert (before cursor), append (after cursor)
I, A                Insert at beginning of line, Append at end of line
o, O                Insert in new line below, above
```

#### Yank

```
yy, Y               Yank line
y0                  Yank from the start of line
y$                  Yank till the end of line

yi(                 Yank inside ()
ya(                 Yank around ()

p                   Paste after
P                   Paste before
```

#### Change

```
cc                  Change line
C                   Change till the end of line
cG                  Change till the end of file

cw                  Change word
2cw                 Change 2 words
caw                 Change around word
ca)                 Change around ()

r                   Replace character
R                   Keep replacing text

s                   Substitute character (with insert mode)
S                   Substitute line (with insert mode)
```

#### Delete

```
dd                  Delete line
d^                  Delete from the start of line
dD                  Delete till the end of line

x                   Delete character under cursor
X                   Delete character before cursor

di}                 delete inside {}
dfs                 delete upto 's'
d/pattern           delete upto the pattern
dn                  delete upto the next highlighted pattern

CTRL + U            Delete till start of line (insert mode)
CTRL + W            Delete previous word (insert mode)
```

#### Indent

```
CTRL + T            Indent (insert mode)
CTRL + D            Unindent (insert mode)

<<                  Indent line backward
>>                  Indent line forward
5>>                 Indent 5 lines forward
:>>>                Indent line forward 3 times

<[motion]           Indent backward
>[motion]           Indent forward
>}                  Indent Paragraph
[visual]5>          Indent the visual block 5 times
```

### Visual Mode

```
v                   Select from the cursor
V                   Select the line
CTRL + V            Select the block
gv                  Reselect the previous selection
```

### Surround

-   Works with the vim-surround plugin (pre-installed in vscode)
-   To add space, use opening bracket: `ysiw(` will add `( hello )`
-   To ignore space, use closing bracket: `ysiw)` will add `(hello)`

```
ysiw'               Surround the word with by ''
yss'                Surround the line with by ''

cs'"                Change the surround from '' to ""
cst<p>              Change the surround from current tag to <p>
ds'                 Drop the surround ''

vi'                 Select content inside ''
va'                 Select content around ''
yi"                 Yank content inside ""
ca)                 Change content around ()
di)                 Delete content inside ()
```

### Search

```
fx                  Search character x forward within the line
Fx                  Search character x backward within the line
;                   Repeat last search forward within the line
,                   Repeat last search backward within the line

*                   Search next match for the word under cursor
#                   Search previous match for the word under cursor

/pattern            Search forward
?pattern            Search backward
n                   Next match
N                   Previous match
:noh                Remove highlights
```

### Substitute

```
:s/old/new/g        Substitute within the line
:%s/old/new/g       Substitute within the file
&                   Repeat the last substitution
```

### Case

```
~                   Toggle case of character
g~[motion]          Toggle case

gu[motion]          Lowercase
gU[motion]          Uppercase

guu                 Lowercase line
gUU                 Uppercase line
```

### Marking

```
mx                  Mark current position as x
mX                  Global mark with file and location

`x                  Jump to x
'x                  Jump to x (at the start of the line)
``                  Jump to previous location
''                  Jump to previous location (at the start of the line)

`.                  Jump to last edit
`[                  Jump to last edit (at the start of the line)
`]                  Jump to last edit (at the end of the line)
gi                  Jump to last edit with insert

CTRL + O            Backward in jumplist
CTRL + I            Forward in jumplist
g;                  Backward in changelist
g,                  Forward in changelist
:marks              List marks
```

### Formatting

```
==                  Fix line indent
=G                  Indent till last line
```

### Commands

```
ZZ, :x, :wq         Write and quit file
:w !sudo tee %      Write using sudo
%                   Current filename

:!cmd               Run command
:r !cmd             Read from command out
:n,m!cmd            Send lines n-m to command
:!!                 Repeat last command

:sh                 Start external shell
:term               Start internal terminal
```
