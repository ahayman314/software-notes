For logging in [[Python Flask]], there are two main steps:
- Set logger settings
- Use logger
Flask uses standard [[Python Logging]]. Messages are logged with `app.logger`. 
### Setting up Logger
In [[Flask Configuration]], set environment variables like `LOGGING_LEVEL` and initialize the logger in the `configure_app()` method.
### Using Logger
Example - `app.logger.info('%s failed to log in', user.username)`

- `app.logger` - globally available logger for Flask application
- `current_app.logger` - used within context of request
In general, **use `app.logger` - you probably don't need `current_app`**

### Dictionary
Flask recommends using `logging.config.dictConfig()` to overwrite the default configurations. This should go before the app definition. 
```python
from logging.config import dictConfig
dictConfig({"config..."})
app = Flask(__name__)
```
### Handlers
You can set up multiple **log handlers**. Log handlers control **where logs are sent and how they are formatted**. This again uses [[Python Logging]]. After configuring the handler, you can add it to the `app.logger` object. 

**By default, Flask adds a `StreamHandler` to `app.logger` automatically**, but you can remove it if you want. 

```python
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)

app.logger.addHandler(file_handler)

app.logger.removeHandler(some_handler)
```