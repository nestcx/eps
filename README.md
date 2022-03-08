This project demonstrates the minimum configuration required in order to build and install a python package (or distribution rather) in editable/development mode.

eps = editable package skeleton

```
eps/
	game/
        __init__.py
        api.py
    examples/
        __init__.py
        example1.py
```

How to import the game api into the example modules?

Do NOT edit the sys.path variable.



Solution:  create a distribution and install it locally on your system in development/editable mode.

Three new files are required:

    eps/
    
    +   setup.cfg
    +   setup.py
    +   pyproject.toml
    
        game/
            __init__.py
            api.py
        examples/
            __init__.py
            example1.py


In the future, only `pyproject.toml` should be required, however at present all three are required to satisfy the current status quo.

These files define the configuration of your project so it can be built into a distribution with a build system.

Notably, the configuration will list the packages that exist in the project (game/examples) 

The default build system is called `setuptools` and usually comes installed with Python, however there are others out there now too like `poetry`.

- for example in a new venv, pip list will show setuptools is the only package installed.



How did these three files come about?

`setup.py` came first, and was replaced in favour of `setup.cfg`.

The issue with `setup.py` and `setup.cfg` is that they are too dependent on `setuptools` being the assumed default build tool, so pep 518 introduced `pyproject.toml` as the build-agnostic replacement.



Despite meaning to be replacements for each other, each of these files is still required to be present in a project because setuptools and pip haven't caught up to the specification:

1.  always need `pyproject.toml` as specified by **pep 517**, to specify, at a minimum, your build system (setuptools)
2.  still need `setup.cfg` because as of today **setuptools** does not support writing its configuration in `pyproject.toml`.
3.  still need `setup.py` for **pip** to create an editable installation.
   - after pip 21.1, only `setup.cfg` will be required for editable installs.


Therefore, when using `setuptools` as a build tool, most of the configuration will go in `setup.cfg` while `pyproject.toml` and `setup.py` will essentially exist just to fulfill requirements for pep 517 and pip, respectively.



`pyproject.toml` - specify `setuptools` as the build system

```
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```



create a 'dummy' `setup.py` file:

```Python
import setuptools
setuptools.setup()
```



`setup.cfg`

- at a minimum it needs to specify your project name / version, and importantly, the packages in the project.

```
[metadata]
name = eps
version = 0.0.1

[options]
packages = game, examples
```

alternatively we can find all the packages in the project with:

```
[options]
packages = find:
```







Now we can build the distribution in editable mode 

```
pip install -e .
```



And we can import the api into the example modules 

```
from game import api

api.func()
```



output of pip freeze:

```
# Editable Git install with no remote (eps==0.0.1)
-e /eps
```







sources:

https://stackoverflow.com/a/64151860
https://stackoverflow.com/a/68939858



https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/#working-in-development-mode
https://packaging.python.org/en/latest/tutorials/packaging-projects/



https://python-packaging-tutorial.readthedocs.io/en/latest/setup_py.html



https://setuptools.pypa.io/en/latest/userguide/quickstart.html
https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#package-discovery
https://setuptools.pypa.io/en/latest/userguide/declarative_config.html



https://github.com/pypa/sampleproject/blob/main/pyproject.toml



https://www.python.org/dev/peps/pep-0621/
https://www.python.org/dev/peps/pep-0518/