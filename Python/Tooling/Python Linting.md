In [[Python]], we usually use `pylint` and `black` for linting and auto-formatting. `flake8` is another one.

`pip install black` & `pip install pylint`

### PyProject
In your `pyproject.toml`, define the linting parameters, as shown in [[Python Packaging & Distribution]]

Example:
```toml
[tool.flake8]
max-line-length = 88

[tool.pylint]
max-line-length = 100
disable = ["missing-docstring", "invalid-name"]

[tool.black]
```

Then, simply run `python -m pylint` or `python -m black`