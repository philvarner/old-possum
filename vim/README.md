# Vim

Think of vi/vim as a family of text-editing tools that work using roughly the same commands. 

macvim
"classic" vim, neovim

Bindings

IntelliJ Vim bindings
Visual Studio Code Vim bindings

* https://github.com/lambdalisue/jupyter-vim-binding
  
## Resources

* [A Great Vim Cheat Sheet](http://vimsheet.com/)
* https://missing.csail.mit.edu/2020/editors/
* [Vim docs - Patterns](http://vimdoc.sourceforge.net/htmldoc/pattern.html)

### Books 

* [Practical Vim]() by Drew Neil
* [Modern Vim]() by Drew Neil
* [Learning the vi and Vim Editors](https://learning.oreilly.com/library/view/learning-the-vi/9780596529833/) by Arnold Robbins, Elbert Hannah, and Linda Lamb

## Capture Groups

```
:%s/\(\w\)\(\w\w\)/\2\1/g
```

or use `\m` magic or `\v` very magic mode -- in the pattern, characters except '0'-'9', 'a'-'z', 'A'-'Z' and '_' have their regex meaning, e.g., `()` is a capturing group rather than literal parens:

```
:%s/\v(\w)(\w\w)/\1y\2/g
```

## Other

linebreak http://vim.wikia.com/wiki/Add_a_newline_after_given_patterns

/[(,)]
:s//\r&/g


## Visual Mode Commands

| Cmd   | Action |
| ----- | ------ |
| v     | Visual mode - character |
| <S-v> | Visual mode - line    |
| <C-v> | Visual mode - block |
| gv    | Reselect last visual selection |
| o     | Toggle free end |

