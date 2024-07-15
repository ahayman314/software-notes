Base64 is a binary to text encoding scheme that encodes binary data into a format that consists of ASCII characters. This is good for **representing binary data in a text-friendly format**. This is common when transmitting/storing binary data in contexts requiring text representations like JSON. 

It contains **only 64 characters** in the set:
- Lowercase/uppercase letters: 52
- Digits: 10
- '+' and '/' 

Each character is assigned a unique 6-bit sequence with has 64 combinations. If the input isn't a multiple of 6-bits, padding is added. 

Example: `01100001 01100010 01100011` -> `YWJj`. The resulting base64 is larger than the bytes of course. **The key here is that it encodes blocks of 6-bits - it can still store information from larger sets because it just cuts the values off**:
`011000 | 01 0110 | 0010 01 | 100011` -> `YWJj`
**It is just an encoding scheme.**

Python has the `base64` library to help with this. 
### Encoding/Decoding Bytes
The core of it is encoding & decoding [[Python Bytes]]: 
- Encoding: `encoded_bytes = base64.b64encode(b"some bytes")`
- Decoding: `decoded_bytes = base64.b64decode(encoded_bytes)`

For example: `abc` in bytes turns into `YWJj` from `011000 | 01 0110 | 0010 01 | 100011` 

Now, to get strings that you can move around, you want to turn the **encoded bytes** into strings. The common way is: 
- Encoding: `encoded_bytes = base64.b64encode(b"some bytes").decode('utf-8')`
- Decoding Short Way: `decoded_bytes = base64.b64decode(encoded_bytes)`
	- Decode knows how to handle a base64 string type
- Decoding Long Way: `decoded_bytes = base64.b64decode(encoded_bytes.encode('utf-8'))`
	- We could convert the string into encoded bytes first, but not needed
### Encoding/Decoding Strings
This builds on byte encoding for strings which is explained in [[Python Bytes]]. Really, we just need to encode the string into bytes and the start, and decode into string at the end.
- Encoding: `base64_str = base64.b64encode("my string".encode("utf-8")).decode('utf-8')`
- Decoding: `decode_str = base64.b64decode(base64_str).decode("utf-8")`

Really this is a wrapper process: string -> bytes -> base64 bytes -> string -> base64 bytes -> string

