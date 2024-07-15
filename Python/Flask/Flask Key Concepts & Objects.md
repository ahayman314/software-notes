In [[Python Flask]], we have some key concepts:

### Application Context
**This is about the APPLICATION from start to end of request**: stored data on server, etc. 
This is for representing the overall Flask application, including **application-wide** resources and configurations that persist throughout the lifetime of the request. The application context is **created before a request is handled and destroyed after each request has been processed**

- `with my_app_b.app_context()` should be used with a context manager to create an application context which lets you access application features when you are working outside of requests
- `current_app` is a proxy to the current Flask application context. **This is about accessing app-related stuff like config, settings, and extensions**
- `g` object is for temporary storage space to store data relating to the current request. **This is valuable for sharing data between functions within a single request.** Again, only available within a **single request.** Data gets cleared when request is complete.
- `@app.before_app_request` gets executed before each request within the entire application context
- `@app.after_app_request` is the same, but after request
- `@app.teardown_appcontext()` in `create_app()` does stuff before closing the app context

This is most relevant when you are running **multiple flask applications** - that is why we call it the **current app**. For the custom context, you also specify which app it comes from.
### Request Context
**This is about the REQUEST from the start to end of request**: request data, response data, etc. 
This is for keeping track of stuff like data **at the request scope**. When a request is being processed, you have a request context. 
- `@my_blueprint.before_request` decorator in `create_app()` does stuff before request on particular blueprint
- `@my_blueprint.after_request` decorator in `create_app()` does stuff after the request on particular blueprint
- `request` stores the request object, including headers, form data and URL parameters
	- `request.method`: method like `GET`
	- `request.url`: full URL of request
	- `request.path`: path of URL
	- `request.headers`: HTTP headers
	- `request.args`: query parameters from URL
	- `request.form`: form data from POST request
	- `request.files`: files sent with request
	- `request.cookies`: dictionary of cookies sent with request
	- `request.data`: raw data of request body
	- `request.json`: JSON object if content type if `application/json`
	- `request.get_json()`: proper way to retrieve the JSON data
- `session` lets you manage user sessions and store session-specific data **between requests**. This relies on **Cookies**
	- `session.get('key', 'default')`
	- `session['key'] = 'val'`
	- `session.pop('key', None)`
	- `session.clear()` to clear all data from session
- `app.permanent_session_lifetime = timedelta(days=7)`: example of persisting sessions longer

### Sessions & Cookies
Session data gets **persisted across multiple requests**. By default, Flask sessions expire when the user's browser is closed. Sessions **typically use cookies for their implementation**. When a user is on the site, the server assigns the user an ID which is stored as a cookie - **This is the only cookie Flask provides**. The server can then associate the ID with user-specific data. 

- **Session data is stored on the Flask servers** associated with an ID
- The ID is sent to the user
- On subsequent requests, the user sends the ID which then retrieves the session data and initializes the session object

Cookies are **small bits of data sent to the user by the web server**. When a user makes a subsequent request, the cookies are sent with the request - this lets the server maintain information about the user. Examples include user preferences, tokens, shopping carts, etc. 

The server includes a header called `Set-Cookie` along with the data to be stored in the cookie. Browsers store this data, and send it back to the server on subsequent requests. Each cookie can have attributes like expiration, domain scope, path, etc. 
### Blueprints
Blueprint objects **are a collection of routes**. These blueprint objects can be **registered, or tied** to an app in `create_app()` method. This is helpful for organizing routes. They define **sets of operations that can be registered**. 

You cannot unregister a blueprint without destroying the entire app.

App Creation File - Register blueprints:
```python
def create_app(): 
	from . import service_a

	app.register_blueprint(service_a.bp)
```

Service File - Define blueprints:
```python
from flask import Blueprint

bp = Blueprint('user', __name__)

@bp.route('/api/stuff', methods=['POST'])
def do_stuff():
	pass
```

The blueprint arguments don't matter too much:
- `user`: the name of the blueprint, occasionally used for things like template paths
- `__name__`: the name of the module so Flask can correctly find the root path
### Views & Models
Views and models are used in the [[Model-View-Controller (MVC)]] design pattern. 

A view is about **presenting data to the user and handling user interaction**. In practice, views are typically route functions that receive requests from users and return a response. 

A model is about the **application data and business logic**. It manages/stores data, and performs operations on the data. Models abstract away data storage and retrieval. For web apps, **models often represent database tables** and encapsulate logic for querying, creating, updating, and deleting data from the database. For python, you will often use something like [[SQLAlchemy]] for the model. 

### Routes
**A route is a URL pattern that MAPS to a specific view function**

Routes are URL patterns that define how your web application can be accessed by associating URLs with functions. Routes map to view functions that determine what happens when the route is accessed. 

**A view** is the **function that gets run when a route is accessed**. 