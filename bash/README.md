# Bash

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


https://www.shellcheck.net/


### run a command in a loop

```
while true; do
	time {cmd}
done
```