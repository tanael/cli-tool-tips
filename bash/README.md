# bash shortcuts

## legend

 * `C` stands for the `ctrl` key
 * `M` stands for the `alt` key

## essentials

 * `C-a` - to start of line
 * `C-e` - to end of line
 * `C-r` - search history backwards

## good to know

 * `C-g` - escape from history searching mode
 * `C-l` - clear the screen
 * `C-k` - delete from cursor to end of line
 * `C-u` - delete from cursor to start of line
 * `C-w` - delete from cursor to start of word
 * `C-y` - paste (from buffer)
 * `C-c` - terminate the command
 * `C-z` - suspend the command

## baby steps

 * `C-f` - forward one char
 * `C-b` - backward one char
 * `C-d` - delete char under cursor
 * `C-h` - delete char before cursor
 * `C-t` - swap char under cursor with previous one

## bigger steps

 * `M-b` - backward one word
 * `M-f` - forward one word
 * `M-d` - delete to end of word
 * `M-c` - capitalize under cursor and move to end of word
 * `M-u` - uppercase from cursor to end of word
 * `M-l` - lowercase from cursor to end of word
 * `M-t` - swap current word with previous word

## the accident

 * `C-s` - stop the output of the screen
 * `C-q` - resume the output of the screen

## protip

 * `C-x C-e` - edit in `EDITOR` the current command
 * `C-p` - rewind
 * `C-n` - forward
 * `C-o` - replay and forward

## capture and edit previous command in `EDITOR` with `fc`

Display commands.

```bash
fc -l
```

Display commands without numbers and in reverse order.

```bash
fc -lnr
```

Display first and second command.

```bash
fc -l 1 2
```

Display replay command after changer `pattern` by `new`.

```bash
fc -s [pattern=new] [cmd]
```

## open a file by repeating the last argument of the previous command

```bash
cat [file]
editor !$
```

## repeat all the arguments of the last command

```bash
grep blablabla /tmp/toto.txt
egrep !:1-$
```

OR

```bash
grep blablabla /tmp/toto.txt
egrep !*
```

## move to folder if !$ is a filename

```bash
grep blablabla /tmp/toto.txt
cd !$:h
```
