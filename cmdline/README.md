# Command Line

- [Command Line](#command-line)
  - [Resources](#resources)
  - [To Remember](#to-remember)
  - [tmux](#tmux)
  - [Terminals](#terminals)
  - [Prompt](#prompt)
  - [Replay cmd line sessions](#replay-cmd-line-sessions)
  - [Tools](#tools)
  - [JSON](#json)
  - [CSV](#csv)
  - [Tricks](#tricks)
  - [Redirect all output from a program:](#redirect-all-output-from-a-program)
  - [Grouping commands](#grouping-commands)
  - [Make](#make)
  - [curl](#curl)
  - [GIF Creation from PNGs](#gif-creation-from-pngs)
  - [Webserver](#webserver)
  - [File MIME Type](#file-mime-type)
  - [Rename files and add a prefix](#rename-files-and-add-a-prefix)
  - [screen](#screen)
  - [grep](#grep)
  - [awk](#awk)
  - [sed](#sed)
    - [Resources](#resources-1)
    - [Description](#description)
  - [tr (translate or transliterate)](#tr-translate-or-transliterate)
  - [cut](#cut)
  - [paste](#paste)
  - [sort](#sort)
  - [less](#less)
  - [find](#find)
  - [parallel](#parallel)
  - [open](#open)
  - [Etc](#etc)
  - [Shell](#shell)
  - [Using killall](#using-killall)
  - [Scripting](#scripting)
  - [Scratch (todo)](#scratch-todo)
- [Scratch (todo)](#scratch-todo-1)
  - [General](#general)

## Resources

* [The Grymoire - home for UNIX wizards](http://www.grymoire.com/Unix/)
* [MIT Missing Lectures - Data Wrangling](https://missing.csail.mit.edu/2020/data-wrangling/)
* [Ron Duplain's dotfiles](https://github.com/rduplain/home)

## To Remember

* `cd -` cd back to previous directory

## tmux

* [Getting Started](https://github.com/tmux/tmux/wiki/Getting-Started)

* `tmux new`
* `C-b` puts into tmux command mode
* `C-b %` - split into two vertical panes
* `C-b "` - split into two horizontal panes
* `C-b 0` changes to window 0, etc.
* `C-b n` and `C-b p` for next and previous window
* `C-b l` last window
* C-b Up, C-b Down, C-b Left and C-b Right change to the pane above, below, left or right of the active pane. T
  
## Terminals

MacOS's Terminal is pretty bare-bones, so most people use other ones.

* [iTerm2](https://iterm2.com/) - support for tabs
* [Alacritty](https://github.com/alacritty/alacritty) - fast, beautiful rendering, good for tmux

## Prompt

* [Starship](https://starship.rs/)

## Replay cmd line sessions

* [ASCIInema](https://asciinema.org/)

## Tools

* [lnav](http://lnav.org/) The Log File Navigator
* journalctl [tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs)

## JSON

* [jq cookbook](https://github.com/stedolan/jq/wiki/Cookbook)

## CSV

* [xsv](https://github.com/BurntSushi/xsv)

## Tricks

* adding `--` to many command line tools indicates the flags have ended. This means `ls -- -l` will list a file named `-l`

## Redirect all output from a program: 

command > /dev/null 2>&1& 

## Grouping commands

conditionally do left and right, but non-conditionally do the right cmds
```
cd target/path && { curl -O URL ; cd -; }
```

Using a subshell

```
(cd target/path && curl -O URL)
```

## Make

* [Makefile Tutorial](https://makefiletutorial.com/)
* [GNU Make Manual](https://www.gnu.org/software/make/manual/)

The most important thing to remember:

Makefile text **requires** the use of tabs instead of spaces. They will not work otherwise!!!

Run these either by renaming the makefile to `Makefile` with

```
make
```

or use the `-f` flag 

```
make -f Makefile.example1
```

## curl

* `--fail` to get an error code for a 4xx or 5xx response, useful for using curl in scripts.

## GIF Creation from PNGs

`convert -loop 0 -delay 100 in1.png in2.png out.gif`

## Webserver

Run a webserver in the current directory: `npx node-static -p 8080`

## File MIME Type

`file  --mime-type {filename}`

## Rename files and add a prefix

rename 's/(.+)\.JPG/prefix_$1.jpg/' *.JPG

use `-n` flag for a dry run


## screen

* [Quick Reference](http://aperiodic.net/screen/quick_reference)
* [IU](https://kb.iu.edu/d/acuy)

```sh  
screen <cmd> # run command in screen
screen -ls # list active screen sessions
screen -r  # reattach

## hmm, not sure what this does
screen -dmS session_name sh -c '/share/Sys/autorun/start_udp_listeners.sh; exec bash'

screen -L # turn on logging

# ctrl-A d  ## to detach

## example running docker
screen -L -S my_docker_session time docker run -it -w /tmp --ulimit nofile=90000:90000 \
    -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
    -e JAVA_OPTS="-Xmx4g -Dtype.safe.config.arg=$XXX" \
    repo/image-name --some-flags
```
## grep

* after: `-A num`
* before: `-B NUM`
* around: `-C NUM` 
* case-insensitive: `-i`
* inverse match: `-v`
* names of files only: `-l`
* inverse name of file only: `-L`
* count only: `-c` 
* original line number: `-n`
* color matches: `--color=always`
* recursive dir: `-r`
* don't print filename: `-h`

Related commands:
* egrep - extended, includes char classes, bracket classes, etc. 
* fgrep - fixed strings, usually read from -f parameter 

## awk

https://www.grymoire.com/Unix/Awk.html

focused primarily on columnar data (vs. sed being for line-oriented)

default -- whitespace-separated columns, indexed from 1

Output second column:
```
awk '{print $2}'
```

> awk '/The/' text.txt 
> awk '/^rs/' text.txt 
> awk 'NR > 5' text.txt 

awk '{print $1 "|" $2 "|" $3}'

cat files.txt | xargs ls -l | cut -c 23-30 | 
awk '{total = total + $1}END{print total}' 

awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }'

## sed

### Resources

* [The Grymoire -- sed](http://www.grymoire.com/Unix/Sed.html)

### Description

Focused primarily on line-oriented data (vs. awk being more columnar)

Sed takes text input in, does some commands on the text, and sends text out. By default, it takes text in on standard in and sends the modified text out on standard out. 

The most common use of sed is for regex replacements with the "s" sed command: 

```sh
sed 's/foo/bar/' < foo.txt 
```

substitutes (s) the first instance of 'foo' on each line with 'bar', from the contents in from foo.txt out to the console. 

Since this is a standard regex replace, we can add 'g' at the end to do the replace globally (e.g., every instance) in the input: 

```sh
sed 's/foo/bar/g' 
```

Sed also allows you to specify the input file name or file names in the command, instead of having to pipe it from the file 

```sh
sed 's/foo/bar/g' foo.txt foo2.txt 
```

This default syntax for the command is shorthand for the flag '-e' 

```sh
# equivalent
sed 's/foo/bar/g' foo.txt 
sed -e 's/foo/bar/g' foo.txt 
```

This is useful if you want to specify more than one command to be run in sequence.  So this command will replace foo with bar, then bar with baz.  For very large files, this is much faster than running sed twice, since the file only has to ```be read once and output written once. 

```sh
sed -e 's/foo/bar/g' -e 's/bar/baz/g' foo.txt 
```

If you have a lot of commands to run, you can put them in a file and call sed with that file 

```sh
sed -f cmds.txt foo.txt 
```

One common use case with sed is doing a replacement on a lot files, and doing the replacements in-place on the files.  The `-i` flag will do these replacements in place on the files.  The argument to `-i` is the suffix to append to the name of the backup of the original file, and while option, is highly recommended so you don't accidentally lose your source files. 

```sh
sed -e 's/foo/bar/g' -i .bak foo.txt 
```

/ can be _ : , | 

sed -i ':a;N;$!ba;s/\0/ /g' *xml 

* `-E`  extended syntax -- parens are special chars
[ab]  -- a or b, one of many different characters

character classes:
* `(ab)`
* `(ab|bc)`
* `\2` capture groups
* `.*?` non greedy match
* `(?:ab|bc)` non-capturing on ab or bc

## tr (translate or transliterate)

     tr [-Ccsu] string1 string2     tr [-Ccu] -d string1 
     tr [-Ccu] -s string1 
     tr [-Ccu] -ds string1 string2 

-C character complement 
-c value complement 

     [:class:]  Represents all characters belonging to the defined character class.  Class names are: 

                alnum        <alphanumeric characters> 
                alpha        <alphabetic characters> 
                blank        <whitespace characters> 
                cntrl        <control characters> 
                digit        <numeric characters> 
                graph        <graphic characters> 
                ideogram     <ideographic characters> 
                lower        <lower-case alphabetic characters> 
                phonogram    <phonographic characters> 
                print        <printable characters> 
                punct        <punctuation characters> 
                rune         <valid characters> 
                space        <space characters> 
                special      <special characters> 
                upper        <upper-case characters> 
                xdigit       <hexadecimal characters> 

tr "[:lower:]" "[:upper:]" < file1 
tr -c "[:alpha:]" "\n" < file1

complement of alphabetic characters -> EOL 

but lots of dupe linebreaks 

tr -cs "[:alpha:]" "\n" < file1 

-s shrinks duplicate \n (second string) 

tr -cs "[:alpha:][:digit:]" "\n" < foo.txt 


tr -cs "[:alnum:]" "\n" < foo.txt 


Remove diacritic marks (maybe useful for misencoded chars?) 
tr "[=e=]" "e" 

Common use is 

echo 'foo bar' | tr "set1" "set2" 

Two lists of chars that are mapped by char.  

tr a b 
  

Subs a with b

tr A-Z a-z

Lowercases.  


for f in *; do mv $f `echo $f | tr A-Z a-z` done


-c complements first string, essentially not.   Second string is padded. 

Tr -c a-z \n

Replacing non az chat with lb

-d delete chars
-s squeeze repeated chars
tr -s \n [ *] 

tr -s '\n' -- merge newlines

## cut

cut -f 2,3,4 foo.txt 

-f fields to cut, 1 indexed 
-d delimiter (defaults to tab) 
-b byte or -c character positions e.g., -c 1-16,26-38 
-s suppress lines without modifiers 
-n don't split multi-byte characters 

$ echo "a,b,c,d" | cut -d ',' -f '2,3'
b,c

$ echo "a,b,c,d,e" | cut -d ',' -f '1-2,4-5'
a,b,d,e

$ echo "a,b,c,d,e" | cut -d ',' -f '1,3-'
a,c,d,e


## paste

paste files 

paste a.txt bar.txt 

creates output with per-line concatenation 

-d list of chars to use as the combiners, defaults to \t 

-s concat the lines in each file as one, rather than per-line-per-file 

- can be used as stdin, and can be used more than once: 

     List the files in the current directory in three columns: 

           ls | paste - - - 

     Combine pairs of lines from a file into single lines (the first is a tab, then an empty line, repeat) 

           paste -s -d '\t\n' myfile 


paste -sd,   # to lines -> csv line 

## sort

Case-insensitive : -f 
Sort numerically instead of lexi: -n
Even better numerical: -g 
unique: -u 
check if sorted: -c 
ignore leading whitespace: -b 
reverse : -r 
field delimiter: -t X 
keys: -k F1[.C1][,F2[.C2]] (F==field, C==character, comma is range) 

sort -n -k1,1

## less

options-- 
verbose prompt : -m 
line nums : -N 

commands-- 
quit: q 
screen forward: Space bar, f, ^V, ^F 
screen backward: b, ^B, ESC-v
line forward: Enter 
go to editor: v 
begin/end of file: < > 
next file : :n  
prev file : :p 

uniq
consecutive, duplicate lines 
add count of dupes: -c 
insensitive: -i 
unique lines only: -u 
dupe lines only: -d 
ignore N chars: -s N 
ignore N whitespace-sep fields: -f N 


## find

-name  
-path (or first arg) 
-type {d,f} directory or file 

-iname, -ipath for insensitive 
-regex : path relative to tree being searched must match 

-atime, -ctime, -mtime {+-}N - time of < == or >  
-amin, -cmin, -mmin 
-mindepth N, -maxdepth N : limit search depth 
-size {+-}Nk 

-print print relative to search dir 
-print0 null separators instead of line separators (use xargs -0 with this)  

-exec ; : invoke shell command 
-ok ; : invoke shell command with confirmation 

## parallel 

https://www.gnu.org/software/parallel/man.html 

find . -name '*.html' | parallel gzip
account for newlines: find . -type f -print0 | parallel -q0 perl -i -pe 's/FOO BAR/FUBAR/g' 
parallel gzip ::: *.html
parallel lame {} -o {.}.mp3 ::: *.wav

## open

open files/urls 
     -a : app to open with 
     --args : args to pass to app 
     -f pipe to default text editor 

## Etc

cd - : previous dir 
pwdx 
mkdir -p : create intermdiate directories 
lsof - open files 
find: find ... -print0 | xargs -0 … 
skip bad file name: find -X 
tee: -a append 
head -n N 
tail -n N 
tail -n +N 
tail -f (keep open) 
tail can intersperse multiple files 

zip -r myfile.zip dirname : recursively zip directory (like you would expect) 

nl - line numbers 
strings - extract strings from binary file 
od - binary dump 
xxd - binary dump (with reverse) 
file - guess at type of file 
column -t : put into columns  
type : what type of thing is this executable
cmp : compare binary files
renice : change priority of already running process
ifconfig -a : see all interfaces 
ifconfig interface : info on one interface 
ipconfig getifaddr en0 
host : get info on ip addr or hostname.  -a gives all info 
yes : prints yes -- useful for interactive commands 
pbcopy and pbpast : cut and paste to Mac clipboard 
expr : evaluates expression, useful in shell scripts 
wc : word count (-c for bytes, -l for lines, -m for chars, -w for words) 
join: sql-style join on columns in two files 

Change default text editor: http://superuser.com/questions/231854/default-editor-for-files-without-file-name-extension-in-mac-os-x 

-exec` and `-exec + 

## Shell

redirects both stderr and stdout to the same stream: 2>&1 |
history w/ sub: ^string1^string2^ OR !!:s/string1/string2/ OR !!:gs/string1/string2/) 

literals -- precede with ctrl-v 

; or && or || as cmd joiners 

set -o vi 

!! Re-run previous command 
!N Re-run command number N in your history 
!-N Re-run the command you typed N commands ago 
!$ Represents the last parameter from the previous command; great for checking that files are present before removing them: 
!* Represents all parameters from the previous command 

Interactive search mode : ctrl-r 

brace expansion: rm foo{1,2,3} ==> deletes foo1, foo2, and foo3 

alias foo='rm foo' 

Redirecting output 
some command 2> error.out  

some command > good.out 2> error.out  

some command &> all.out  
some command 2&1> all.out  

Mac OS X 
defaults write com.apple.finder AppleShowAllFiles TRUE 

## Using killall 

killall -KILL Finder 

killall -KILL 'Microsoft Word' 


ipconfig getifaddr en0 ==> ip address 



rsync -aE myfolder server:              Copy changed files 

## Scripting 


if test-command ; then commands ; elif test-command; then commands; else commands; fi

if [ "$a" = "$b" ] ; then echo "foo" ; fi 

if [ "$1" -eq "$2" ] ; then echo "$1";  elif [ "$1" -nq "$2" ] ; then echo "$2"; else echo "none"; fi 

-lt, -ne, -eq, -gt, -ge, -lt, -le for numerics 
=, != for strings 

test foo or [ foo ] 

for i in *; do mv $i $i.old; done

echo for simple out 
printf "a %d b" 2 : sub out 
read foo 

last command status code: $? 

test 2 -lt 1 

test has alias [ ] 

builtins true and false 

test options 
file is a dir: -d name
file is a file: -f name
file is a symlink: -L name
file is readable: -r name
file is writeable: -w name
file is executable: -x name
file is nonzero size: -s name 

f1 -nt f2 
File f1 is newer than file f2 
f1 -ot f2 
File f1 is older than file f2 
String tests 
s1 = s2 
String s1 equals string s2 
s1 != s2 
String s1 does not equal string s2 
-z s1 
String s1 has zero length 
-n s1 
String s1 has nonzero length 
Numeric tests 
a -eq b 
Integers a and b are equal 
a -ne b 
Integers a and b are not equal 
a -gt b 
Integer a is greater than integer b 
a -ge b 
Integer a is greater than or equal to integer b 
a -lt b 
Integer a is less than integer b 
a -le b 
Integer a is less than or equal to integer b 
Combining and negating tests 
t1 -a t2 
And: both tests t1 and t2 are true 
t1 -o t2 
Or: either test t1 or t2 is true 
! your_test 
Negate the test, i.e., your_test is false 
\( your_test \) 
Parentheses are used for grouping, as in algebra 



echo -n : echo with no linebreak 
read a : read a line into var $a 
$# : number of cmd line args 
$n : numbered cmd line args 
$@ : all args 
shift : shift the first arg out 
-- : ignore dashes as options (escape) 
$# -eq 0 
test options: 
-e exists 
-f exists and is file 
-d exists and is directory 
-s exists and is not empty 
-r exists and is readable 
-w exists and is writeable 
-x exists and is executable  

bash -x : debug mode 

for loop-index in arg-list 
do      commands 
done 

while test-cmd 
do      
     commands 
done 

until test-cmd 
do     commands 
done 

break, continue 

case test-string in      
     pattern-1)          commands-1          ;; 
     pattern-2a | pattern-2b)          commands-2          ;;      
     *)          commands-3          ;; 
esac 

exit n 

echo -e : interpret special chars

NAMES=(a b c d) 
${NAMES[2]} 

function nam () { 
     echo $myname 
     myname=zach 
} 

$$ : PID 
$? : exit status 
$# : num of cmd line args 
$0 : name of calling program 
$1-$n : args 
$* : all cmd line args 
$@ : array of args 


shift : promotes cmd line args 

set $(date) 

${LIT:-"foo"} : use default value 
:= : assign default value 
: ${name:=default} 
:? display error message 

read : read into $REPLY 
-a aname : array 
-d delim : instead of EOL 
-e : user Readline 
-n num : read num of char 
-p prompt :  
-s : silent 
-un : file descriptor 

exec : runs a connect  

let "VALUE=VALUE * 10 + NEW" 

[[ ]] : logical evaluation 
++, --, pre and post 
** : exponentiation 
% : remainder 

range:  {1..5} 
shell expansion -- expands to 1 2 3 4 5 
can use like : cp foo{.txt,.old} to copy file to  

zsh: 
autocomplete commands  
autoload -U compinit 
compinit


find . -name "$1" -print0 | xargs -0 grep -i "$2"



```sh
java -Xdebug -Xnoagent -Djava.compiler=NONE 
  -Xrunjdwp:transport=dt_socket,server=y,address=8787 
```

find . -o -name '*.raw' -name '*.sraw' | xargs -0 grep -il watt.seas

--- 

>   for i in `find . -name '*.raw'`; do 
>     cp $i $i.temp 
>     sed -e 's/old text/new text/g' < $i.temp > $i 
>     rm $i.temp 
>     echo $i updated. 
>   done 

------ 

grep 
-c count of all matched lines 
-h matched lines 
-i case-insensitive 
-l files, not lines 
-n lines and line numbers 
-v all lines not 

----- 

tar - remember no dashes! 

t print names of files in tar 
c create new 
r append 
x extract 
u add if not on or if modified 
f archive name 
v print function letter and names 

tar xvf name target 
tar cvf name sources 

tar -cvf `find . -print` > backup.tar 

gpg -c


PDF Converstion

tiffcp -c lzw a.tif b.tif result.tif
tiff2pdf -p letter -j -q 75 -t "Document" -f -o output.pdf input.tiff


parallel 'cd {}; cdo -f nc2 mergetime *.nc xxx/LST_{}.nc' ::: {2000..2003}

find /data/ -name "*" -print0 | xargs -0 rm
find . -type f -name "*.mp3" -exec cp {} /tmp/MusicFiles \;


## Scratch (todo)

https://unix.stackexchange.com/questions/67503/move-all-files-with-a-certain-extension-from-multiple-subdirectories-into-one-di

https://stackoverflow.com/questions/11289551/argument-list-too-long-error-for-rm-cp-mv-commands


log cmd
system log

https://gist.github.com/emadehsan/ad6e81ca595e99045abb391844f45346

gnuplot  | gnuplot -p -e 'set boxwidth 0.5; plot "-" using 1:xtic(2) with boxes'


pup https://github.com/EricChiang/pup HTML
jq https://stedolan.github.io/jq/


R w/ ggplot https://ggplot2.tidyverse.org/

uniq -c  --- collapse same lines


Remove the first line if it's blank in all java files 
find . -name '*.java' | xargs -n 1 sed -e '/./,$!d' -i .bak 
-n 1 is necessary b/c sed take one file argument 
http://stackoverflow.com/questions/1935081/remove-leading-whitespace-from-file

xargs, parallel 

General 

sort [file] | uniq -c | awk '$1 !~/1/'  

find core/src/main/resources/ -name '*.properties' | grep 'jive_i18n_..\(_..\)\?.properties' | xargs grep -h 'profile.friends.remove.confirm.text' | cut -d " " -f 3- | sed -e 's/\(.*\)/"\1"/g' | groovysh | grep '===>' 


remove blank lines
sed -i '/^\s*$/d' file.txt

awk -v n=-2 'NR==n+1 && !NF{next} /match-me/ {n=NR}1' file

# Scratch (todo)

https://unix.stackexchange.com/questions/67503/move-all-files-with-a-certain-extension-from-multiple-subdirectories-into-one-di

https://stackoverflow.com/questions/11289551/argument-list-too-long-error-for-rm-cp-mv-commands


log cmd
system log

https://gist.github.com/emadehsan/ad6e81ca595e99045abb391844f45346

gnuplot  | gnuplot -p -e 'set boxwidth 0.5; plot "-" using 1:xtic(2) with boxes'


pup https://github.com/EricChiang/pup HTML
jq https://stedolan.github.io/jq/


R w/ ggplot https://ggplot2.tidyverse.org/

uniq -c  --- collapse same lines


Remove the first line if it's blank in all java files 
find . -name '*.java' | xargs -n 1 sed -e '/./,$!d' -i .bak 
-n 1 is necessary b/c sed take one file argument 
http://stackoverflow.com/questions/1935081/remove-leading-whitespace-from-file

xargs, parallel 

## General 

sort [file] | uniq -c | awk '$1 !~/1/'  

find core/src/main/resources/ -name '*.properties' | grep 'jive_i18n_..\(_..\)\?.properties' | xargs grep -h 'profile.friends.remove.confirm.text' | cut -d " " -f 3- | sed -e 's/\(.*\)/"\1"/g' | groovysh | grep '===>' 


remove blank lines
sed -i '/^\s*$/d' file.txt

awk -v n=-2 'NR==n+1 && !NF{next} /match-me/ {n=NR}1' file


Crash your system right quick:  :(){ :|:& };: 

find . -type f | xargs -n1 grep foo

-n1 -- only run once per field separator

find . -type f -print0 | xargs -0 -n1 grep foo
-print0 is to us the null byte as the separator, and -0 to expect it
