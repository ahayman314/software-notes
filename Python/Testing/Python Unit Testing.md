`pytest` & `unittest` are the go-to unit testing packages in [[Python]]. 
Some specialized stuff to know:
- [[Flask Unit Testing]]

Related:
- [[C++ GTest]] and [[C++ GMock]] - all of the same concepts
- [[Unit Testing]] - unit testing paradigms and approaches
### Directory Setup
- `test/
	- `conftest.py`: define fixtures accessible across multiple test files
	- `__init__.py`
	- `test_some_test_file.py`

### Imports
For files in `test/` that you want to import like `constants.py` to `test_stuff.py`, do `from .constants import PI`. `from constants import PI` doesn't work because it looks for `constants` in the top-level directory. If you did `from test.constants import PI`, it would work. 

Alternatively, you could add `test` to the `pythonpath` in the `pyproject.toml`. 
### Running Tests
Run `pytest` at the root directory
- `-s`: show output
- `-v`: verbose
- `-k "TestClass and not test_one"`: test only certain names
- `-W PYTHONWARNINGS`: show warnings
### Configuration Files
You can define configuration files:
- `pytest.ini`: top priority and specifies specific settings
- `pyproject.toml`: new and preferred way of specifying settings
If you define a [[Python Packaging & Distribution]], you can specify useful attributes like paths for testing. 
See https://docs.pytest.org/en/7.1.x/reference/reference.html in 'Configuration' for a full list of configuration values.

Some common ones:
```
[tool.pytest.ini_options]
addopts == --maxfail=2 -rf
minversion = "6.0"
pythonpath = ["app"]
testpaths = ["tests"]
junit_suite_name = my_suite
```

Now you can simply run `pytest` and it will pick up the settings from `pyproject.toml`
### Flow
1. Arrange - Set up fixtures - these are decorators on functions that will run before running. 
2. Act - run the functionality
3. Assert - check the resulting state
4. Clean up
### Test Fixtures
- In fixtures, we always **yield** for returning. For teardown, write stuff after yield. 
- You usually want to specify the **scope and name explicitly.**
- The scope sets when **the fixture is created & destroyed**. Two most common are:
	- `function`: Created/torn down for each test run. Default
	- `session`: Create/torn down once for the whole test suite
- You can set the fixture to be available by name in all tests instead of having to explicitly declare the fixture using `autouse` argument. You still need to include the fixture as an argument. Auto-use just means the fixture will be automatically run.

There are three common use cases for fixtures:
1. Setup **mocks in fixtures** and then use those mocks in the tests. 
2. Setup **fakes** in fixtures and use the fakes in the tests - also uses mocks
3. Prepare **dummy data or other objects** to be used

```python
import pytest
from unittest import mock

@pytest.fixture()
def fixture_my_module_mock():
	with mock.patch("my_custom_module.some_method") as m:
		yield m

@pytest.fixture(autouse=True)
def fixture_my_module_fake():
	with mock.patch("my_custom_module.some_method", module.Fake()) as m:
		yield m

@pytest.fixture(scope='function', name='dummy_data')
def fixture_dummy_data():
	yield "stuff"
```

Fixtures can be passed as arguments to other fixtures. A good example is flask:
```python
@pytest.fixture()
def app_fixture():
	app = create_app()
	yield app

@pytest.fixture
def app_client(app_fixture):
	yield app_fixture.test_client()
```
### Tests
Now, we can include fixtures as arguments, or access them automatically if `autouse` is true.
- Assertions let you assert something, and then print the following object if not passing
```python
def test_stuff(app_client, dummy_data):
	response = app_client.post('/endpoint', json={"data": dummy_data})
	assert response.json["stuff"] == "more stuff", response.json
```
###### Parameterized Tests
We can also do parameterized tests using `pytest.mark.parameterize`
```python
@pytest.mark.parameterize('size', [1, 2, 3])
def test_stuff(size):
	assert 2 * size == double_size(size)
```
###### Exception Tests
We can test exceptions get raised:
```python
def test_stuff():
	with pytest.raises(ExceptionExceptedToRaise):
		run_method()
```
### Mocks
Mocks allow us to assert calls on methods:
- Have you called the method? How many times?
- What did you call the method with? 
This is the same as [[C++ GMock]] concepts where mocks are all about **expectations**. See [[Unit Testing]] concepts to consider how to use mocking. 
**Instead of mocking, you can create dummies** if you want. 

###### Setting up Mocks
If you want a mock across several tests, **use a fixture**. Otherwise, decorators are pretty handy:
```python
@mock.patch('myapp.my_method')
def test_stuff(mock_my_method):
	...
```

Alternatively, you can use context managers:
```python
with mock.patch("my_custom_module.some_method") as m:
	yield m
```
###### Setting Stubbed Data
This is like an `ON_CALL` in [[C++ GMock]] - it returns canned data with no expectations
```python
def test_stuff(my_mock):
	my_mock.my_func.return_value = 3
	assert call_other_method() == 9
```
###### Setting Stubbed Exceptions
```python
def test_stuff(my_mock):
	my_mock.my_func.side_effect = SomeException()
	with pytest.raises(SomeException):
		call_other_method()
```
###### Setting Stubbed Data and Expectations (Mocks)
Now, let's add assertions - lots of options. This is like `EXPECT_CALL` in [[C++ GMock]]. 
```python
def test_stuff(my_mock):
	my_mock.my_func.side_effect = SomeException()
	call_other_method()
	my_mock.assert_called_once()
	my_mock.assert_called_once_with(arg=10)
	my_mock.assert_called()
	my_mock.assert_called_with(arg=10)
	assert my_mock.some_method.call_count == 2
	assert call(42) == my_mock.some_method.call_args_list
```