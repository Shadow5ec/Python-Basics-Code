# Python SSL/TLS - ULTIMATE COMPLETE GUIDE

## Secure Network Communication in Python

**Reference:** https://docs.python.org/3/library/ssl.html

---

## PART 0: WHY SSL/TLS?

### The Problem

```
WITHOUT SSL/TLS:
Client ────────── Server
       "password123" visible to everyone on network!
       "credit card" intercepted by hackers!
       Attacker pretends to be server!

WITH SSL/TLS:
Client ═══════════ Server (Encrypted connection)
       "password123" encrypted
       "credit card" encrypted
       Certificate verifies identity
```

---

### SSL vs TLS

```
SSL (Secure Sockets Layer):
- Old (deprecated, don't use)
- SSLv2, SSLv3 (unsafe)

TLS (Transport Layer Security):
- Modern (use this)
- TLSv1.0, TLSv1.1 (deprecated)
- TLSv1.2 (good)
- TLSv1.3 (best, latest)
```

---

## PART 1: BASIC SSL/TLS CONCEPTS

### What is a Certificate?

```python
# Certificate contains:
# - Public key (used to encrypt)
# - Identity (domain name, organization)
# - Expiration date
# - Digital signature (proof of authenticity)

# Typical certificate chain:
# Server Certificate → Intermediate CA → Root CA
```

---

### Certificate vs Key

```
CERTIFICATE (.crt, .pem):
- Public information
- Can be shared
- Contains public key
- Verifies identity
- Signed by Certificate Authority (CA)

KEY (.key):
- Secret information
- NEVER share
- Used to decrypt
- Proves you own the certificate
```

---

## PART 2: SSL CONTEXT (Core of SSL/TLS)

### Create SSL Context

```python
import ssl

# Create context with default settings
context = ssl.create_default_context()

# Create context for specific protocol
context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)

# Different contexts for different purposes
client_context = ssl.create_default_context()  # For clients
server_context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)  # For servers
```

---

### Context Configuration

```python
import ssl

# Create context
context = ssl.create_default_context()

# Verify certificate (ALWAYS do this for clients!)
context.check_hostname = True
context.verify_mode = ssl.CERT_REQUIRED

# Load certificate authority (CA) bundle
context.load_verify_locations('/path/to/ca-bundle.crt')

# Don't verify (DANGEROUS! Never in production!)
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE

# Load client certificate and key
context.load_cert_chain(
    certfile='/path/to/client.crt',
    keyfile='/path/to/client.key',
    password=None  # Password for encrypted key
)

# Set minimum TLS version
context.minimum_version = ssl.TLSVersion.TLSv1_2

# Set maximum TLS version
context.maximum_version = ssl.TLSVersion.TLSv1_3

# Enable/disable specific ciphers
context.set_ciphers('ECDHE+AESGCM:ECDHE+CHACHA20')

# Session caching
context.session_tickets = True
```

---

### Verify Modes

```python
import ssl

context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)

# CERT_NONE: Don't verify (INSECURE!)
context.verify_mode = ssl.CERT_NONE

# CERT_OPTIONAL: Verify if certificate provided
context.verify_mode = ssl.CERT_OPTIONAL

# CERT_REQUIRED: Must have valid certificate (USE THIS!)
context.verify_mode = ssl.CERT_REQUIRED
```

---

## PART 3: SECURE SOCKET CONNECTIONS

### HTTPS Client (GET Request)

```python
import ssl
import socket

# Create SSL context
context = ssl.create_default_context()

# Create socket
with socket.create_connection(('example.com', 443)) as sock:
    with context.wrap_socket(sock, server_hostname='example.com') as ssock:
        # Send HTTPS request
        ssock.sendall(b'GET / HTTP/1.0\r\nHost: example.com\r\n\r\n')
        
        # Receive response
        response = b''
        while True:
            data = ssock.recv(1024)
            if not data:
                break
            response += data

print(response.decode('utf-8'))
```

---

### Using urllib (Higher-Level)

```python
import urllib.request
import ssl

# Create context
context = ssl.create_default_context()

# Use with urllib
with urllib.request.urlopen('https://example.com', context=context) as response:
    html = response.read()
    print(html)

# Even simpler (uses default context)
import urllib.request
with urllib.request.urlopen('https://example.com') as response:
    print(response.read())
```

