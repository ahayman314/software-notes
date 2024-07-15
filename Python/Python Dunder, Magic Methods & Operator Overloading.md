These are [[Python]] methods with `__method__` double underscore that have special purposes.

This is really just like [[C++ Overloading]] of methods. **ALL methods** have some operators you can overload and define just like in C++. This gives a nice unified set of operations that can be used across objects. 

| Method                          | Invocation                                                             |
| ------------------------------- | ---------------------------------------------------------------------- |
| `__init__(self,...)`            | Constructor method invoked with object is created like `a = MyClass()` |
| `__str__(self)`                 | Called by `str()` operator and `print()` operator                      |
| `__len__(self)`                 | Called by `len()` operator                                             |
| `__getitem(self, key)`          | Called by indexed getting like `obj[key]`                              |
| `__setitem__(self, key, value)` | Called by indexed setting like `obj[key] = val`                        |
| `__iter__(self)`                | Called when iterator object is requested                               |
| `__next__(self)`                | Called by an iterator to get the next value                            |
| `__eq__(self, other)`           | Called by `==` operator                                                |
| `__lt__(left, other)`           | Called by `<` operator                                                 |
| `__gt__(left, other)`           | Called by `>` operator                                                 |
| `__add__(self, other)`          | Called by `+` operator                                                 |
| `__call__(self, ...)`           | Called by `()` operator to create **functors**                         |
| `__hash__(self)`                | Called by hash-based collections                                                                       |

### Name
`__name__` is a special attribute **set for every module**. It provides the name of the current module as a string.
###### Main
When a script is executed as the main program, `__name__` is set to `__main__`.  This can be used to run a method when the script is run - otherwise, it can just be imported:
```python
if __name__ == "__main__":
	run_stuff()
```
###### Modules
Otherwise, the `__name__` is set to the module name which is again **the name of the Python script without the `.py`**. 

### All
The `__all__` attribute is a list that can be defined within a module to specify which symbols/names should be considered public when the module is imported using `from module import *` strategy **AND ONLY WITH a STAR** - really, this isn't helpful. 
This is helpful when you want to keep some methods or things private, and only expose a specific public-facing API. 

```python
# my_module.py
def _private_function():
	pass

def public_function():
	pass

__all__ = ['public_function']
```