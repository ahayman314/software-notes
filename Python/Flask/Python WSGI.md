In Python frameworks like [[Python Flask]], WSGI (Web Server Gateway Interface) defines a standardized interface between web servers and applications/frameworks - **just a contract definition** for separation of concerns.
It is really a common protocol, so things like [[Python Flask]] or Django can talk to [[Python Gunicorn]] or uWSGI. 

When a request comes in, the web server (i.e. Gunicorn) handles the incoming HTTP request and passes it to the Python application (i.e. Flask)

The Python application implements a **WSGI application** and is a callable that takes an environment dictionary and start response function. The application generates a response that is returned to the web server through that start response function. This includes the status code, headers, and body.

A web server typically spawns workers which each load the web application when spawned up. Some things like session data storage is still shared.