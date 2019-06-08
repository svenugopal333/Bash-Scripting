# Bash-Scripting
My complete notes on bash scripting
This one meant for my complete notes on bash scripting which i have taken from the training


Operator          Description

! EXPRESSION	     ( The EXPRESSION is false. )

-n STRING	      (  The length of STRING is greater than zero. )

-z STRING       (  The lengh of STRING is zero (ie it is empty). )

STRING1 = STRING2	  (  STRING1 is equal to STRING2  )

STRING1 != STRING2  (  STRING1 is not equal to STRING2  )

INTEGER1 -eq INTEGER2  ( INTEGER1 is numerically equal to INTEGER2  )

INTEGER1 -gt INTEGER2  (  INTEGER1 is numerically greater than INTEGER2  )

INTEGER1 -lt INTEGER2  (  INTEGER1 is numerically less than INTEGER2  )

-d FILE	 ( FILE exists and is a directory. )

-e FILE	 ( FILE exists. )

-r FILE	 ( FILE exists and the read permission is granted. )

-s FILE	( FILE exists and it's size is greater than zero (ie. it is not empty). )

-w FILE  (	FILE exists and the write permission is granted. )

-x FILE	 (  FILE exists and the execute permission is granted. )


Shell scripting:

echo hello, world!

=> Add the path of the scripts exists 

=> Add executable permission to ur script using this command
chmod u+x /path/of/file
chmod a+x /path/of/file (exec by everyone)

=> Always add the interpreter on the very first line of the script (shbang)
#!/bin/bash

=> Add some comments with prefix of # to be ignored by the interpreter


=> pass first param of script executables to notes.txt
echo $1 >> ~/notes.txt
./notes sometext (will work)
./notes some text (wont work)
./notes "some text string" (will work)


=> if u want to accept any num of args then simply modify the code as follows

echo $* >> ~/notes.txt


=> to add date value to the command

echo $(date): $* >> ~/notes.txt


=> add path of the script to the PATH var
PATH="$PATH:/path/of/all/scripts"
so now i can run the script from any dir


=> Always give Absolute path of log file instead of relative path

=> Shebang (must be first line of all your scripts)
for adding interpreter info
#!/bin/bash
or
#!/usr/bin/env bash

this will find bash on the user's path so that it will work on any 


=> script name must not be system command names , to check the name is sys command use "type" command

type test (will return as built in)

=============================================================
Variables:

myvar="hello"

echo $myvar

myfiles="file1 file2 file3"

touch $myfiles (creates file1 file2 file3)

=> variable names are case sensitive

=> all sys variables are in upper case 
eg:
echo $myvar, $USER

=> In a string variable assignments we might need to use double quotes because it has to be single word
mystring=$hello, $USER (this will throw error)

right syntax:
mystring="$hello, $USER"

x = 5 (throw error)
x=5   (right syntax to assign)

-------------------------

=> "read" command to get run time user input through command line

read -p "Enter your note: " note
echo $(date): $note >> ~/notes.txt
echo Note saved: $note


or 

date=$(date)
echo $date: $note >> ~/notes.txt


------

=> create first arg as script name

topic=$1
echo $date: $note >> ~/${topic}notes.txt

myscript shopping
Enter you note: my eggs
Note saved:

------

much more readability:

# get the date
date=$(date)

# get the topic
topic="$1"

# filename to write to
filename="${HOME}/${topic}notes.txt"

# ask input
read -p "your note" note

echo "$date: $note" >> "$filename"
echo "Note '$note' saved to $filename"


run: myscript worknotes
--------------
Note: Always use double quotes while saving the user input

=> use "$x" instead of $x
=> use braces 
echo "${foo}bar" - prints value of var "foo" followed by string "bar"
echo "$foobar" prints value of "foobar" variable

=> use $HOME instead of ~ 
---------------

how to do Debugging

either we can add it in interpreter which will enable the debugging for the whole script

#!/bin/bash -x  

or

if you want to enable debugging for a specific lines of code then make use of "set -x" (enable debugging) and "set +x" (disable debugging)

=> + prefix symbol indicates debugging enabled on those lines of code

#===================

IF loop and error code

if [[ $note ]];then
   echo "$date: $note" >> "$filename"
   echo "Note: '$note' saved to $filename"
else
   echo "No input: note wasn't saved"
fi

Usual syntax of IF loop

if testcode; then
   # ur code here when succeeds condition
else
   # when condition fails
fi

(or)

if testcode; then successcode; else failcode; fi


#===================

RETURN CODE 

exit status from 0 to 255

0 - succcess 
other values are errors

shell scripts return values with exit
exit 0 indicates script successfully executed

=> always program your code to exit with the right value

#===================

CONDITIONAL EXPRESSION

- tests on files and directories
- tests on strings
- arithmetic tests

[[ expression ]]

eg:
[[ $str ]]  -  if str is not empty return true
[[ $str = "sometext" ]] - if str equals string "sometext" return true
[[ $str="sometext" ]] - always returns true
[[ -e $filename ]] - file $filename exists
[[ -d $dirname ]] - $dirname is a directory

=> space in the expression is very critical

-----------

# Is there an argument
if [[ ! $1 ]]; then
   echo "Missing argument
   exit 1
fi

bindir="${HOME}/bin"
filename=#${bindir}/$1"

if [[ -e $filename ]]; then
   echo "File ${filename} already exists"
   exit 1
fi

# check bin directory exists
if [[ ! -d $bindir ]]; then
   # if not created then
   if mkdir "$bindir"; then
	echo "Created ${bindir}"
   else
	echo "Could not create ${bindir}"
	exit 1
   fi
fi

exit command with return code of 1 - this may be helpful if you execute multiple scripts and the status of this script might help the other one

ELIFs

if [[ $1 = "cat" ]]; then
	echo "meow"
elif [[ $1 = "dog" ]]; then
	echo "woof"
elif [[ $1 = "cow" ]]; then
	echo "Moo"
else
	echo "unknown animal"
fi

Note: use ! to negate a test
 [[ ! -e $file ]]

use spaces carefully around !

use && for "and"
[[ $# -eq 1 && $1 = "foo" ]]
true if there is exactly 1 argument with value "foo"


use || for "or"
[[ $a || $b ]]
true if a or b contains a value or both contains value


=====================

Input and Output

ECHO 
-n suppresses the newline
-e allows use of escape sequences
\t: tab
\b backspace


use PRINTF for more options in format spring

printf "hello"

=> printf will not go for new line be default

so printf "hello\n"

use of variable 

printf "hello $s, how are you? \n" $USER

printf "p%st\n" a e i o u
pat
pet
pit
pot
put

printf "%ss home is in $s" $USER $HOME

printf "|$20s |%20s |%20s |\n" $(ls)
gives in a 20 width output

printf -v greeting "hello %s, how are you? \n" $USER

echo $greeting

so -v will send output to a variable

=====================

READ

=> read input into a variable
=> "read x"
=> no variable specified ? will use REPLY
=> -n or -N specifies num of char to read
=> -s will suppress output (useful for passwd)
=> -r disallows escape sequences, line continuation
=> always use -r
=> read can read words in a line into multiple variables

read x y (gets two input and assign it to x and y)

read -r; echo $REPLY
removes escape char entered by user

echo -n "are you sure [y/n]? "
answered=
while [[ ! $answered ]]; do
	read -r -n 1 -s answer
	if [[ $answer = [Yy] ]]; then
		answered="yes"
	elif [[ $answer = [Nn] ]]; then
		answered="no"
	fi
done
printf "\n%s\n" $answered

reads only single char

read a b
hi here im
echo $a # returns hi
echo $b # returns here im

#========================

Standard streams

input , output , error
represented by num(file descriptor), or special file

0 - standard input (stdin) i.e /dev/stdin
1 - standard output (stdout) i.e /dev/stdout
2 - standard error (stderr) i.e /dev/stderr

stderr used for diag or error message

/dev/null discards all data sent to it

--------------------------

REDIRECTION 1 

get input from somewhere else,
send output or errors somewhere else

input redirection: <
grep milk < shoppingnotes.txt

output redirection: >
ls > listing.txt  (it will overwrite files)
ls >> listing.txt ( appends to EOF)

pipes
ls | grep x

----------------------------------------

REDIRECTION 2

redirect a specific stream with N>
"cmd 2> /dev/null" discards all errors

redirect to a specific stresm >&N
>&2 sends output to stderr (equivalent to 1>&2)
2>&1 redirects stderr into stdout

sending both error and output to a single file
cmd > logfile 2>&1

dont do this: 
cmd > logfile 2> logfile

dont use &> or >&

redir can be allowed anywhere on command line , but order matters

cmd < inputfile > outputfile
>outputfile cmd < inputfile
both are same

cmd >logfile 2>&1 (sends errors to logfile)
2>&1 >logfile cmd (send errors to stdout and output to logfile)


ls > demo (ls output to demo file) 

tn > demo 2>&1 ( redirec output and error to demo file)

tn > demo 
tn 1> demo
both are same

=========================================
WHILE loop

while test; do
   code to be repeated
done
code executes as long as test is true

-----------
UNTIL loop


until test; do
   code to be repeated
done
code executes as long as test returns false

----------- 

FOR Loop

for var in words; do
	code to be repeated
done

=> do not quote WORDS in loop

for i in just a little; do echo $i; done
just
a
little

for i in "just a little"; do echo $i; done
"just a little"

-----------------------

CASE

case WORD in
	PATTERN1)
		code for pattern 1;;
	PATTERN2)
		code for pattern 2;;
	PATTERNn)
		code for pattern n;;
