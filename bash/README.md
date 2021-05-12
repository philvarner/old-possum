# Bash

## Resources

- [Learn Bash the Hard Way](https://leanpub.com/learnbashthehardway) by Ian Miell
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/) by Mendel Cooper
- [Classic Shell Scripting](https://learning.oreilly.com/library/view/classic-shell-scripting/0596005954/) by Arnold Robbins and Nelson H. F. Beebe

##  Tools

- [shellcheck](http://www.shellcheck.net/)

## Example

```sh
#!/usr/bin/env bash
set -Eeuo pipefail
# set -x # print each command before executing

readonly VAR1="foo"

function help(){
  cat > /dev/stdout << END
  Some help.
END
}

 #!/bin/bash
while getopts "h?vxy:z" opt
do
  case "$opt" in
  h|\?) 
          help
          exit 0
          ;;
  v) VERBOSE=1 ;;
  x) echo '-x';;
  y) echo "-y ${OPTARG}";;
  z) echo '-z';;
  esac
done

if [[ ${VERBOSE} -gt 0 ]]
then
  set -x fi
fi

# todo

if [[ 1 -lt 2 ]] 
then
  echo 'c' 
elif [[ 1 -gt 2 ]]; then
  echo 'b' 
else
  echo 'c' 
fi

for f in $(ls *.scala)
do
  echo "$f"
done

while [[ true ]]
do 
  echo "forever"
done

case "$A" in 
a) echo "a";;
b) echo "b";;
b*) echo "b*";;
*) echo "something else";;
esac



```

## Pitfalls

* variable assignment must NOT have space around the `=` -- otherwise, it's 3 space-delimited arguments! e.g., "FOO=foo"
* not block-scoped -- variables that are assigned are in-scope for the rest of the script

* export allows sub-processes (shells?) to see the value, or run cmds with "FOO=foo some-cmd"
* `set +o history` to not write commands to history (good for passwords and keys)--> `+o` means off, lol
* VARIABLES are usually ALL CAPS
* reference in strings as "a${SOMEVAR}b" -- double quotes interpolate, single quotes do not
* put one test in each `[...]` and logical those, e.g., `! [ ... ]`, `[ ... ] && [ ... ]`
* `=` and `==` are the same in test

## Other

* `readonly A=a` ==> readonly vars
* `local A=a` ==> by default vars are global, this makes A local to the function it's defined in
* clean Bash shell ==> `env -i bash --noprofile --norc`
* `$IFS` defines the default separator for things, e.g. ` \t\n`

## Tests

* `=` and `==` are the same
* use `[[ ${A} = 'a']]` for conditionals -- `[ ${A} = 'a']` fails if `${A}` is undefined
* `[` is a builtin that does test
* `[[` is a completely different builtin that does test but handles non-existent variables correctly
* The trick `[ "x$UNDEFINED" = "x" ]` should use `[[` w/o the `x` 


* `-z ${A}` true if empty string
* `-e ${A}` true if A exists as a file
* `-d ${A}` true if A is a directory

* integer comparisons should use form like `-lt` or put double-parens instead of `[[` --> ``(( 1 < 2))``

With glob pattern matching:

```sh
[[ $a == z* ]]   # True if $a starts with an "z" 
[[ $a == "z*" ]] # literal match

[ $a == z* ]     # globbing and word splitting
[ "$a" == "z*" ] # literal match
```

## What's the difference between .bash_profile and .bashrc?

https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html

* Interactive login shell --  `~/.bash_profile`
* Interactive non-login shell, it reads and executes commands from ~/.bashrc

Add to `~/.bash_profile`:

```
if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi
```

## Prelude

Start scripts with:
```
#!/usr/bin/env bash
set -Eeuo pipefail
set -x # print each command before exec
```

* `-E` : fire ERR traps correctly
* `-e` : exit immediately on command failure. Append '|| true' or execute in conditional to disable.
* `-u` : unset variables trigger failure
* `-o` pipefail : don't prevent piped results from masking non-zero exit code
* `-x` : print each command before execution

## Static analysis

* [ShellCheck](https://www.shellcheck.net/)

## run a command in a loop

```
while [[ true ]]; do
	time {cmd}
done
```

## Default variable values

```
FOO=${VARIABLE:-default} 
VARIABLE=${1:-DEFAULTVALUE}
```

## output from running another command

```
result=$(ls)
```

## splitting with regexes

```
shopt -o nounset
declare -rx       FILENAME=payroll_2007-06-12.txt

# Splits
declare -rx   NAME_PORTION=${FILENAME%.*}     # Left of .
declare -rx      EXTENSION=${FILENAME#*.}     # Right of .
declare -rx           NAME=${NAME_PORTION%_*} # Left of _
declare -rx           DATE=${NAME_PORTION#*_} # Right of _
declare -rx     YEAR_MONTH=${DATE%-*}         # Left of _
declare -rx           YEAR=${YEAR_MONTH%-*}   # Left of _
declare -rx          MONTH=${YEAR_MONTH#*-}   # Left of _
declare -rx            DAY=${DATE##*-}        # Left of _

clear

echo "  Variable: (${FILENAME})"
echo "  Filename: (${NAME_PORTION})"
echo " Extension: (${EXTENSION})"
echo "      Name: (${NAME})"
echo "      Date: (${DATE})"
echo "Year/Month: (${YEAR_MONTH})"
echo "      Year: (${YEAR})"
echo "     Month: (${MONTH})"
echo "       Day: (${DAY})"
```

## Comparison Operators

[Comparison Ops in the Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/comparison-ops.html)

Integer comparison: -eq -ne -gt -ge -lt -le < <= > >= 

String comparison: = == != < > -z (== '') -n (!= '')

File test: -e exists

## Heredoc

substituted $MYVAR

```bash
cat > script.sh << END
  #!/bin/bash
  echo $MYVAR
END
```

literal $MYVAR (END in quotes)

```bash
cat > script.sh << 'END'
  #!/bin/bash 
  echo $MYVAR
END
```

## Scratch 

```bash
exec 5>&1
FF=$(echo aaa|tee >(cat - >&5))
echo $FF
```


```bash
if [ -z "${MY_VAR:-}" ]; then
  echo "MY_VAR was not set"
fi
```

```
function prt
{
  echo "[$(date +"%Y-%m-%d %H:%M:%S")] - $1"
}
```

```
if [ -z "$(aws s3 ls ${HUED_URI})" ]; then
    exit 1
fi
```

last exit code
```
$?
```

* https://www.topbug.net/blog/2013/04/14/install-and-use-gnu-command-line-tools-in-mac-os-x/
* https://www.topbug.net/blog/2017/07/31/inputrc-for-humans/
* http://tldp.org/LDP/abs/html/exit-status.html
* https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/
* https://ryanstutorials.net/bash-scripting-tutorial/    



pushd dirs popd 
!! previous command 
!-n nth last cmd 
!?string 

find . -name MultiPro* -exec svn log {} \; 


sed -n '1h;1!H;${g;s/search/replace/;p;}'

perl -pi.bak -e 's/(<foo>12<\/foo>\n<bar>)42(<\/bar>)/${1}33${2}/' text.txt 

```bash 
find deid_files -type f -name '*.json' \
-print0 | xargs -0 python replay.py --uri=http://localhost:3004 \
```


```
for d in */; do
    zip -r Albemarle_Board_of_Supervisors_Minutes_${d::-1}.zip $d 
done
```

```
find . -name '*.pdf' -print0 | sort -z | xargs -0 -L1 sh -c 'echo "<li><a href=\"${0:2}\">${0:2}</a></li>"' > index.html
```

## Glob

* `*` - zero or more characters (equivalent to regex `.*`
• `?` - a single character (equivalent to regex `.`)
• [abz] - a b or z
• [a-z] - range of a to z

## Array

* array --  `arr=("a" "b" "c")`
* access -- `echo "${arr[0]}"`
* length -- `echo {#arr[@]}`

## Process Substitution

make a virtual file out of command output

```sh
diff <(cmd1) <(cmd2)
```

also `>()`

## Subshell

```sh
(
  ls
) >> out
```