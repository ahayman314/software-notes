For unit testing [[Python Flask]] apps with [[Python Unit Testing]], there are a few pointers. 

As usual with [[Python Unit Testing]], create a `test` directory with `__init__.py`, `conftest.py` and your test files starting with `test_`. 

The key part is to return a **test client app**. First, you create the app, but instead of **running the app with `app.run()`**, you instead create a **test client with `app.test_client()`**

```python
import pytest
from myapp import create_app

@pytest.fixture
def app(): 
	# You don't really need this - test test_client() sets this to true anyways
	app = create_app({'TESTING': True})
	yield app

@pytest.fixture
def client(app):
	return app.test_client()
```

You can then send requests like so: 
```python
response = client.post('/endpoint', json = my_uri_dict, header = my_header)
```