esac

=> use * string
? single char
[] speccific set of chars

case $1 in
	cat)
		echo "meow";;
	dog)
		echo "woof";;
	cow)
		echo "mooo";;
	*)
		echo "unknown animal"
esac

==============

COMMAND GROUPS

group commands with {}
=> group them into a single statement
=> can use I/O redir for whole group
=> use group in an if statement or while loop
=> return status is that of the last command in the group

{cmd1;cmd2;cmd3;}
=> seperate the commands with newlines or semicolons
=> use spaces around braces
=> ending semicolon and new lines are mandatory

===========

|| and &&

execute next statement depending on return status of previous statement

&& - will execute next statement only if previous one succeeded
mkdir newdir && cd newdir

|| - will execute next statement only if previous one failed
[[ $1 ]] || echo "missing argument" >&2

multiple if statement can be put it in a single line

if [[ $# -ne 2 ]]; then
    echo "need exactly two arguments" >&2
    exit 1
fi

can be written as

[[ $# -ne 2 ]] && { echo "Need exactly two args" >&2; exit 1 }

#==================================

EXPORTING VARIABLES

variables are local to your script or terminal session

when u export a var , we are making it avail for subprocesses and use export var when you cannot pass a var to the program that runs your script

export var - means var avail for the subprocess of that sesison

#===================================

HANDLING SCRIPT PARAMETERS

special variables:

Postiitonal param such as $1 $2

$* and $@  (all params)

$# (total no of params)

$0 (command itself)


----

shift & getopts

$0 - holds the name of script as it was called

$@ - all params equival to $1 $2 .. $N

"$1" "$2" "$3" ... "$N"

however when double quoted stay intact

$* - equivalent to $1 $2 $3 ... $N
when double quoted "$1 $2 .. $N"
thats why always use $@


$# - total no of args passed to script

#======================

SHIFT - removes first argument

makes all positional param shift

value of $2 -> $1
value of $3 -> $2
etc

also $# lowered by 1

#=================

GETOPTS - utility help to parse argument lists

expects options to start with a dash (-x)
allows options that take an argument (-f file)

#===============

FUNCTIONS

1. declare
2. use
3. return data
4. export 

------

define your own command

function myfunc () {}
or
function name {}

give some arg (posi param)
use redirection

eg:

sum() {
return $(( $1 + $2 ))
}

sum 4 5
echo $?
--------
echo $(sum 4 5)

--------

starts_with_a () {
[[ $1 == [aA]* ]];
return $?
}

if starts_with_a axe; then
	echo "yup"
else 
	echo "nope"
fi

================

Bash variables are globally visible

=> exit a function with return
=> returns a status code, like exit
=> without a return statement, function returns status of last statement

While returning any other value
=> Use a global variable
=> send the data to output and us e command substitution

ExPORTING A FUNC
=> export -f func

==========================

STRING MANAGEMENT

Removing a pattern

To remove part of a string:
${var#pattern} removes shortest match from begin of string

${var##pattern} removes longest match from begin of string

${var%pattern} removes shortest match from end of string

${var%%pattern} removes longest match from end of string

pattern matching is like pathname matching with *, ?, []

example with i="/users/reindert/demo.txt"
${i#*/} = returns users/reindert/demo.txt
${i##*/} = returns demo.txt
${i%.*} = returns /users/reindert/demo
${i%/*} = returns /users/reindert

#====

search and replace

${var/pattern/string} = Substitue first match with string
${var//pattern/string} = Substitue all matches with string

Anchor your pattern

${var/#pattern/string} matches beginning of string
${var/%pattern/string} matches end of string
i="mytxt.txt"
echo ${i/txt/jpg}  => myjpg.txt

echo ${i/%txt/jpg} => mytxt.jpg

echo ${i//txt/jpg} => myjpg.jpg

echo ${i//txt/} => my.

echo ${i/%txt/} => mytxt.

echo ${i%txt} => mytxt.

echo ${i#txt} => mytxt.txt

echo ${i/[yx]/a} => matxt.txt

echo ${i//[yx]/a} => matat.tat

NOTE: oneliner 

[[ $# -ne 2 ]] && { echo "Need exactly two args" >&2; exit 1; }

#quick script to change the ext of files

for f in *"$1"; do
   mv "$f" "${f/%$1/$2}"
done

run: myscript txt jpg

=================

Setting a default values

=================

Conditional expression patterns

[[ $var == pattern ]] returns true when $var matches pattern
[[ $filename == *.txt ]] 

Use quotes to force string matching

[[ $var =="[0-9]*" ]] matches the string "[0-9]*"

[[ hello = h*o ]] && echo yep  => output yep

[[ hello = "h*o" ]] && echo yep => false

[[ "h*o" = "h*o" ]] && echo yep => true

[ hello = h*o ] && echo yep => returns false

========================

conditional expression patterns

=~ does reg expression matching

? matches the token before it 0 or 1 times 
eg: [0-9]? will match a single digit or nothing at all

* matches the token before it for any number of times
eg: [a-z]* will match any lowercase text or nothing at all

+ matches the token before it for one or more times
eg: [0-9]+ will match 1 or more digits

^ matches start of string
$ matches end of string

========================

End of options

is denoted by --

good to use a var as an arg for a cmd
use -- with the commands you call

touch -a.jpg (give error)

touch -- -a.jpg (works)

=======

Running code from a file

using hash-bang and running it as a command

will start a bash subprocess
executable permission has to be set

when there is no hashbang in ur script

bash myscript
no permissions necessary
bash -x myscript

---------------------

To import code in the current shell process use "source myscript"

---------------------

Background and nohup

myscript & => executes scripts in background

note : this background exec will be suspendded if it tries to read input from terminal

======

For long running scripts
if u want to keep ur scripts running even when u exit the terminal session then use nohup

nohup myscript &

=====

if u want to run your script with a lower priority then use NICE

nice myscript
or
nohup nice myscript &

======

REDIRECT IO for the whole script with EXEC

useful for logging

exec >logfile 2>errorlog 

======

running your script another time


AT 
will exec your script at specific time
at -f myscript noon tomorrow

CRON
will execute your script according to schedule

=========

SET and SHOPT

customize bash behaviour

set -x prints each cmd and arg as it is executed

-u gives error when using uninitialized var and exits script

-n read command but do not execute

-v print each command as it is read

-e exits script whenever a command fails( but not with if while until || &&)


SHOPT

can set many options with -s, unset with -u

shopt -s nocaseglob: ignore case with pathname expansion

shopt -s extglob: enable extended pattern matching


shopt -s dotglob: include hidden files with pathname expansion

set -o noclobber: dont overwrite on redirection operations

=====

Summary
