In [[Python]], we use the concepts of modules and packages for defining units. 
### Modules & Packages
###### Modules
A module is **a single file with Python code**. It is self containing and contains some code. 
###### Package
A package is **a collection of Python modules (i.e. files) organized in a directory hierarchy**.
A package can have **sub-packages** in subdirectories. 

Python **only** considers a directory to be a package when you have an `__init__.py`. When searching for package resolution with imports like `from numpy.custom import stuff`, it:
1. Looks at built-in libraries with `numpy.custom` package
2. Check current directory for `numpy.custom` package
3. Check 3rd party library package locations
4. Checks sub-directories from the current directory
5. Checks the `PYTHONPATH`
Basic question: **is it in the current directory somewhere, is it built-in or is it installed?**
### Importing
There are really two scenarios for importing:
- Are you a package that will be installed or consumed?
	- Can use absolute OR relative imports
- Are you a package that will be executed or run? 
	- Can only use absolute imports
### Absolute Imports
Both packages that will be installed / consumed and ones that will be executed / run can use absolute imports. This follows the rules above. Ask to import **any package**, and it will search through the paths and try to resolve it. Then, you can import stuff from the package:
- `from pkg import mod`
- `from pkg.sub_pkg import mod`
- `from pkg.mod import func`
- `from pkg import func` - **this works** because of `__init__.py` if they have funcs in there or imported there
	- **This is a KEY pattern for public packages**: expose everything you want through the `__init__.py` file. Fine for the normal path to be accessible, but it's much cleaner
- `from mod import func` - **often fails** - **you must look for packages** (unless you are running from that current directory in which case it will find it)

Basically, everything stems from some path after a package. It can be `pkg.subpkg.subpkg.mod`, `pkg.func`, `pkg.mod.func`, etc. 

General syntaxes:
- `import pkg` - import package
- `from pkg import sub_pkg, mod, func` - import specific stuff
- `import pkg as new_name` - change name
Never do `from pkg import *`. **Always use `from pkg import x`** to specify exactly what you need and no more.
### Relative Imports
**Only use relative imports within a package you are developing. Relative imports DO NOT make sense in the context of executables.** Why? Because when you normally import a package, the relative imports get resolved relative to the root package. Now with executables, there is no sense of a project root so we must be explicit.
- `pkg/mod1`: `from .mod2 import func`
	- Caller: `from pkg.mod1 import x`
	- Module: converts `.mod2` to `pkg.mod2`
- `pkg/mod1`: `from . import mod2`
	- Caller: `from pkg.mod1 import x`
	- Module: converts `.` to `pkg`
**In other words, relative imports within packages get resolved as needed**
### Styling of Imports
1. Imports are at top of file after module comments and docstrings
2. Divide into three groups separated by a blank space
	1. Standard library imports (built-in)
	2. Related third party imports
	3. Local application imports
3. Split imports alphabetically
### Executable Tips
- Import full packages only, no relative paths
- Include parent folder in `PYTHONPATH`
- Append folder at runtime to `sys.path`
- Run app from root of project to keep it easy
- Keep a flat hierarchy if sensible

### Package Tips
- Use relative or absolute imports.
- Sibling modules:
	- `from .mod2 import x` <- **most common for simple libraries**
	- `from pkg.mod2 import x` <- **most common for large libraries**
- Sibling packages:
	- `from ..subpkg.mod2 import x`
	- `from pkg.subpkg.mod2 import x`
- Have a **single package directory** with **a single `__init__.py`** to load in the exposed functions. Keep it flat and keep it simple
- For more advanced libraries, use absolute imports. It adds more clarity on what you're importing, ensures consistency and is generally more explicit
- Example: `requests` uses a single package directory with relative imports. `numpy` uses multiple directories with absolute imports. 

### Testing
For testing packages, it is straightforward:
For both packages and executables, add path in `pyproject.toml` to the package directory. Then, you can import the package like any other 3rd party package. 