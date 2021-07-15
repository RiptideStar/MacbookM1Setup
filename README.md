# Configure Apple M1 MacBook

## Change default terminal shell from zsh to bash
Terminal > Preferences -> custom shell

## reuse rsa keys across or gen new key set 
1. if reuse, copy old key sets to new mac. No need to upload pub key to Github and other servers
2. If new keys, need to upload pub keys to Github and other servers

## Config bash .profile / .bashrc
Reuse the setting from PC linux

## install homebrew on Mac
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-homebrew-on-macos

## The color scheme for ls on mac is different from linux
To be consistent, use coreutil; (gnu version of cmds).
```
brew install coreutils
```
add this line in .bashrc
```
PATH="/opt/homebrew/opt/coreutils/libexec/gnubin:$PATH"
```

## Install pyenv to manage multiple Python envs
```
brew install pyenv
```

However, `pyenv install 3.7.10` would run into problem on Apple M1 Macbook -
```
pyenv install 3.9.1

python-build: use openssl@1.1 from homebrew
python-build: use readline from homebrew
Downloading Python-3.9.1.tar.xz...
-> https://www.python.org/ftp/python/3.9.1/Python-3.9.1.tar.xz
Installing Python-3.9.1...
python-build: use readline from homebrew
python-build: use zlib from xcode sdk

BUILD FAILED (OS X 11.1 using python-build 1.2.22-12-gbb14f225)

Inspect or clean up the working tree at /var/folders/87/4qkbk8qx0pngsqr3k054m2640000gn/T/python-build.20210110005845.35542
Results logged to /var/folders/87/4qkbk8qx0pngsqr3k054m2640000gn/T/python-build.20210110005845.35542.log
```
https://github.com/pyenv/pyenv/issues/1768

```
this issue is related to using GNU coreutils from brew. When I removed GNU coreutils from my $PATH, Python built fine with the builtin macOS tools.
```

## `pyenv global 3.7.11` wont take effect unless `pyenv init` is configured

Need to config pyenv in `.profile` or `.bash_profile` -
```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
```


