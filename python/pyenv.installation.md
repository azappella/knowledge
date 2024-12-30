# pyenv installation

## Getting started (mac os)

Install system dependencies (Xcode Command Line Tools) and then

```
brew install openssl readline sqlite3 xz zlib tcl-tk@8
```

Install pyenv first with pyenv-virtualenv plugin

```
brew install pyenv
```

Setup your shell environment

e.g zsh

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc

echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc

echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc
```

Install the correct version of python

```
pyenv install <version>
```

Optionally define a global python 

```
pyenv global 3.13.1
``` 

## Switch between python versions

To select a Pyenv-installed Python as the version to use, run one of the following commands:

```
    pyenv shell <version> -- select just for current shell session
    pyenv local <version> -- automatically select whenever you are in the current directory (or its subdirectories)
    pyenv global <version> -- select globally for your user account
```

## list all versions of python3

```bash
pyenv install --list | grep "3\.[678]"
```

## install local version with pip

```
pyenv local 3.8.13
```

### references

- Docs for linux https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python
- Debian https://www.debian.org/doc/manuals/maint-guide/first.en.html
- pyenv https://realpython.com/intro-to-pyenv/
