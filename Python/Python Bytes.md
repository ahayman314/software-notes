In [[Python]], I often get confused about their bytes implementation. 
In Python, bytes are **immutable sequences of bytes**. 
You can turn data into bytes using the `bytes(val)` operator. 

There are two main byte types:
- `bytes`: immutable sequence of bytes
	- `my_bytes = bytes(val)` - byte conversion
	- `my_bytes = b"This is a bytes literal"` - literals
- `bytearray`: mutable array of bytes with operations like push, pop, insert, append, etc. 
	- `my_byte_arr = bytearray(my_bytes)`
### General To/From Strings
In general, we need to specify the **encoding mechanism/type**. We can do this with `bytes()` but `encode()` and `decode()` are preferred:
- Encoding: `byte_data = bytes(source, encoding, errors)`
- Encoding: `byte_data = source.encode(encoding, errors)`
- Decoding: `output = byte_data.decode(encoding, errors)`

- Encoding: `utf8` is the default, but we can manually specify it as well: 
	- `byte_string = bytes(source="cool string", encoding='utf-8')`
	- `byte_string = byte("cool string")`
	- `byte_string = "my string".encode(encoding='utf-8')`
- Decoding: `utf-8` is also the default, but it's good to manually specify:
	- `my_string = byte_string.decode(encoding='utf-8')`
### Files
Use `wb` mode to write and `rb` mode to read
- Write: `binary_file.write("blahblah".encode('utf-8'))` - must specify encoding type for strings
- Read: `data = binary_file.read()`

For binary files, there are some useful methods:
- Get size: `size = os.path.getsize('my_file.txt')`
- Get to a specific point: `binary_file.seek(0, 0)`
- Read a certain number of bytes: `binary_file.read(2)`
### Generate Random Bytes
```python
import secrets

num_bytes = 16  # Change this to the desired number of bytes
random_bytes = secrets.token_bytes(num_bytes)
```