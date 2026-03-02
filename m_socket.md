# Python Sockets Mastery Guide - Complete Learning Path

## Table of Contents
1. [Fundamentals](#fundamentals)
2. [Socket Module Overview](#socket-module-overview)
3. [Basic Socket Operations](#basic-socket-operations)
4. [Server-Client Communication](#server-client-communication)
5. [Advanced Concepts](#advanced-concepts)
6. [Real-World Examples](#real-world-examples)
7. [Best Practices](#best-practices)

---

## Fundamentals

### What is a Socket?
A socket is an **endpoint for sending or receiving data** across a network. Think of it like a telephone jack - you plug in and connect to another socket.

### Types of Sockets
- **TCP Sockets (SOCK_STREAM)**: Reliable, ordered, connection-based
- **UDP Sockets (SOCK_DGRAM)**: Fast, connectionless, may lose data
- **Raw Sockets (SOCK_RAW)**: Low-level, direct IP protocol access

### Address Families
- **AF_INET**: IPv4 addresses (192.168.1.1)
- **AF_INET6**: IPv6 addresses
- **AF_UNIX**: Local file sockets

---

## Socket Module Overview

### Importing the Socket Module
```python
import socket
```

### Core Module Functions

#### 1. **socket.socket()**
Creates a new socket object.

```python
# Simplest form - TCP socket for IPv4
sock = socket.socket()

# Explicit parameters
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```

#### 2. **socket.gethostname()**
Gets your computer's hostname.

```python
hostname = socket.gethostname()
print(hostname)  # Output: your-computer-name
```

#### 3. **socket.gethostbyname()**
Converts hostname to IP address.

```python
ip = socket.gethostbyname('localhost')
print(ip)  # Output: 127.0.0.1

ip = socket.gethostbyname('google.com')
print(ip)  # Output: 142.251.32.46 (example)
```

#### 4. **socket.inet_aton() / socket.inet_ntoa()**
Convert between IP address formats.

```python
# String to bytes
ip_bytes = socket.inet_aton('192.168.1.1')

# Bytes to string
ip_string = socket.inet_ntoa(ip_bytes)
```

#### 5. **socket.getservbyname()**
Gets port number for a service.

```python
port = socket.getservbyname('http')
print(port)  # Output: 80

port = socket.getservbyname('https')
print(port)  # Output: 443
```

---

## Basic Socket Operations

### Server Socket Methods

#### 1. **bind(address)**
Bind socket to an address and port.

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind(('localhost', 5000))
# ('localhost', 5000) = (host, port)
```

#### 2. **listen(backlog)**
Listen for incoming connections. Backlog = max queued connections.

```python
sock.listen(5)  # Queue up to 5 connections
```

#### 3. **accept()**
Accept an incoming connection. Returns (socket, address).

```python
client_socket, client_address = sock.accept()
print(f"Connected to {client_address}")
```

#### 4. **close()**
Close the socket.

```python
sock.close()
```

### Client Socket Methods

#### 1. **connect(address)**
Connect to a server.

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('localhost', 5000))
```

#### 2. **connect_ex(address)**
Same as connect() but returns error code instead of raising exception.

```python
error = sock.connect_ex(('localhost', 5000))
if error == 0:
    print("Connected!")
else:
    print(f"Connection failed: {error}")
```

### Data Transfer Methods

#### 1. **send(data)**
Send bytes to connected socket.

```python
message = "Hello Server"
sock.send(message.encode())  # Must be bytes, not string
```

#### 2. **sendall(data)**
Send all data (keeps sending until everything is sent).

```python
sock.sendall(b"Hello Server")  # Better for large data
```

#### 3. **recv(bufsize)**
Receive up to bufsize bytes.

```python
data = sock.recv(1024)  # Receive up to 1024 bytes
message = data.decode()  # Convert bytes to string
```

#### 4. **recvfrom(bufsize)**
Receive data and sender address (mainly for UDP).

```python
data, address = sock.recvfrom(1024)
```

---

## Server-Client Communication

### Simple TCP Server

```python
import socket

def simple_server():
    # Create socket
    server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Bind to localhost:5000
    server_sock.bind(('localhost', 5000))
    
    # Listen for connections
    server_sock.listen(1)
    print("Server waiting for connection...")
    
    # Accept connection
    client_sock, client_address = server_sock.accept()
    print(f"Connected to {client_address}")
    
    # Receive data
    data = client_sock.recv(1024)
    print(f"Received: {data.decode()}")
    
    # Send response
    client_sock.send("Hello from server".encode())
    
    # Close
    client_sock.close()
    server_sock.close()

simple_server()
```

### Simple TCP Client

```python
import socket

def simple_client():
    # Create socket
    client_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Connect to server
    client_sock.connect(('localhost', 5000))
    print("Connected to server")
    
    # Send data
    client_sock.send("Hello from client".encode())
    
    # Receive response
    data = client_sock.recv(1024)
    print(f"Received: {data.decode()}")
    
    # Close
    client_sock.close()

simple_client()
```

---

## Advanced Concepts

### Socket Options: setsockopt() and getsockopt()

#### 1. **SO_REUSEADDR**
Allow socket to bind to address even if in TIME_WAIT state.

```python
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

#### 2. **SO_KEEPALIVE**
Enable TCP keep-alive.

```python
sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
```

#### 3. **SO_RCVBUF / SO_SNDBUF**
Set receive/send buffer size.

```python
sock.setsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF, 4096)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_SNDBUF, 4096)
```

#### 4. **TCP_NODELAY**
Disable Nagle's algorithm (send data immediately).

```python
sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)
```

### Getting Socket Options

```python
# Get receive buffer size
rcv_buf = sock.getsockopt(socket.SOL_SOCKET, socket.SO_RCVBUF)
print(f"Receive buffer: {rcv_buf}")
```

### Socket Timeouts

```python
# Set timeout to 5 seconds
sock.settimeout(5)

# Set no timeout (blocking)
sock.settimeout(None)

# Set non-blocking
sock.settimeout(0)
```

### Blocking vs Non-Blocking

```python
# Blocking socket (default) - waits for operation to complete
sock.setblocking(True)

# Non-blocking socket - returns immediately
sock.setblocking(False)
```

### Multi-Client Server with Threading

```python
import socket
import threading

def handle_client(client_sock, address):
    print(f"Connected to {address}")
    try:
        data = client_sock.recv(1024)
        print(f"Received from {address}: {data.decode()}")
        client_sock.send("Got your message".encode())
    finally:
        client_sock.close()

def multi_server():
    server_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server_sock.bind(('localhost', 5000))
    server_sock.listen(5)
    print("Server started on localhost:5000")
    
    try:
        while True:
            client_sock, address = server_sock.accept()
            # Handle each client in separate thread
            thread = threading.Thread(target=handle_client, args=(client_sock, address))
            thread.daemon = True
            thread.start()
    finally:
        server_sock.close()

multi_server()
```

### UDP Socket Example

```python
import socket

def udp_server():
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.bind(('localhost', 5000))
    print("UDP Server listening...")
    
    while True:
        data, address = sock.recvfrom(1024)
        print(f"Received from {address}: {data.decode()}")
        sock.sendto("Got it!".encode(), address)

def udp_client():
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.sendto("Hello UDP".encode(), ('localhost', 5000))
    data, _ = sock.recvfrom(1024)
    print(f"Received: {data.decode()}")
    sock.close()
```

---

## Real-World Examples

### Example 1: HTTP GET Request

```python
import socket

def http_get(host, path='/'):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((host, 80))
    
    # Send HTTP request
    request = f"GET {path} HTTP/1.1\r\nHost: {host}\r\nConnection: close\r\n\r\n"
    sock.sendall(request.encode())
    
    # Receive response
    response = b""
    while True:
        chunk = sock.recv(4096)
        if not chunk:
            break
        response += chunk
    
    sock.close()
    return response.decode('utf-8', errors='ignore')

# Usage
html = http_get('example.com')
print(html[:500])
```

### Example 2: Port Scanner

```python
import socket

def scan_port(host, port, timeout=1):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.settimeout(timeout)
    
    try:
        result = sock.connect_ex((host, port))
        sock.close()
        return result == 0  # True if port is open
    except socket.timeout:
        return False

def scan_ports(host, ports):
    open_ports = []
    for port in ports:
        if scan_port(host, port):
            open_ports.append(port)
            print(f"Port {port} is OPEN")
        else:
            print(f"Port {port} is closed")
    return open_ports

# Usage
ports = range(1, 100)
scan_ports('localhost', ports)
```

### Example 3: Echo Server (Multi-threaded)

```python
import socket
import threading

def echo_server(host='localhost', port=5000):
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((host, port))
    server.listen(5)
    print(f"Echo server started on {host}:{port}")
    
    def handle_connection(client, addr):
        try:
            while True:
                data = client.recv(1024)
                if not data:
                    break
                print(f"Echo from {addr}: {data.decode()}")
                client.sendall(data)  # Echo back
        finally:
            client.close()
    
    try:
        while True:
            client, addr = server.accept()
            thread = threading.Thread(target=handle_connection, args=(client, addr))
            thread.daemon = True
            thread.start()
    finally:
        server.close()

# Usage
# echo_server()
```

### Example 4: File Transfer

```python
import socket
import os

def send_file(filename, host, port):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((host, port))
    
    # Send filename
    sock.sendall(os.path.basename(filename).encode())
    sock.recv(1024)  # Wait for acknowledgment
    
    # Send file
    with open(filename, 'rb') as f:
        while True:
            data = f.read(4096)
            if not data:
                break
            sock.sendall(data)
    
    sock.close()
    print("File sent!")

def receive_file(output_dir, port):
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind(('localhost', port))
    server.listen(1)
    print(f"Waiting for file on port {port}...")
    
    client, addr = server.accept()
    
    # Receive filename
    filename = client.recv(1024).decode()
    client.sendall(b"OK")
    
    # Receive file
    filepath = os.path.join(output_dir, filename)
    with open(filepath, 'wb') as f:
        while True:
            data = client.recv(4096)
            if not data:
                break
            f.write(data)
    
    client.close()
    server.close()
    print(f"File received: {filepath}")
```

---

## Best Practices

### 1. Always Close Sockets
```python
try:
    sock = socket.socket()
    sock.connect(('localhost', 5000))
    # do work
finally:
    sock.close()
```

### 2. Use Context Manager (Recommended)
```python
with socket.socket() as sock:
    sock.connect(('localhost', 5000))
    # do work
# Socket auto-closes
```

### 3. Handle Exceptions
```python
try:
    sock = socket.socket()
    sock.connect(('localhost', 5000))
except socket.timeout:
    print("Connection timeout")
except socket.error as e:
    print(f"Socket error: {e}")
finally:
    sock.close()
```

### 4. Set SO_REUSEADDR for Servers
```python
server = socket.socket()
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind(('localhost', 5000))
```

### 5. Use Proper Timeouts
```python
sock.settimeout(5)  # Prevent hanging indefinitely
```

### 6. Buffer Management
```python
# Don't hardcode buffer sizes, make them configurable
BUFFER_SIZE = 4096

data = sock.recv(BUFFER_SIZE)
```

### 7. Graceful Shutdown
```python
def graceful_shutdown(sock):
    try:
        sock.shutdown(socket.SHUT_RDWR)
    except:
        pass
    finally:
        sock.close()
```

---

## Common Socket Errors

| Error | Meaning | Solution |
|-------|---------|----------|
| `ConnectionRefused` | Server not listening | Check server is running |
| `TimeoutError` | Operation took too long | Increase timeout or check network |
| `AddressAlreadyInUse` | Port already in use | Use SO_REUSEADDR or change port |
| `ConnectionReset` | Connection abruptly closed | Peer closed connection |
| `Timeout` | No data received | Set appropriate timeout |

---

## Learning Roadmap

### Week 1: Fundamentals
- [ ] Understand socket concept
- [ ] Learn socket vs AF_INET vs SOCK_STREAM
- [ ] Master socket(), bind(), listen(), accept()
- [ ] Create simple echo server

### Week 2: Client-Server
- [ ] Build TCP client
- [ ] Handle multiple clients with threading
- [ ] Learn send(), recv(), sendall()
- [ ] Practice connection handling

### Week 3: Advanced
- [ ] Learn socket options
- [ ] Handle timeouts and non-blocking
- [ ] Error handling patterns
- [ ] Build multi-threaded server

### Week 4: Real-World
- [ ] HTTP client
- [ ] Port scanner
- [ ] File transfer
- [ ] Protocol implementation

---

## Quick Reference Cheat Sheet

```python
# Create socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Server
sock.bind(('localhost', 5000))
sock.listen(5)
client, addr = sock.accept()

# Client
sock.connect(('localhost', 5000))

# Send/Receive
sock.send(b"data")
sock.sendall(b"data")
data = sock.recv(1024)

# Socket options
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.settimeout(5)
sock.setblocking(False)

# Close
sock.close()
```

---

## Additional Resources to Explore

- **ssl module**: Secure sockets with encryption
- **asyncio**: Async socket programming
- **select module**: Monitor multiple sockets
- **socketserver module**: High-level socket server framework
- **http.server**: Built-in HTTP server

---

**Happy Learning! Master these fundamentals and you'll be able to build any networked application.** 🚀
