# macOS

## screen capture

* Cmd-Shift-4 - drag to select screen
* Cmd-Shift-Ctrl-4 - drag to select, then to clipboard
* Cmd-Shift-3 - entire screen

## update homebrew

```sh
#!/bin/bash

brew update
brew upgrade
brew cleanup -s
brew cask cleanup
brew doctor
brew missing
```

## disassociate all file types from Xcode

```
ln -s /System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Versions/Current/Support/lsregister ~/bin/lsregister
lsregister -u /Applications/Xcode.app
```

## Copy to clipboard & paste

```
pbcopy < ~/.ssh/id_rsa.pub 
pbpaste 
```

## Speak 

```
say 
```

## Time Machine 

Disable local backup, which can take up a lot of disk space.

```
sudo tmutil disablelocal 
```
