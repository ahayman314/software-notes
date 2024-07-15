[[Python]] exceptions are used like most languages.
- Do not catch a general `Exception`

There are some useful Python exceptions you can re-use:

| Exception        | Description                                                             |
| ---------------- | ----------------------------------------------------------------------- |
| `AttributeError` | Call method on invalid attribute.                                       |
| `SyntaxError`    | Parser encounters syntax error like no colon.                           |
| `TypeError`      | Using an operator on invalid type.                                      |
| `NameError`      | Name doesn't exist, often from typos, missing imports, or scope issues. |
| `IOError`        | For opening file that doesn't exist.                                    |
| `KeyError`       | Key doesn't exist in dictionary.                                        |
| `ValueError`     | Right type, but invalid value.                                          | 

### Custom Exceptions
Typically, you will create your own custom exception deriving from the base Exception class:
```python
class EncryptException(Exception):
	"""Encryption exception"""
```

### Best Practices
- Never catch general `Exception`
- Never use exceptions for flow control. If you want to find a substring, do not throw exception if not found - return None. 
- Do not break encapsulation - handle client library errors and pass up more specific errors when appropriate. Re-raise exception in try-except blocks with more appropriate type.
- Log exceptions when doing try-except blocks.

### Changing Exceptions
```python
try:
	do_func()
except SomeException as e:
	# A - new error message, keep chain
	raise OtherException("bad stuff") from e

	# B - same message, keep chain
	raise OtherException(e) from e

	# C - empty message, keep chain
	raise OtherException from e

	# D - new error message, scrap chain
	raise OtherException("bad stuff")

	# E - same message, scrap chain
	raise OtherException(e)

	# F - simply re-raise
	raise
```

