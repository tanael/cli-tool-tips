# scripting with `bash`

## don't do this

```bash
ls -l | awk '{ print $8 }'
```

```bash
if echo "$file" | fgrep .txt; then ls *.txt | grep story
```

```bash
cat file | grep pattern
```

```bash
for line in $(<file); do
```

```bash
for number in $(seq 1 10); do
```

```bash
i=`expr $i +1`
```

## essentials

Essential samples.

```bash
#!/usr/bin/env bash
# essential - how to bash properly
#
# Copyright © 12022 Nathan Lövsund
#
# essential


## List files
echo "--- files and globing"
# files in current dir
echo ./*
echo ""


## Variables
echo "--- Variables"
# 52
a=5; a+=2; echo "$a"; unset a

# 7
a=5; let a+=2; echo "$a"; unset a

# 7
declare -i a=5; a+=2; echo "$a"; unset a

# 5+2
a=5+2; echo "$a"; unset a

# 7
declare -i a=5+2; echo "$a"; unset a
echo ""


## Arrays
echo "--- Arrays"
# instead of ls
files=(./*); echo "${files[@]}"
# handmade
names=("Bob" "Peter"); echo "${names[@]}"
names=([0]="Jason" [1]="Nate"); echo "${names[@]}"
names[0]="Liam"; names[1]="Tyler"; echo "${names[@]}"
# breaking up a string
IFS=. read -ra ip_elements <<< "127.0.0.1"; echo "${ip_elements[@]}"
declare -a myfiles='([0]="/tmp/toto" [1]="/tmp/lolo" [2]="/tmp/lulu")'
# unset in array
unset 'myfiles[1]'
# dump array
declare -p myfiles
# print array
printf '%s\n' "${myfiles[@]}"
# expanding indices
first=(Jessica Sue Peter)
last=(Jones Storm Parker)
echo "${first[1]} ${last[1]}"
# dump array after filling gaps
myfiles=("${myfiles[@]}")
declare -p myfiles
# array w/o indices / associative arrays
#declare -A dict
#dict["astro"]="Foo Bar"
#declare -p dict
echo ""

## I/O
# > emtpy >> append
echo "--- I/O"
echo "This thing" > myfile
# read from stdin
read -rp "What is your name? " name; echo "Good day, $name. Woule you like some tea?"
# stdout FD 1 (file descriptor)
cat ./myfile > /tmp/myfile
cat ./myfile 1> /tmp/myfile
# stdin FD 0
cat < /tmp/myfile
cat 0< /tmp/myfile
# stderr FD 2
cat ./toto 2> /tmp/myerrors; cat /tmp/myerrors
cat ./toto 2> /dev/null
rm myfile
# redirecting both stdout and stderr to a file, by duplicating FD 1 in FD 2
cat myfile > /tmp/proud.log 2>&1
cat /tmp/proud.log
# or the same
cat myfile &> /tmp/proud.log
cat /tmp/proud.log
echo ""


## Heredocs and Herestrings
echo "--- Heredocs and Herestrings"
grep proud <<END
I am a proud sentence.
END
echo ""


## Pipes
# pipe operator creates a subshell env, var inside do not appear outside
echo "--- Pipes"
printf "rat\nbear" | grep bea
echo ""


## Brace Expansion
echo "--- Brace Expansion"
echo th{e,a}n
echo {1..9}
echo {5..1}
echo {0,1}{0..9}
echo {A..z}
echo {a..Z}
echo ""


## Process Substitution
echo "--- Process Substitution"
echo "One" > dico
echo "Two" >> dico
diff -y <(head -n 1 dico) <(tail -n 1 dico)
echo diff -y <(head -n 1 dico) <(tail -n 1 dico)
rm dico
echo ""


## Compound Commands
echo "--- Compound Commands"
# subshell
(cd /tmp || exit 1; date > timestamp)
pwd
echo ""


## Tests
echo "--- Conditions and Tests"
# condition
declare -i a=5
declare -i b=6
[[ $a < $b ]] && echo "a inferior to b"

# pattern matching
[[ 'test.png' = *.png ]] && echo "PNG file"

# cases
case $LANG in
    en*) echo 'Hello!' ;;
    fr*) echo 'Salut!' ;;
    de*) echo 'Guten Tag!' ;;
    nl*) echo 'Hallo!' ;;
    it*) echo 'Ciao!' ;;
    es*) echo 'Hola!' ;;
    C|POSIX) echo 'hello world' ;;
    *) echo 'I do not speak your language.' ;;
esac

# select
bash ./select

# success
[ -f "essential" ] || { echo "\"file\" does not exist" >&2; exit 1; }

# success because failure is negated
! [ -f "file" ] && echo "\"file\" does not exist indeed"

# failure and execute group
[ -f "file" ] || { echo "\"file\" does not exist" >&2; exit 1; }
echo ""

# use 'trap' to debug
```

Little selecting script.

```bash
#!/usr/bin/env bash
# select - select use sample
#
# Copyright © 12022 Nathan Lövsund
#
# select

echo "Which of these does not belong in the group?"; \
    select choice in Apples Pears Crisps Lemons Kiwis; do
        if [[ $choice = Crisps ]]
        then echo "Correct! Crisps are not fruit."; break; fi
        echo "Errr... no. Try again."
    done

```
