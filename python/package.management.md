# package management

## installing packages

- Packages used to be installed via the standard Python distutils mode (python setup.py install). This is no longer the case as of python 3.12
- Many packages can also be installed via the setuptools extension or pip wrapper, see https://pip.pypa.io/.

The entire distutils package is deprecated, to be removed in Python 3.12. Its functionality for specifying package builds has already been completely replaced by third-party packages setuptools and packaging, and most other commonly used APIs are available elsewhere in the standard library (such as platform, shutil, subprocess or sysconfig). There are no plans to migrate any other functionality from distutils, and applications that are using other functions should plan to make private copies of the code. Refer to PEP 632 for discussion.

## References

- https://docs.python.org/3/library/distutils.html?highlight=setuptools
- https://packaging.python.org/en/latest/tutorials/installing-packages/