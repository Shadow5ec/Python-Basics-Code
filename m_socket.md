# Python Sockets Mastery Guide

Complete guide to mastering Python sockets with all modules, functions, and simple examples.

---

## Table of Contents

1. [Socket Basics](#socket-basics)
2. [Socket Module](#socket-module)
3. [Socket Types](#socket-types)
4. [Core Functions](#core-functions)
5. [Server Implementation](#server-implementation)
6. [Client Implementation](#client-implementation)
7. [Communication Patterns](#communication-patterns)
8. [Error Handling](#error-handling)
9. [Advanced Topics](#advanced-topics)
10. [Real-World Examples](#real-world-examples)

---

## Socket Basics

### What is a Socket?

A socket is an endpoint for sending or receiving data across a network. Think of it like a telephone jack - it's the interface your program uses to connect to the network.

**Key Concepts:**
- **Host:** Computer's address (IP)
- **Port:** Application's address on the computer (0-65535)
- **Protocol:** Rules for communication (TCP, UDP)
- **Connection:** Established link between two sockets

---

## Socket Module

### Import the Socket Module

```python
import socket
```

### Essential Socket Module Attributes

```python
# Address families
socket.AF_INET          # IPv4
socket.AF_INET6         # IPv6
socket.AF_UNIX          # Unix sockets

# Socket types
socket.SOCK_STREAM      # TCP (connection-oriented)
socket.SOCK_DGRAM       # UDP (connectionless)
socket.SOCK_RAW         # Raw socket

# Common constants
socket.IPPROTO_TCP      # TCP protocol
socket.IPPROTO_UDP      # UDP protocol
socket.SOL_SOCKET       # Socket level options
```

---

## Socket Types

### 1. TCP Sockets (SOCK_STREAM)

**Characteristics:**
- Connection-oriented (must establish connection first)
- Reliable data delivery (guaranteed, ordered)
- Used for: HTTP, FTP, Email, SSH
- Slower but safer

```python
import socket

# Create TCP socket
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

### 2. UDP Sockets (SOCK_DGRAM)

**Characteristics:**
- Connectionless (send data without connection)
- Unreliable delivery (data may be lost)
- Used for: DNS, Video streaming, Online games
- Faster but less reliable

```python
import socket

# Create UDP socket
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```

---

## Core Functions

### Socket Creation

```python
import socket

# TCP Socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# UDP Socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# IPv6 TCP Socket
sock = socket.socket(socket.AF_INET6, socket.SOCK_STREAM)
```

### Binding to Port

```python
sock.bind(('localhost', 5000))
# Bind to specific address and port
# ('localhost', 5000) = (host, port)
```

### Listening for Connections (Server)

```python
sock.listen(5)
# Listen with max 5 connections in queue
# Only for TCP/SOCK_STREAM
```

### Accepting Connections (Server)

```python
client_socket, client_address = sock.accept()
# Returns: (socket object, (client_ip, client_port))
```

### Connecting to Server (Client)

```python
sock.connect(('localhost', 5000))
# Connect to host:port
# Only for TCP/SOCK_STREAM
```

### Sending Data

```python
# TCP - Send to connected socket
sock.send(b'Hello')
# Must send bytes, not strings

# UDP - Send to specific address
sock.sendto(b'Hello', ('localhost', 5000))
```

### Receiving Data

```python
# TCP - Receive from connected socket
data = sock.recv(1024)
# 1024 = max bytes to receive
# Returns bytes object

# UDP - Receive and get sender info
data, address = sock.recvfrom(1024)
# Returns (data, (sender_ip, sender_port))
```

### Closing Socket

```python
sock.close()
# Closes the socket connection
```

### Socket Options

```python
# Set socket options
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# This allows reusing address/port immediately

# Get socket options
value = sock.getsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR)
```

### Getting Socket Information

```python
# Get socket name (local address)
sock.getsockname()  # Returns (ip, port)

# Get peer name (remote address)
sock.getpeername()  # Returns (ip, port)
```

### Shutdown Socket

```python
# Shutdown before closing
sock.shutdown(socket.SHUT_RDWR)
# SHUT_RD = stop receiving
# SHUT_WR = stop sending
# SHUT_RDWR = stop both
```

---

## Server Implementation

### Simple TCP Server

```python
import socket

def start_tcp_server():
    # Create socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Allow reusing address
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    
    # Bind to port
    server_socket.bind(('localhost', 5000))
    
    # Listen for connections
    server_socket.listen(5)
    print("Server listening on port 5000...")
    
    try:
        while True:
            # Accept connection
            client_socket, client_address = server_socket.accept()
            print(f"Client connected: {client_address}")
            
            # Receive data
            data = client_socket.recv(1024)
            print(f"Received: {data.decode()}")
            
            # Send response
            client_socket.send(b"Message received!")
            
            # Close client socket
            client_socket.close()
    
    except KeyboardInterrupt:
        print("Server shutting down...")
    
    finally:
        server_socket.close()

# Run server
if __name__ == "__main__":
    start_tcp_server()
```

### Simple TCP Server with Threading

```python
import socket
import threading

def handle_client(client_socket, client_address):
    print(f"Client connected: {client_address}")
    
    try:
        data = client_socket.recv(1024)
        print(f"Received from {client_address}: {data.decode()}")
        
        client_socket.send(b"Message received!")
    
    except Exception as e:
        print(f"Error: {e}")
    
    finally:
        client_socket.close()

def start_tcp_server_threaded():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server_socket.bind(('localhost', 5000))
    server_socket.listen(5)
    
    print("Server listening on port 5000...")
    
    try:
        while True:
            client_socket, client_address = server_socket.accept()
            
            # Create thread for each client
            thread = threading.Thread(
                target=handle_client,
                args=(client_socket, client_address)
            )
            thread.daemon = True
            thread.start()
    
    except KeyboardInterrupt:
        print("Server shutting down...")
    
    finally:
        server_socket.close()

if __name__ == "__main__":
    start_tcp_server_threaded()
```

### Simple UDP Server

```python
import socket

def start_udp_server():
    # Create UDP socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
    # Bind to port
    server_socket.bind(('localhost', 5000))
    
    print("UDP Server listening on port 5000...")
    
    try:
        while True:
            # Receive data and sender info
            data, client_address = server_socket.recvfrom(1024)
            print(f"Received from {client_address}: {data.decode()}")
            
            # Send response back
            server_socket.sendto(b"Message received!", client_address)
    
    except KeyboardInterrupt:
        print("Server shutting down...")
    
    finally:
        server_socket.close()

if __name__ == "__main__":
    start_udp_server()
```

---

## Client Implementation

### Simple TCP Client

```python
import socket

def tcp_client():
    # Create socket
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    try:
        # Connect to server
        client_socket.connect(('localhost', 5000))
        print("Connected to server")
        
        # Send message
        message = "Hello Server!"
        client_socket.send(message.encode())
        print(f"Sent: {message}")
        
        # Receive response
        response = client_socket.recv(1024)
        print(f"Received: {response.decode()}")
    
    except ConnectionRefusedError:
        print("Could not connect to server")
    
    except Exception as e:
        print(f"Error: {e}")
    
    finally:
        client_socket.close()

if __name__ == "__main__":
    tcp_client()
```

### Simple UDP Client

```python
import socket

def udp_client():
    # Create UDP socket
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
    try:
        # Send message to server
        message = "Hello UDP Server!"
        client_socket.sendto(message.encode(), ('localhost', 5000))
        print(f"Sent: {message}")
        
        # Receive response
        data, server_address = client_socket.recvfrom(1024)
        print(f"Received from {server_address}: {data.decode()}")
    
    except Exception as e:
        print(f"Error: {e}")
    
    finally:
        client_socket.close()

if __name__ == "__main__":
    udp_client()
```

---

## Communication Patterns

### 1. Request-Response Pattern (TCP)

**Server waits for request, sends response**

```python
# SERVER
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind(('localhost', 5000))
server.listen(1)

client, addr = server.accept()
request = client.recv(1024)
client.send(b"Response: " + request)
client.close()
server.close()

# CLIENT
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 5000))
client.send(b"My request")
response = client.recv(1024)
print(response)
client.close()
```

### 2. Echo Server Pattern

**Server echoes back what client sends**

```python
# SERVER
import socket

def echo_server():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind(('localhost', 5000))
    server.listen(1)
    
    client, addr = server.accept()
    print(f"Connected: {addr}")
    
    while True:
        data = client.recv(1024)
        if not data:
            break
        client.send(data)  # Echo back
    
    client.close()
    server.close()

echo_server()
```

### 3. Broadcast Pattern (UDP)

**Send message to multiple recipients**

```python
import socket

def broadcast():
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)
    
    # Send to broadcast address
    sock.sendto(b"Hello everyone!", ('<broadcast>', 5000))
    sock.close()

broadcast()
```

### 4. Multiple Client Handling (Threading)

```python
import socket
import threading

def handle_client(sock, addr):
    print(f"Client: {addr}")
    sock.send(b"Hello client!")
    sock.close()

def server():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(('localhost', 5000))
    s.listen(5)
    
    while True:
        client, addr = s.accept()
        thread = threading.Thread(target=handle_client, args=(client, addr))
        thread.daemon = True
        thread.start()

server()
```

---

## Error Handling

### Common Socket Errors

```python
import socket

try:
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(('localhost', 5000))
    
except socket.timeout:
    print("Connection timed out")

except socket.gaierror:
    print("Address-related error")

except socket.error as e:
    print(f"Socket error: {e}")

except ConnectionRefusedError:
    print("Connection refused by server")

except OSError as e:
    print(f"OS error: {e}")

except Exception as e:
    print(f"Unexpected error: {e}")
```

### Timeout Handling

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Set timeout in seconds
sock.settimeout(5)

try:
    sock.connect(('localhost', 5000))
except socket.timeout:
    print("Connection timed out after 5 seconds")

# Disable timeout
sock.settimeout(None)

# Set timeout for receive
sock.settimeout(2)
try:
    data = sock.recv(1024)
except socket.timeout:
    print("No data received within 2 seconds")
```

---

## Advanced Topics

### 1. Non-Blocking Sockets

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Make socket non-blocking
sock.setblocking(False)

try:
    sock.connect(('localhost', 5000))
except BlockingIOError:
    print("Connection in progress...")

try:
    data = sock.recv(1024)
except BlockingIOError:
    print("No data available right now")
```

### 2. Select Module (Multiplexing)

```python
import socket
import select

# Create sockets
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 5000))
server.listen(5)

sockets = [server]

while True:
    # Wait for sockets to be ready
    readable, _, _ = select.select(sockets, [], [])
    
    for sock in readable:
        if sock == server:
            client, addr = server.accept()
            sockets.append(client)
            print(f"New connection: {addr}")
        else:
            data = sock.recv(1024)
            if not data:
                sockets.remove(sock)
            else:
                print(f"Received: {data.decode()}")
```

### 3. Socket Address Info

```python
import socket

# Get address information
result = socket.getaddrinfo('localhost', 5000)
# Returns list of (family, type, proto, canonname, sockaddr)

for family, type, proto, canonname, sockaddr in result:
    print(f"Family: {family}, Type: {type}, Address: {sockaddr}")

# Get host by name
ip = socket.gethostbyname('localhost')
print(f"IP: {ip}")

# Get host name
hostname = socket.gethostname()
print(f"Hostname: {hostname}")
```

### 4. Socket Keepalive

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Enable keepalive
sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)

# For Linux: set keepalive intervals
if hasattr(socket, 'TCP_KEEPIDLE'):
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPIDLE, 120)
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPINTVL, 30)
    sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_KEEPCNT, 5)
```

---

## Real-World Examples

### 1. HTTP Client

```python
import socket

def http_request(host, path='/'):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    try:
        # Connect to web server
        sock.connect((host, 80))
        
        # Send HTTP request
        request = f"GET {path} HTTP/1.1\r\nHost: {host}\r\nConnection: close\r\n\r\n"
        sock.send(request.encode())
        
        # Receive response
        response = b""
        while True:
            data = sock.recv(4096)
            if not data:
                break
            response += data
        
        print(response.decode())
    
    finally:
        sock.close()

# Usage
http_request('example.com', '/')
```

### 2. File Transfer Server

```python
import socket
import os

def file_server():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind(('localhost', 5000))
    server.listen(1)
    
    client, addr = server.accept()
    print(f"File request from {addr}")
    
    # Get filename from client
    filename = client.recv(1024).decode()
    
    # Send file
    if os.path.exists(filename):
        with open(filename, 'rb') as f:
            while True:
                data = f.read(4096)
                if not data:
                    break
                client.send(data)
        print("File sent")
    else:
        client.send(b"File not found")
    
    client.close()
    server.close()

file_server()
```

### 3. File Transfer Client

```python
import socket

def file_client(host, port, filename):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((host, port))
    
    # Request file
    sock.send(filename.encode())
    
    # Receive file
    with open(f"downloaded_{filename}", 'wb') as f:
        while True:
            data = sock.recv(4096)
            if not data:
                break
            f.write(data)
    
    print("File downloaded")
    sock.close()

file_client('localhost', 5000, 'myfile.txt')
```

### 4. Simple Chat Application

```python
# SERVER
import socket
import threading

def handle_chat_client(client_socket, clients_list):
    try:
        while True:
            msg = client_socket.recv(1024)
            if not msg:
                break
            
            # Broadcast to all clients
            for client in clients_list:
                if client != client_socket:
                    try:
                        client.send(msg)
                    except:
                        pass
    finally:
        clients_list.remove(client_socket)
        client_socket.close()

def chat_server():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind(('localhost', 5000))
    server.listen(5)
    
    clients = []
    
    try:
        while True:
            client, addr = server.accept()
            clients.append(client)
            print(f"User joined: {addr}")
            
            thread = threading.Thread(
                target=handle_chat_client,
                args=(client, clients)
            )
            thread.daemon = True
            thread.start()
    finally:
        server.close()

chat_server()

# CLIENT
import socket
import threading

def receive_messages(sock):
    while True:
        try:
            msg = sock.recv(1024)
            if msg:
                print(f"Message: {msg.decode()}")
        except:
            break

def chat_client():
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(('localhost', 5000))
    
    # Start receiving thread
    thread = threading.Thread(target=receive_messages, args=(sock,))
    thread.daemon = True
    thread.start()
    
    # Send messages
    while True:
        msg = input("You: ")
        sock.send(msg.encode())

chat_client()
```

### 5. DNS Query (UDP)

```python
import socket

def simple_dns_query(domain):
    # This is a simplified example
    # Real DNS requires specific packet format
    
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
    try:
        # Get IP of domain
        ip = socket.gethostbyname(domain)
        print(f"{domain} -> {ip}")
    except socket.gaierror:
        print(f"Cannot resolve {domain}")
    finally:
        sock.close()

simple_dns_query('google.com')
```

---

## Summary

### Socket Workflow

**TCP Server:**
1. Create socket → Bind → Listen → Accept → Receive → Send → Close

**TCP Client:**
1. Create socket → Connect → Send → Receive → Close

**UDP Server:**
1. Create socket → Bind → RecvFrom → SendTo → Close

**UDP Client:**
1. Create socket → SendTo → RecvFrom → Close

### When to Use What

| Protocol | Use Case | Reliable | Speed | Connection |
|----------|----------|----------|-------|------------|
| TCP | Email, Web, Files | Yes | Slower | Required |
| UDP | DNS, Gaming, Streaming | No | Faster | Not needed |

### Key Takeaways

1. **Bytes not Strings:** Always encode/decode when sending/receiving
2. **Always Close:** Use try-finally or context managers
3. **Handle Errors:** Network operations always fail sometimes
4. **Test Locally:** Use localhost:port for testing
5. **Port Numbers:** Ports 0-1023 are reserved (need admin)
6. **Reuse Address:** Use SO_REUSEADDR to restart servers quickly
7. **Thread Pool:** Use threading for multiple clients
8. **Timeouts:** Set timeouts to prevent hanging

---

## Quick Reference

```python
# Create socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Server side
s.bind(('localhost', 5000))
s.listen(5)
conn, addr = s.accept()

# Client side
s.connect(('localhost', 5000))

# Send/Receive
s.send(b'data')
data = s.recv(1024)

# Close
s.close()
```

---

**Now you have everything to master Python sockets! Start with simple examples and gradually build complexity.**