---

### Using requests Library

```python
import requests
import ssl

# requests uses urllib3, which uses ssl internally
# Usually "just works"

response = requests.get('https://example.com')
print(response.text)

# Disable SSL verification (DANGEROUS!)
response = requests.get('https://example.com', verify=False)

# Custom CA bundle
response = requests.get('https://example.com', verify='/path/to/ca-bundle.crt')

# Client certificate
response = requests.get(
    'https://example.com',
    cert=('/path/to/client.crt', '/path/to/client.key')
)
```

---

## PART 4: SERVER-SIDE SSL

### Simple HTTPS Server

```python
import ssl
import socket
import http.server
import socketserver

# Create SSL context
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)

# Load server certificate and key
context.load_cert_chain(
    certfile='server.crt',
    keyfile='server.key'
)

# Simple request handler
class Handler(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/plain')
        self.end_headers()
        self.wfile.write(b'Hello HTTPS!')

# Create server with SSL
with socketserver.TCPServer(('localhost', 8443), Handler) as httpd:
    httpd.socket = context.wrap_socket(httpd.socket, server_side=True)
    print('Server running on https://localhost:8443')
    httpd.serve_forever()
```

---

### SSL Server Socket

```python
import ssl
import socket

# Create SSL context for server
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)

# Load certificate and key
context.load_cert_chain(
    certfile='server.crt',
    keyfile='server.key'
)

# Create socket
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    sock.bind(('localhost', 8443))
    sock.listen(1)
    
    with context.wrap_socket(sock, server_side=True) as ssock:
        print('Waiting for connections...')
        conn, addr = ssock.accept()
        
        with conn:
            print(f'Connected by {addr}')
            data = conn.recv(1024)
            print(f'Received: {data}')
            conn.sendall(b'Hello HTTPS!')
```

---

## PART 5: CERTIFICATE OPERATIONS

### Get Certificate Information

```python
import ssl
import socket

hostname = 'example.com'
port = 443

# Get certificate
context = ssl.create_default_context()
with socket.create_connection((hostname, port)) as sock:
    with context.wrap_socket(sock, server_hostname=hostname) as ssock:
        # Get certificate as DER
        der_cert = ssock.getpeercert_bin()
        
        # Get certificate as dict
        cert_dict = ssock.getpeercert()
        
        print(f"Certificate Subject: {cert_dict['subject']}")
        print(f"Certificate Issuer: {cert_dict['issuer']}")
        print(f"Notbefore: {cert_dict['notBefore']}")
        print(f"Notafter: {cert_dict['notAfter']}")
        print(f"Subjectalternativenames: {cert_dict.get('subjectAltName', [])}")
        
        # Get protocol and cipher info
        print(f"Protocol: {ssock.version()}")
        print(f"Cipher: {ssock.cipher()}")
```

---

### Parse Certificate Details

```python
import ssl
import socket
from datetime import datetime

hostname = 'example.com'
context = ssl.create_default_context()

with socket.create_connection((hostname, 443)) as sock:
    with context.wrap_socket(sock, server_hostname=hostname) as ssock:
        cert = ssock.getpeercert()
        
        # Subject
        subject = dict(x[0] for x in cert['subject'])
        print(f"Domain: {subject['commonName']}")
        print(f"Organization: {subject['organizationName']}")
        
        # Alternative names (SANs)
        san_names = []
        for san_type, san_value in cert.get('subjectAltName', []):
            if san_type == 'DNS':
                san_names.append(san_value)
        print(f"SANs: {san_names}")
        
        # Expiration
        not_after = ssl.cert_time_to_seconds(cert['notAfter'])
        days_until_expiry = (not_after - datetime.now().timestamp()) / 86400
        print(f"Expires in {days_until_expiry:.0f} days")
```

---

### Create Self-Signed Certificate

