# pyenv installation

## steps

Install pyenv first with pyenv-virtualenv plugin

Install the correct version of python

```
pyenv install <version>
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
