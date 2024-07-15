#### Joining Strings
Good: use `join` method with no space in-between
``` python
result = ''.join(strings)
```
Bad: loop over the stirngs
``` Python
result = ""
for s in strings:
	result +=s;
```

#### Truth
Good: don't check if true
``` Python
if x:
	do(y)
```
Bad: 
``` Python
if x == True:
	do(y)
```

#### Truth List
Good: don't check length or empty list
``` Python
if items:
	do(x)
```
Bad:
``` Python
if len(items) != 0:
	do(x)

if items != []:
	do(x)
```

#### Imports
Good: Only import the things you need and make naming clear
``` Python
import module
import module as mod
from module import name
```
Bad: import everything
``` Python
from module import *
```

#### Looping over Dictionary Keys
Good: treat dictionary keys like a list
``` Python
for key in d:
	print(key)
if key in d:
	do(x)
```
Bad:
``` Python
for key in d.keys(): 
	print(key)
if d.has_key(key):
	do(x)
```

#### Open and Close Operations
Good: use contexts to avoid descriptor leaks
``` Python
with open('data.txt', 'w') as f: 
	f.write('data')
```
Bad:
``` Python
f = open('data.txt', 'w')
f.write('data')
f.close()
```

#### Enumerating
Good: use enumeration when you need index and items
Good: 
``` Python
for (i, item) in enumerate(items): 
	print(i, item)
```
Bad: 
``` Python
for i in range(len(items)): 
	print(i, items[i])
```

### None
Good:
```python
if val is not None:
	do_stuff()

if val is None:
	do_stuff()
```

Bad:
```python
if val != None:
	do_stuff()

if val == None:
	do_stuff()
```