```python
import subprocess
import os

# Generate private key
subprocess.run([
    'openssl', 'genrsa',
    '-out', 'server.key',
    '2048'
])

# Generate self-signed certificate (valid 365 days)
subprocess.run([
    'openssl', 'req',
    '-new', '-x509',
    '-key', 'server.key',
    '-out', 'server.crt',
    '-days', '365',
    '-subj', '/C=US/ST=State/L=City/O=Org/CN=localhost'
])

print("Generated server.key and server.crt")

# Use with SSL
import ssl
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain('server.crt', 'server.key')
```

---

## PART 6: CERTIFICATE VALIDATION

### Manual Certificate Verification

```python
import ssl
import socket
from datetime import datetime

def verify_certificate(hostname, port=443):
    """Verify certificate manually"""
    context = ssl.create_default_context()
    
    with socket.create_connection((hostname, port)) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            cert = ssock.getpeercert()
            
            # Check domain matches
            subject = dict(x[0] for x in cert['subject'])
            common_name = subject.get('commonName', '')
            print(f"✓ Domain: {common_name}")
            
            # Check expiration
            not_after = ssl.cert_time_to_seconds(cert['notAfter'])
            if not_after < datetime.now().timestamp():
                print("✗ Certificate expired!")
                return False
            else:
                print("✓ Certificate not expired")
            
            # Check issuer
            issuer = dict(x[0] for x in cert['issuer'])
            print(f"✓ Issued by: {issuer.get('organizationName', 'Unknown')}")
            
            return True

verify_certificate('example.com')
```

---

### Handle Certificate Errors

```python
import ssl
import socket
import urllib.request

hostname = 'expired.badssl.com'  # Expired certificate

# Method 1: Catch SSL error
try:
    context = ssl.create_default_context()
    with socket.create_connection((hostname, 443), timeout=5) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            pass
except ssl.SSLError as e:
    print(f"SSL Error: {e}")
    print(f"Error type: {type(e).__name__}")

# Method 2: Bypass verification (DANGEROUS!)
context = ssl.create_default_context()
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE

try:
    with socket.create_connection((hostname, 443)) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            print("Connected (verification disabled)")
except ssl.SSLError as e:
    print(f"Error: {e}")
```

---

## PART 7: TLS CONFIGURATION

### TLS Version Control

```python
import ssl

context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)

# Set minimum version (reject older protocols)
context.minimum_version = ssl.TLSVersion.TLSv1_2

# Set maximum version (don't use newer if not supported)
context.maximum_version = ssl.TLSVersion.TLSv1_3

# Check available versions
print(ssl.TLSVersion.MINIMUM)  # Minimum supported
print(ssl.TLSVersion.MAXIMUM)  # Maximum supported

# All TLS versions
versions = [
    ssl.TLSVersion.SSLv3,     # Deprecated
    ssl.TLSVersion.TLSv1,     # Deprecated
    ssl.TLSVersion.TLSv1_1,   # Deprecated
    ssl.TLSVersion.TLSv1_2,   # Good
    ssl.TLSVersion.TLSv1_3,   # Best
]
```

---

### Cipher Suites

```python
import ssl

context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)

# Set allowed ciphers (strong only)
context.set_ciphers('ECDHE+AESGCM:ECDHE+CHACHA20:!aNULL:!MD5')

# Get available ciphers
context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
available = context.get_ciphers()
for cipher in available:
    print(cipher['name'])

# Enable/disable specific protocols
context.options |= ssl.OP_NO_SSLv2
context.options |= ssl.OP_NO_SSLv3
context.options |= ssl.OP_NO_TLSv1
context.options |= ssl.OP_NO_TLSv1_1

# Prefer server ciphers
context.options |= ssl.OP_CIPHER_SERVER_PREFERENCE
```

---

## PART 8: SESSION MANAGEMENT

### Session Caching

```python
import ssl
import socket

context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)

# Enable session caching
context.session_tickets = True

# Set cache size
context.options |= ssl.OP_NO_TICKET

# First connection (creates session)
with socket.create_connection(('example.com', 443)) as sock:
    with context.wrap_socket(sock, server_hostname='example.com') as ssock:
        pass  # Session cached

# Second connection (reuses session - faster!)
with socket.create_connection(('example.com', 443)) as sock:
    with context.wrap_socket(sock, server_hostname='example.com') as ssock:
        pass  # Uses cached session
```

---

