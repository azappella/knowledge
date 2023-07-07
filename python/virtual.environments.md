# virtual environments

Python “Virtual Environments” allow Python packages to be installed in an isolated location for a particular application, rather than being installed globally. If you are looking to safely install global command line tools, see Installing stand alone command line tools.

Currently, there are two common tools for creating Python virtual environments:

- venv is available by default in Python 3.3 and later, and installs pip and setuptools into created virtual environments in Python 3.4 and later.
- virtualenv needs to be installed separately, but supports Python 2.7+ and Python 3.3+, and pip, setuptools and wheel are always installed into created virtual environments by default (regardless of Python version).

### Basic usage for venv

```
python3 -m venv <DIR>
source <DIR>/bin/activate
```

### Basic usage for virtualenv
```
python3 -m virtualenv <DIR>
source <DIR>/bin/activate
```

## References

- https://packaging.python.org/en/latest/tutorials/installing-packages/