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

## Issue of running python 3.7 on MacBook M1
Complains module ctypes are not found -
```
python -c 'import _ctypes'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named '_ctypes'
```
I ended up getting Python 3.7.10 working on my M1 via: https://github.com/pyenv/pyenv/issues/1768
```
# Install x86 brew
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

# Install Python 3.7
arch -x86_64 /usr/local/bin/brew install python@3.7

export PATH=/usr/local/opt/python@3.7/bin:$PATH

python -V
# Python 3.7.10
python -c 'import _ctypes'. # works!
```

## Conda Environment on M1 Macbook
The link that saved us all: https://towardsdatascience.com/installing-tensorflow-on-the-m1-mac-410bb36b776
### Conda Env Tools:
for ease of conventions: rename new hijacked .bash_profile (bash config file) to .bash_profile_conda
```
mv .bash_profile .bash_profile_conda
```
To switch to the conda virtual environment now, we just switch to the conda hijacked profile
```
source .bash_profile_conda
```
We know we are in the virtual environment within bash when '(base)' is before our bash name
```
(base) [kyle@KyleMac:~]$
```
From there on, we have a new python environment that can run great libraries like tensorflow! (follow above link for more)