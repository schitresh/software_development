## Basics
```
.                   repeat last edit command
u, U                undo, undo line
CTRL + R            redo
CTRL + O            Insert mode: Execute one command
CTRL + [            Exit insert mode
```

## Screen Navigation
```
nG or ngg or :n     Move to line number n
h, j, k, l          Arrow movements
H, M, L             Highest, Middle, Lowest line of screen
nH, nL              n lines after-highest, before-last line
gg, G               First, last line of file

CTRL + F, B         Scroll forward, backward
CTRL + D, U         Scroll forward, backward half screen
CTRL + E, Y         Reposition scroll down, up
zt, zz, zb          Reposition line: top, middle, bottom
```

## Word, Line, Section Navigation
- Lowercase (w, b, e, ge) - Consider puntuations
- Uppercase (W, B, E, gE) - Skip puntuations

```
b, e                Beginning, end of current word
w, ge               Beginning of next word, end of previous word
0, $                Beginning, end of line
_, g_               Beginning, end of line without whitespace

%                   Matching bracket
(, )                Previous, next sentence
{, }                Previous, next paragraph
[[, ]]              Previous, next section
```

## Line Operations
```
gm                  Middle of line
J                   Join two lines
:x,yj               Join lines x to y
```

## Motions
```
[operation][frequency][motion] ( or [frequency][operation][motion])

a, i                around, inside
w, s, p             word, sentence, paragraph
b, B, t             (), {}, <>
), }, >, "          (), {}, <>, ""

y$                  Yank till end of line
yi(                 Yank inside ()
ya(                 Yank around ()

cw                  Change word
c2w                 Change 2 words
caw                 Change around word
ca)                 Change around ()
cG                  Change till end of file

d^                  delete till start of line
di}                 delete inside ()
dfs                 delete upto s
d/pattern           delete upto pattern
dn                  delete upto next highlighted pattern
```

## Operations
```
Basics
i, a                Insert (before cursor), append (after cursor)
I, A                Insert at beginning of line, Append at end of line
o, O                Insert in new line below, above

Change
cc, C               Change line, till end of line
r, R                Replace character, Keep replacing text
s, S                Substitute character, line (with insert mode)

Delete
dd, D               Delete line, till end of line
x, X                Delete character under, before cursor
CTRL + U            Insert mode: Delete till start of line
CTRL + W            Insert mode: Delete previous word

Yank
p, P                Paste after, before
yy, Y               Yank line
y'x                 Yank from mark x to cursor
:%y                 Yank buffer
"ayy                Yank line into named buffer a

Indentation
<<, >>              Indent line backward, forward
5>>                 Indent 5 lines
:>>>                Indent line 3 times
<[motion]           Indent backward
>[motion]           Indent forward
>}                  Indent Paragraph
[visual]5>          Indent the visual block 5 times
CTRL + T            Insert mode: Indent
CTRL + D            Insert mode: Unindent

Visual Mode
v                   custom
V                   line
CTRL + V            block
gv                  reselect previous selection
```

## Surround
- vim-surround (pre-installed in vscode)
- To add space, use opening bracket: ysiw( will add ( hello )
- To ignore space, use closing bracket: ysiw) will add (hello)

```
ysiw'               Add surround `'` in word
yss'                Add surround `'` in line
cs'"                Change surround from `'` to `"`
cst<p>              Change surround from current tag to `<p>`
ds'                 Drop surround `'`
vi'                 Select content in ''
va'                 Select content around ''
yi"                 Yank content in ""
ca)                 Change content around ()
di)                 Delete content in ()
dfs                 Delete content upto 's'
```

## Search and Substitute
```
Line
fx, Fx              Forward, backward character x
tx, Tx              Forward, backward character before, after x
;, ,                Repeat last

Current Word
*, #                Next, previous match
[i, [I              First, all lines

/pat, ?pat          Forward, backward
n, N                Next, previous match
/, ?                Repeat last

:s/old/new/g        substitute in line
:%s/old/new/g       substitute in file

&                   repeat last substitution
:noh                remove highlights
```

## Case
```
~                   Toggle case of character
g~[m]               Toggle case
gu[m], gU[m]        Lowercase, uppercase
guu, gUU            Lowercase, uppercase line
CTRL + A, X         Increment, decrement number
```

## Marking
```
mx                  mark current position as x
mX                  global mark with file and location

`x                  Jump to x
'x                  Jump to x line's start

``                  Jump to previous location
''                  Jump to previous line's start

`.                  Jump to last edit
`[, `]              Jump to last edit line's start, end
gi                  Jump to last edit with insert

CTRL + O, I         Backward, forward in jumplist
g;, g,              Backward, forward in changelist

:marks              List marks
:jumps              List jumps
:reg x              List registers of x
```

## Formatting
```
==                  Fix line indent
=G                  Indent till last line
```

## Commands
```
ZZ, :x, :wq         write and quit file
:w !sudo tee %      write using sudo
%                   current filename

:!cmd               run command
:r !cmd             read from command out
:n,m!cmd            send lines n-m to command
:!!                 repeat last command

:sh                 start external shell
:term               start internal terminal
```
