In [[Python]], we often want to package up our packages into something we can distribute for installation over pip for example. 
Today, use `pyproject.toml`, and probably `poetry` as well. 
### `setuptools` - Build System
`setuptools` is a Python library that helps with packaging, distributing and installation of Python packages. 
This relies on a `setup.py` script that contains metadata and things like dependencies, package directories, and entrypoints. 
In modern Python, use [[PyProject Toml Files]] instead with the `setuptools` as the build system. 

```python
from setuptools import setup

setup(
	  name='my_package',
	  version='1.0.0',
	  author='Name',
	  description='stuff',
	  packages=['my_module'],
	  install_requires=['dependency'],
)
```

You can then run `python setup.py sdist bdist_wheel`
- Creates a source distribution `sdist`
- Creates a binary wheel distribution `bdist_wheel`
- These both get stored in the `dist` 
### `.tar.gz` or `.zip` - Source Distribution Format
This is the old way of doing packaging - you packaging all of your **source files** into an archive. The archive will also contain some metadata and other files. Today, it is preferred to use `wheel` package format.
Typically `sdist` name. 
### `wheel` - Binary Wheel Distribution Format
`wheel` is a binary package built on top of `setuptools` to improve package installations. 
**Wheel packages are DIFFERENT from source packages and are meant to be better and faster.**
Wheel packages have a `.whl` extension. 
**Wheels are just a package format.**
### Building Process
1. Detect backend like `setuptools`
2. Check dependencies
3. Compile any source if needed like `Cython` or `Rust`
4. Create source distributions and wheel distributions
5. Upload to wherever you want
### `pyproject.toml` - Overall Project Management
This is the new standard for configuring [[Python]] projects defined in  `pyproject.toml` files and requires 3.8 or higher.
###### Tool Configuration: 
This is to configure build tools like for linting
```toml
[tool.flake8]
max-line-length = 88

[tool.pylint]
max-line-length = 100
disable = ["missing-docstring", "invalid-name"]

[tool.black]
... 
```

###### Build system
This is to configure how project is built and specify dependencies. This is for **packaging applications**. The main three build backends are:
- `setuptools`
- `flit`
- `poetry`
```toml
[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"
```
###### Metadata 
Specify metadata about the project. This **includes dependencies**. 
```toml
[project]
name = "MyPackage"
version = "0.0.1"
description = "My super cool package"
readme = "README.md"
requires-python = ">=3.8"
classifiers = [
	"Programming Language :: Python :: 3",
	"Operating System :: OS Independent",
]
dependencies = [
	"flask = 5.13",
	"mypackage" = "6.14",
]
```
###### What to Include
You may not want to include your test files.
```toml
[project.package]
include = ["src/*"]
```
###### Packaging
You can simply run the following to get `dist/*` package ready to go.
```bash
python -m build
```
### Poetry
Poetry is a popular dependency management and packaging tool for Python used to simplify and improve the workflows. 
Install it with `pip install poetry` (not recommended) or `curl -sSL https://install.python-poetry.org | python3 -`
- Manage all dependencies from `pyproject.toml`, and generate `poetry.lock` files which is awesome
- Separate out dev dependencies which won't be included in publish and won't be installed by default
- Handle creation of source and wheel distributions which simplifies packaging
- Define and run project scripts
- Deal with virtual environments. Now, it creates venvs in your user directory, excluding it from source control
	- In general, you now **only run poetry commands** - it will run from your venv automatically. You no longer need to activate your venv unless you want to run commands without poetry. 

With poetry, we can scrap `twine`, `venv` and `setuptools`. 
###### Commands
| Command                                                     | Description                                                                           |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `poetry init`                                               | Initialize new project from existing project                                          |
| `poetry add dep-name`                                       | Add a new dependency like `flask` - auto-adds it to the `pyproject.toml` dependencies |
| `poetry add --dev dep-name`                                 | Add new dep under `tool.poetry.dev-dependencies`                                      |
| `poetry update dep-name`                                    | Update a dependency                                                                   |
| `poetry build`                                              | Build a distribution package, including `sdist` and `bdist_wheel`                     |
| `poetry install` (--dev)                                    | Install project dependencies. Can also do dev dependencies                            |
| `poetry run script-name`                                    | Run script in `tool.poetry.scripts`                                                   |
| `poetry publish`                                            | Publish package to PyPI                                                               |
| `poetry run python your-script.py`                          | Run a script                                                                          |
| `poetry run pytest`                                         | Run pytest                                                                            |
| `poetry new my-project`                                     | Create a new project with initial setup                                               |
| `poetry shell`                                              | Actual venv                                                                           |
| `exit`                                                      | Exit shell & deactivate                                                               |
| `poetry env use python3.10`                                 | Use a specific Python version                                                         |
| `poetry remove dep-name`                                    | Remove a dependency                                                                   |
| `poetry env info`                                           | Get info about venv                                                                   |
| `poetry source add my-repo https://pypi.example.org/simple` | Add a new repo                                                                        |
|                                                             |                                                                                       |
### `twine` - Upload
Twine is the go-to way of **securely uploading distribution packages** after they have been built - **this is its only purpose**. It is a python package, so you must install it with `pip install twine`. 

After building your package, you can:
```bash
twine upload dist/*
```

Some helpful command line flags:
- `--repository-url <my-url>`: specify URL to repo
- `--username <my-user>`: specify username
- `--password <my-pass>`: specify password 