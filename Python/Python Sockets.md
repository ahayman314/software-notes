In Python, you can do a lower-level sockets communication instead of [[Python Requests]]. This API maps to lower-level system calls. 

You can have a socket server that **listens** for requests and makes responses.
Or, you can have a socket client that **sends** requests to a server.

Data is received and sent at a **byte** level and must be encoded/decoded as needed.

Remember:
- TCP is about a sustained connection, doing a handshake, ensuring in-order data and ensuring segment delivery through re-transmission
- UDP is connectionless, with no handshake, and anyone can send a segment in with no guarantee of delivery or order of delivery
### Configuration
When initializing the socket object:
- First argument is the address family like IPv4 (`AF_INET`). The main 3:
	- `AF_INET` = IPv4
	- `AF_INET6` = IPv6
	- `AF_UNIX` = inter-process communication on the same machine
- Second argument is the mode like TCP (`SOCK_STREAM`)
	- `SOCK_STREAM` = TCP
	- `SOCK_DGRAM` = UDP

For sending data:
- `send()` is not guaranteed to send all the data in one segment. You need to check and run send until it sends everything
- `sendall()` is a nice alternative that guarantees all data will be sent

For receiving data:
- `recv(1024)` will wait for **up to** but not necessarily 1024 bytes
	- If you get 1 byte, it will return 1 byte
	- If it gets 0 bytes, it indicates the **client has closed the connection**
	- If it gets more than 1024 bytes, you will read it in chunks
### Server
The server **binds** to a host & port and **listens** before then accepting requests from clients. 
When a request is accepted, it uses the client socket to get the data and send responses until close.
```python
import socket

server_socket = socket.socket(socker.AF_INET, socker.SOCK_STREAM)

server_host = '127.0.0.1'
server_port = 5001
server_socket.bind((server_host, server_port))

server_socket.listen(5) # queue up to 5 connections

while True:
	# The client_socket here represents the connection distinct from the server soc
	# The client_address has the IP and port of the client
	client_socket, client_address = server_socket.accept() # blocking wait
	data = client_socket.recv(1024)
	response = "hello!"
	client_socket.send(respones.encode('utf-8'))
	client_socket.close()

	# For UDP, it is connectionless and we can get segments from anywhere
	data, addr = udp_socket.recvfrom(1024)
```
### Client
The client **connects** to a server and sends messages, receiving replies. When done, the client closes, sending a **close** message to the server, which will then close down the client.
During the connect step, it does a 3-way handshake. 
```python
import socket

client_socket = socket.socket(socker.AF_INET, socker.SOCK_STREAM)

server_host = '127.0.0.1'
server_port = 5001
client_socket.connect((server_host, server_port))

message = "hello"
client_socket.send(message.encode('utf-8'))

data = client_socket.recv(1024)

client_socket.close()
```

### Security 
To use [[TLS & SSL Protocol]], we can do TLS on top of stream sockets. This wraps our connections in TLS. The ssl socket itself takes ownership of the client sockets.
The wrapping step automatically runs the TLS handshake with the server and sets up a secure connection. 
As usual, we can customize our [[TLS Certificates]]. 
Notice how the TLS connection is setup **after the TCP connection is made**

Client: 
```python
import socket
import ssl

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_address = ('example.com', 443)
client_socket.connect(server_address)

# The ssl socket takes OWNERSHIP of the client socket
tls_socket = ssl.wrap_socket(client_socket, ssl_version=ssl.PROTOCOL_TLS_CLIENT)

# Now, send & receive as normal
tls_socket.send(b'Hello, server!')
data = tls_socket.recv(1024)
tls_socket.close()
```

To customize certs:
```python
tls_socket = ssl.wrap_socket(
	 client_socket,
	 ssl_version=ssl.PROTOCOL_TLS_CLIENT,
	 certs_reqs=ssl.CERT_REQUIRED,
	 ca_certs='/path/to/trusted_ca.crt'
)
```

Server: 
```python
import socket
import ssl

context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile='/path/to/server.crt', keyfile='/path/to/server.key')

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('0.0.0.0', 443))
server_socket.listen(5)

while True:
    client_socket, client_address = server_socket.accept()
    # The ssl socket takes OWNERSHIP of the client socket
    ssl_socket = context.wrap_socket(client_socket, server_side=True)
    # Handle the SSL/TLS-encrypted communication with the client here
    ssl_socket.close() 
```
### In Practice
In practice, we can take a sockets server, and deploy it in a Docker container, mapping the listening port into the localhost. This allows other services to access the socket. 
- `0.0.0.0`: bind to **all** IPs on the machine
- `127.0.0.1`: bind to loopback IP on the machine

To-do: make good note on docker networking