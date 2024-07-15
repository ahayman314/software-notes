When dealing with [[Python Flask]] requests, we have some useful tools we can use. First, check out [[Flask Key Concepts & Objects]] that explains blueprints and the request context. 

### Methods
- `jsonfiy()`: create a JSON response
```python
test = {'a': 124, 'name': 'bee'}
return make_response(jsonify(test), 200)
```

- `url_for(endpoint, **args)`: create a URL for a given endpoint function and pass in arguments
```python
@app.route('/user/<username>')
def index(username):
	return f"Hello {username}"

@app.route('/show_url')
def show_url():
	return url_for('index', username = 'John Doe')
```

- `make_response()` to create a response that's more customized with headers and cookies for example
```python
response = make_response("hello", 201)
response.headers['X-Custom-Header'] = 'Custom Value'
response.set_cookie('custom_cookie', 'value')
return response
```