[[Python]] has a `logging` built-in module to help with logging.

A **logger** is a person that:
1. Receives log string
2. Sort by pre-defined levels
3. Use his own **handler** to process it. The handler is like a contractor that will do something with the log (format it according to formatter and send it somewhere)
4. Pass log to superior logger (who repeats until it is root who has no superior OR the logger's `propagate` flag is false)


### Levels
There are several levels you can set and access.
- `NOTSET`: 0
- `DEBUG`: 10
- `INFO`: 20
- `WARNING`: 30
- `ERROR`: 40
- `CRITICAL`: 50

```python
import logging

# Configure logging
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)

# Use logging
logger.warning('Uh oh') # also defined for debug, info, etc.
logger.log(logging.ERROR, 'Error') # can specify level here too
```
### Configuration
To configure logging, we typically add **a formatter and a handler to a root logger.**

For configuring the root logger:
- `logging.config.dictConfig({})`
- `logging.basicConfig(level=logging.INFO, ...)`
	- This is a shortcut to configure the `root` logger
	- By default, level is `WARNING` and handler is `StreamHandler` to `stderr`

Getting a logger:
- `logger = logging.getLogger()` returns the root logger
- `logger = logging.getLogger(__name__)` returns a logger under root based on module name

Formatters define how logs are formatted:
- `formatter = logging.Formatter('%(asctime)s:...)'`

Handlers define how logs are handled, and you can have multiple handlers:
- Create handler: `file_handler = logging.FileHandler('sample.log')`
- Attach formatter: `file_handler.setFormatter(formatter)`
- Attach handler to logger: `logger.addHandler(file_handler)`
### Logger Hierarchy and Custom Loggers
For your own package, you typically have your own logger subclass and 'register' it with the root logger. 
- Set null handler to avoid no handler warnings
- The `propagate` property in `logging.Logger` is set to true by default, so events logged are passed to the parent
In `__init__.py` of `my_package`: 
```python
import logging
from logging import NullHandler

class CustomLogger(logging.Logger):
	def __init__(self, name, level=logging.NOSET):
		super().__init__(name, level)

logging.getLogger(__name__).addHandler(NullHandler())
```

Now, in the consumer of the package, you can easily do: 
```python
import logging
import my_package

my_package_logger = logging.getLogger("my_package")
my_package_logger.setLevel(logging.INFO)
```