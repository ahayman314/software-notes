In [[Python]], when you're import modules and packages, you need to specify where to import from.

###### `sys.path`
The `sys.path` is a list of paths Python uses to search for modules and packages. It is initialized from several sources by default:
- Current directory containing the script itself
- Standard library locations
- Anything extra from `PYTHONPATH`
When you specify a path like `from app import blah` or `import app`, it looks in these locations.

A common technique of adding a path to different modules is:
```python
import sys

if "/path/to/dir" not in sys.path:
	sys.path.append("/path/to/dir")
```

###### `PYTHONPATH`
This is an environment variable you can set to add additional directories to Python's search path. 
