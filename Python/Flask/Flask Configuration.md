In [[Python Flask]], you typically want to pass in environment variables to configure your application. 

The **`app.config` stores the configuration of the Flask application**. Some special variables can be set without the dictionary, but just use the dictionary or ideally, config objects
```python
# Top - load from a module or class in a module with class variables (see below)
app.config.from_object('app.config.ProdConfig')

# OR
from app.config import Config
app.config.from_object(Config)

# Good
app.config.from_envvar('ENV_VAR_WITH_PATH_TO_CFG')
app.config.from_file("config.json", load=json.load)

# Okay
app.config['TESTING'] = True
app.config.update(TESTING=True, SECRET_KEY='a8f9ad9')

# Bad
app.testing = True
```

Best practices:
- Keep the default version in version control in `app/config.py`
- Use environment variables to switch between configuration versions if needed
### Standard Setup
There are lots of features for doing this, but in general, do this: 

Create `/app/config.py` that **defines various configurations:**
```python
import logging
import os
import sys

# Can also do a constructor if you need more customization
# Example: reading Redis unix socket vs. http with if-else
class Config():
	SOME_ENV_VAR = os.environ.get('SOME_ENV_VAR', 'my-default-val')
	ANOTHER_VAR = 841

def configure_app(app):
	app.config.from_object(Config)
```

Now, in `/app/my_app/__init__.py`, you can **call the configure to inject the config and logger into the app**:
```python
from app.config import configure_app

def create_app():
	app = Flask(__name__)
	configure_app(app)
```

For dev, staging, prod, you have two options:
- Custom config classes for each, inherited from base `Config()` class, and selected through env vars
- Just load specific env vars into the main `Config()` class, where the env vars are tailored to the environment 
### `instance/` Directory
The `app = Flask(__name__, instance_relative_config=True)` means to look for configuration relative to project root directory. It lets you access files as `app.config.from_pyfile('config.py')` for example.

```python
# Look for `instance/ directory against base`
app = Flask(__name__, instance_relative_config=True)

# You can still load regular config files
app.config.from_object('config')

# But now, you can also access the config files in instance/ with `from_pyfile`
app.config.from_pyfile('config.py')
```

**This is valuable for configs you don't want to check into source control.**

This is generally unnecessary - just load environment variables into `.config.py` at run-time
### Secrets
You should load secrets from the environment variables, but ideally integrate with a 3rd party secret manager like HashiCorp Vault. You still need an API token and username, but that's okay - a malicious user can no longer modify variables, and specific permissions can be set on the user/token. All advantages:
- Fine grained token policies
- Secret rotation and 
- Short lived tokens
- Encryption at rest
- Loading at runtime without using `config` object for the occasional authentication

This really means that instead of storing your secrets with something like SOPS in Kubernetes files in source control, we can bypass that entirely, and only provide APIs tokens to the secret manager. This is more scalable an secure. 