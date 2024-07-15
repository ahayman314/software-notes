In [[Python]], we have context manages. This is very similar to [[RAII]] in C++. 
We don't want to call `open()` and then `close()` afterwards because it can cause file descriptor leaks. 

Just like how we can use smart pointers or classes in C++ to manage this issue, we have context managers in Python that define the 'entering' and 'exiting' of the resource. This is only supported by certain classes:
- `__enter__()`: what to do initially on context setup
- `__exit__()`: what to do on context teardown

In this example, the open method creates a file class instance that is configured to run 'close' on destruction - in other words, the RAII is taken care of for us in certain classes by using context managers. 
```python
with open('hello.txt', mode='w') as file:
	file.write('hello world!')
```

In practice, there is a sequence of actions:
1. Call expression to get the context manager
2. Store enter & exit methods from context manager
3. Call enter method and bind output to target variable
4. Execute code block
5. Call exit method with code finishes or when exception is thrown