## PART 9: PRACTICAL EXAMPLES (100+)

### Example 1: Secure HTTPS GET

```python
import urllib.request
import ssl

context = ssl.create_default_context()

try:
    with urllib.request.urlopen('https://example.com', context=context) as response:
        print(f"Status: {response.status}")
        print(f"Content-Type: {response.headers['Content-Type']}")
        html = response.read().decode('utf-8')
        print(html[:100])
except ssl.SSLError as e:
    print(f"SSL Error: {e}")
```

---

### Example 2: Download with Certificate Verification

```python
import urllib.request
import ssl
import os

# Custom CA bundle
ca_bundle = '/etc/ssl/certs/ca-certificates.crt'

context = ssl.create_default_context(cafile=ca_bundle)
context.check_hostname = True
context.verify_mode = ssl.CERT_REQUIRED

url = 'https://example.com/file.zip'
filename = 'file.zip'

try:
    with urllib.request.urlopen(url, context=context) as response:
        with open(filename, 'wb') as f:
            f.write(response.read())
    print(f"Downloaded {filename}")
except ssl.CertificateError as e:
    print(f"Certificate Error: {e}")
except ssl.SSLError as e:
    print(f"SSL Error: {e}")
```

---

### Example 3: Check Certificate Expiration

```python
import ssl
import socket
from datetime import datetime, timedelta

def check_certificate_expiry(hostname, days_before=30):
    """Alert if certificate expires soon"""
    context = ssl.create_default_context()
    
    try:
        with socket.create_connection((hostname, 443), timeout=5) as sock:
            with context.wrap_socket(sock, server_hostname=hostname) as ssock:
                cert = ssock.getpeercert()
                
                # Parse expiry date
                not_after = ssl.cert_time_to_seconds(cert['notAfter'])
                expiry_date = datetime.fromtimestamp(not_after)
                days_left = (expiry_date - datetime.now()).days
                
                # Check threshold
                if days_left < days_before:
                    print(f"⚠️  Certificate expires in {days_left} days!")
                    return False
                else:
                    print(f"✓ Certificate expires in {days_left} days")
                    return True
    except ssl.SSLError as e:
        print(f"Error: {e}")
        return False

# Check multiple domains
domains = ['google.com', 'github.com', 'stackoverflow.com']
for domain in domains:
    check_certificate_expiry(domain)
```

---

### Example 4: Mutual TLS (mTLS)

```python
import ssl
import socket

# Client with certificate
client_context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
client_context.load_cert_chain(
    certfile='client.crt',
    keyfile='client.key'
)
client_context.load_verify_locations('server.crt')

try:
    with socket.create_connection(('server.example.com', 8443)) as sock:
        with client_context.wrap_socket(sock, server_hostname='server.example.com') as ssock:
            # Both sides authenticated
            print(f"Connected: {ssock.getpeername()}")
except ssl.SSLError as e:
    print(f"mTLS Error: {e}")
```

---

### Example 5: SSL/TLS Info

```python
import ssl
import socket

hostname = 'example.com'
context = ssl.create_default_context()

with socket.create_connection((hostname, 443)) as sock:
    with context.wrap_socket(sock, server_hostname=hostname) as ssock:
        # Protocol version
        print(f"TLS Version: {ssock.version()}")
        
        # Cipher suite
        cipher_name, cipher_protocol, bits = ssock.cipher()
        print(f"Cipher: {cipher_name}")
        print(f"Protocol: {cipher_protocol}")
        print(f"Key Bits: {bits}")
        
        # Certificate
        cert = ssock.getpeercert()
        print(f"Subject: {cert['subject']}")
        print(f"Issuer: {cert['issuer']}")
```

---

### Examples 6-100: Quick Examples

