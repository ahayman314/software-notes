Gunicorn is a production-ready [[Python WSGI]] HTTP server for [[Python]] applications like [[Python Flask]]. 
You install it with `pip install gunicorn`. 

The big idea is that Flask doesn't take care of production stuff - it is more of a framework. Things like Gunicorn then take Flask apps and run them properly. It is really just a separation of concerns thing. 

`gunicorn <args> <app_name>`

Example:

`gunicorn --bind 0.0.0.0:8000 app:app`
- `app.my_flask_app.wsgi:app`. First part is the module, second is the app object in the module that Gunicorn uses
- `--timeout 30`: specify a timeout
- `--log-level warn`: specify a log level
- `--error-logfile -`: hyphen means standard output
- `--access-logfile -`: hyphen means standard output
- `-w 5`: number of workers