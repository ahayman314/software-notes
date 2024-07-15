In [[Python]], Flask is a popular & simple web application framework that works well for basic backend services. 

- [[Flask Configuration]]
- [[Flask Logging]]
- [[Flask Key Concepts & Objects]]
- [[Flask Requests]]

- Awesome resource: https://exploreflask.com/en/latest/preface.html

### Directory Structure
- `/app`: contains the app
	- `/my-flask-app`
		- `/__init__.py`: contains `create_app()` method with:
			- App definition
			- App configuration
			- Blueprint registration
			- Before/after request actions
			- App error handlers
		- `/wsgi.py`: creates app from `create_app()` and calls `app.run()` in main
		- `/utils.py`: typically contains some utility methods
		- `/service_a`
	- `/config.py`: contains Config() class and `configure_app(app)` method

There are two ways of splitting: by function or by division:
- Group views, static, templates by purpose like admin, home, service A, etc.
- Group all files by views, static, templates, etc. 

More scalable setup:
- `/app`: contains the app
	- `/config.py`: contains Config() class and `configure_app(app)` method
	- `wsgi.py` or `run.py`: create app from `create_app()` and call `app.run()` in main
	- `instance/config.py`: additional config?
	- `/utils`: directory to store common utilities
	- `/my-flask-app` - can have **multiple sibling applications**
		- `/__init__.py`: contains `create_app()` method with:
			- App definition
			- App configuration
			- Blueprint registration
			- Before/after request actions
			- App error handlers
		- **Below: group by function**
		- `/static`: directory for static files like `.css` 
			- `style.css`
		- `/templates`: directory for `.html` templates
			- `layout.html`
			- `index.html`
		- `/routes`: directory for routes
			- `some_api.py`
		- `/views`: directory for views
			- `some_view.py`
		- **Below: group by division**
		- `/service_a`
			- `__int__.py`
			- `models.py`
			- `views.py`
			- `templates/`
			- `static/`

###### `wsgi.py` File - Create & Run
Use `wsgi.py` instead of `app.py` when using a WSGI server in production (like Gunicorn or uWSGI). 
Usually pretty basic:
```python
from app.my-flask-app import create_app
app = create_app()

if __name == '__main__':
	app.run()
```

###### `__init__.py` File - App Factory
- Complete config and blueprint registration in `create_app`
```python
from app.config import configure_app
from flask import Flask, current_app, g

def create_app():
	app = Flask(__name__)
	configure_app(app)

	from . import service_a, service_b

	app.register_blueprint(service_a.bp)
	app.register_blueprint(service_b.bp)

	@app.before_request
	def create_session():
		g.my_session_obj = Dummy()

	@app.after_request
	def do_stuff():
		g.my_session_obj.some_method()

	@app.errorhandler(KeyError)
	def handle_key_error(exception):
		current_app.logger.exception(exception)
		return 'Some Error', 405

	return app

def configure_app(app):
	app.config.from_object(Config)
```

###### `config.py` - App Configuration
```python
import os

class Config():
	SOME_ENV_VAR = os.environ.get('SOME_ENV_VAR', 'my-default-val')
```
###### `service_a` File - API
```python
from flask import Blueprint, g, jsonify, make_response

bp = Blueprint('user', __name__)

@bp.route('/api/do-stuff', methods=['POST'])
def do_stuff():
	some_dict = {"a": "B"}
	return make_response(jsonify(some_dict), 200);
```

###### Running the App
- `flask run` from app directory
- `gunicorn --statsd-host localhost:5000 --statsd-prefix some.app --bind 0.0.0.0:5000 -w 4 app.my_app.wsgi:app`
- For a specific sub-directory, use `export FLASK_APP=app.wsgi`
- Or, run `flask --app app.wsgi run` to specify the module unless:
	- `wsgi.py` or `app.py` is defined in the current directory