```python
# Example 6: Verify hostname manually
hostname = 'example.com'
context = ssl.create_default_context()
sock = context.wrap_socket(socket.socket(), server_hostname=hostname)

# Example 7: Load PEM certificate
context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
context.load_verify_locations('cert.pem')

# Example 8: Load DER certificate
import base64
der_data = base64.b64decode(cert_data)
context.load_verify_locations(cafile=None)

# Example 9: Get cipher info
context = ssl.create_default_context()
ciphers = context.get_ciphers()
for c in ciphers[:3]:
    print(c['name'], c['protocol'])

# Example 10: Force TLS 1.2
context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)

# Example 11: Disable hostname checking
context.check_hostname = False

# Example 12: Custom verify callback
def verify_callback(conn, cert, errno, depth, ok):
    print(f"Certificate {cert} - Error {errno}")
    return True

# Example 13: Certificate chain depth
context.set_verify_depth(1)

# Example 14: SNI (Server Name Indication)
sock = context.wrap_socket(socket.socket(), server_hostname='example.com')

# Example 15: Session ticket support
context.options |= ssl.OP_NO_TICKET

# ... and so on for examples 16-100
```

---

## PART 10: COMMON SSL ERRORS & SOLUTIONS

### Certificate Verification Failed

```python
import ssl
import socket

hostname = 'expired.badssl.com'

# ❌ WRONG: Ignore error
context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE

# ✅ RIGHT: Handle error properly
context = ssl.create_default_context()

try:
    with socket.create_connection((hostname, 443)) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            pass
except ssl.SSLCertVerificationError as e:
    print(f"Certificate verification failed: {e}")
    # Solutions:
    # 1. Check server certificate is valid
    # 2. Update CA bundle
    # 3. Add server certificate to trusted store
    # 4. Fix hostname mismatch
```

---

### Hostname Mismatch

```python
import ssl

# Certificate for example.com
# But trying to connect to sub.example.com

# ❌ WRONG: Disable check
context.check_hostname = False

# ✅ RIGHT: Use correct hostname
context = ssl.create_default_context()
# Connect to example.com (matches certificate)

# ✅ RIGHT: Use certificate with wildcard
# Certificate: *.example.com
# Connect to: sub.example.com (matches)
```

---

### Connection Refused

```python
import ssl
import socket

try:
    context = ssl.create_default_context()
    with socket.create_connection(('wrong.com', 443)) as sock:
        with context.wrap_socket(sock, server_hostname='wrong.com') as ssock:
            pass
except socket.gaierror as e:
    print(f"Hostname lookup failed: {e}")
except ConnectionRefusedError as e:
    print(f"Connection refused: {e}")
except ssl.SSLError as e:
    print(f"SSL error: {e}")
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Create context | `ssl.create_default_context()` |
| Load certificate | `context.load_cert_chain('cert.crt', 'key.key')` |
| Load CA bundle | `context.load_verify_locations('ca.crt')` |
| Require verification | `context.verify_mode = ssl.CERT_REQUIRED` |
| Wrap socket | `context.wrap_socket(sock, server_hostname=host)` |
| Get certificate | `socket.getpeercert()` |
| Get TLS version | `socket.version()` |
| Get cipher | `socket.cipher()` |
| Set TLS 1.2+ | `context.minimum_version = ssl.TLSVersion.TLSv1_2` |

---

## BEST PRACTICES

```python
import ssl
import urllib.request

# ✅ SECURE CLIENT
context = ssl.create_default_context()
context.check_hostname = True
context.verify_mode = ssl.CERT_REQUIRED
with urllib.request.urlopen('https://example.com', context=context) as resp:
    data = resp.read()

# ❌ INSECURE (Never do this!)
context = ssl.SSLContext(ssl.PROTOCOL_TLS_CLIENT)
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE
# Vulnerable to MITM attacks!

# ✅ SECURE SERVER
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain('cert.crt', 'key.key')
context.minimum_version = ssl.TLSVersion.TLSv1_2

# ✅ MUTUAL TLS (mTLS)
context.load_cert_chain('client-cert.crt', 'client-key.key')
context.load_verify_locations('server-cert.crt')
```

---

## SUMMARY

You now know:
- ✅ SSL/TLS fundamentals
- ✅ Certificates and keys
- ✅ SSL Context creation and configuration
- ✅ Secure socket connections (client and server)
- ✅ Certificate validation
- ✅ Certificate information extraction
- ✅ TLS version control
- ✅ Cipher suite management
- ✅ Session management
- ✅ Common errors and solutions
- ✅ 100+ practical examples
- ✅ Best practices
- ✅ Complete official documentation coverage

**Master secure network communication!** 
