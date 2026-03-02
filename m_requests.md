# Complete Guide to Mastering Python Requests Library

## Table of Contents
1. [Installation & Setup](#installation--setup)
2. [Core HTTP Methods](#core-http-methods)
3. [Response Objects](#response-objects)
4. [Headers & Authentication](#headers--authentication)
5. [Request Parameters](#request-parameters)
6. [Sessions](#sessions)
7. [Proxies & SSL](#proxies--ssl)
8. [Timeouts & Retries](#timeouts--retries)
9. [Cookies](#cookies)
10. [Advanced Features](#advanced-features)
11. [Error Handling](#error-handling)
12. [Real-World Examples](#real-world-examples)

---

## Installation & Setup

### Install Requests

```python
pip install requests
```

### Import the Library

```python
import requests
```

---

## Core HTTP Methods

### 1. GET Request (Retrieve Data)

**Purpose:** Fetch data from a server

```python
import requests

# Simple GET request
response = requests.get("https://api.example.com/users")
print(response.text)  # Print response body
```

**With URL Parameters:**

```python
# Method 1: URL parameters directly
response = requests.get("https://api.example.com/users?id=1&name=john")

# Method 2: Using params dictionary (BETTER)
params = {"id": 1, "name": "john"}
response = requests.get("https://api.example.com/users", params=params)
```

### 2. POST Request (Send Data)

**Purpose:** Send data to a server to create a new resource

```python
# Simple POST with data
data = {"username": "john", "email": "john@example.com"}
response = requests.post("https://api.example.com/users", data=data)
print(response.text)
```

**POST with JSON data:**

```python
# Using JSON (modern approach)
json_data = {"username": "john", "email": "john@example.com"}
response = requests.post("https://api.example.com/users", json=json_data)
```

### 3. PUT Request (Update Full Resource)

**Purpose:** Replace an entire resource

```python
# Update user with id=1
data = {"username": "jane", "email": "jane@example.com"}
response = requests.put("https://api.example.com/users/1", json=data)
```

### 4. PATCH Request (Update Partial Resource)

**Purpose:** Update only specific fields

```python
# Update only the email of user with id=1
data = {"email": "newemail@example.com"}
response = requests.patch("https://api.example.com/users/1", json=data)
```

### 5. DELETE Request (Remove Resource)

**Purpose:** Delete a resource

```python
# Delete user with id=1
response = requests.delete("https://api.example.com/users/1")
```

### 6. HEAD Request (Get Headers Only)

**Purpose:** Same as GET but only returns headers, no body

```python
response = requests.head("https://api.example.com/users")
print(response.headers)  # Only headers, no body
```

### 7. OPTIONS Request (Get Allowed Methods)

**Purpose:** Find out which HTTP methods are allowed

```python
response = requests.options("https://api.example.com/users")
print(response.headers.get("Allow"))  # Shows allowed methods
```

---

## Response Objects

### Understanding Response Structure

```python
response = requests.get("https://api.example.com/users")

# Access response content
response.text          # Response as string
response.content       # Response as bytes
response.json()        # Response as JSON (Python dict)
response.status_code   # HTTP status code (200, 404, etc.)
response.headers       # Response headers as dictionary
response.url           # Final URL after redirects
response.reason        # HTTP reason (OK, Not Found, etc.)
```

### Simple Example

```python
response = requests.get("https://api.example.com/users/1")

# Check if request succeeded
if response.status_code == 200:
    user_data = response.json()
    print(user_data["name"])
else:
    print(f"Error: {response.status_code}")
```

### Common Status Codes

```
200 - OK (success)
201 - Created (resource created)
204 - No Content
400 - Bad Request (invalid data)
401 - Unauthorized (need auth)
403 - Forbidden (access denied)
404 - Not Found
500 - Server Error
503 - Service Unavailable
```

### Working with Response Data

```python
response = requests.get("https://api.example.com/data")

# As JSON
data = response.json()
print(data["key"])

# As text
text = response.text
print(text)

# As bytes
raw_bytes = response.content
print(raw_bytes)

# Check encoding
print(response.encoding)

# Change encoding
response.encoding = "utf-8"
```

---

## Headers & Authentication

### Adding Headers

```python
# Custom headers
headers = {
    "User-Agent": "MyApp/1.0",
    "Accept": "application/json"
}
response = requests.get("https://api.example.com/data", headers=headers)
```

### Basic Authentication

```python
# Username and password
response = requests.get(
    "https://api.example.com/secure",
    auth=("username", "password")
)
```

### Bearer Token (OAuth)

```python
# For APIs that use bearer tokens
headers = {
    "Authorization": "Bearer YOUR_TOKEN_HERE"
}
response = requests.get("https://api.example.com/data", headers=headers)
```

### API Key in Header

```python
headers = {
    "X-API-Key": "your-api-key-here"
}
response = requests.get("https://api.example.com/data", headers=headers)
```

### API Key as Parameter

```python
params = {"api_key": "your-api-key-here"}
response = requests.get("https://api.example.com/data", params=params)
```

### Custom Auth Classes

```python
from requests.auth import AuthBase

class MyCustomAuth(AuthBase):
    def __init__(self, token):
        self.token = token
    
    def __call__(self, r):
        r.headers["Authorization"] = f"Custom {self.token}"
        return r

response = requests.get("https://api.example.com/data", auth=MyCustomAuth("token123"))
```

---

## Request Parameters

### URL Parameters (Query String)

```python
# Clean way using params dictionary
params = {
    "page": 1,
    "limit": 10,
    "search": "python"
}
response = requests.get("https://api.example.com/search", params=params)
```

### Form Data (application/x-www-form-urlencoded)

```python
# Traditional form data
form_data = {
    "username": "john",
    "password": "secret"
}
response = requests.post("https://api.example.com/login", data=form_data)
```

### JSON Data (application/json)

```python
# Modern JSON approach
json_data = {
    "name": "John",
    "email": "john@example.com"
}
response = requests.post("https://api.example.com/users", json=json_data)
```

### File Uploads

```python
# Upload a file
with open("myfile.txt", "rb") as f:
    files = {"file": f}
    response = requests.post("https://api.example.com/upload", files=files)
```

**With additional data:**

```python
# Upload file + other data
with open("myfile.txt", "rb") as f:
    files = {"file": f}
    data = {"description": "My file"}
    response = requests.post(
        "https://api.example.com/upload",
        files=files,
        data=data
    )
```

### Multiple Files

```python
files = [
    ("file", open("file1.txt", "rb")),
    ("file", open("file2.txt", "rb"))
]
response = requests.post("https://api.example.com/upload", files=files)
```

---

## Sessions

### What is a Session?

A session maintains state across multiple requests (cookies, headers, etc.)

### Basic Session Usage

```python
import requests

# Create a session
session = requests.Session()

# Set default headers
session.headers.update({"User-Agent": "MyApp/1.0"})

# Make requests (headers will be included automatically)
response1 = session.get("https://api.example.com/users")
response2 = session.get("https://api.example.com/posts")

# Close session when done
session.close()
```

### Session with Context Manager (Auto-closes)

```python
# Best practice - automatically closes
with requests.Session() as session:
    session.headers.update({"Authorization": "Bearer token"})
    response = session.get("https://api.example.com/data")
    print(response.json())
# Session closes automatically here
```

### Persistent Cookies

```python
with requests.Session() as session:
    # Login (gets cookies)
    session.post("https://api.example.com/login", 
                data={"user": "john", "pass": "secret"})
    
    # Subsequent requests include cookies automatically
    response = session.get("https://api.example.com/profile")
```

### Pre-configured Session

```python
session = requests.Session()
session.headers.update({
    "User-Agent": "MyBot/1.0",
    "Accept": "application/json"
})
session.auth = ("username", "password")

# All requests use these defaults
response = session.get("https://api.example.com/data")
```

---

## Proxies & SSL

### Using a Proxy

```python
# HTTP proxy
proxies = {
    "http": "http://proxy.example.com:8080",
    "https": "https://proxy.example.com:8080"
}
response = requests.get("https://api.example.com/data", proxies=proxies)
```

### Proxy with Authentication

```python
proxies = {
    "https": "https://user:pass@proxy.example.com:8080"
}
response = requests.get("https://api.example.com/data", proxies=proxies)
```

### Disable SSL Verification (NOT RECOMMENDED)

```python
# Skip SSL certificate verification (for testing only!)
response = requests.get("https://api.example.com/data", verify=False)
```

### Custom SSL Certificate

```python
# Use custom certificate
response = requests.get(
    "https://api.example.com/data",
    verify="/path/to/ca-bundle.crt"
)
```

---

## Timeouts & Retries

### Timeout Basics

```python
# Timeout in seconds
try:
    response = requests.get("https://api.example.com/data", timeout=5)
except requests.exceptions.Timeout:
    print("Request timed out!")
```

### Separate Connect and Read Timeouts

```python
# (connection_timeout, read_timeout)
response = requests.get(
    "https://api.example.com/data",
    timeout=(3, 10)  # 3 seconds to connect, 10 to read
)
```

### Retry Logic with Requests

```python
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

# Create session
session = requests.Session()

# Configure retry strategy
retry_strategy = Retry(
    total=3,  # Maximum retry attempts
    backoff_factor=1,  # Wait 1, 2, 4 seconds between retries
    status_forcelist=[429, 500, 502, 503, 504],  # Retry on these codes
)

# Apply to adapter
adapter = HTTPAdapter(max_retries=retry_strategy)
session.mount("https://", adapter)
session.mount("http://", adapter)

# Use session
response = session.get("https://api.example.com/data", timeout=5)
```

### Simple Retry Loop

```python
import time

max_retries = 3
for attempt in range(max_retries):
    try:
        response = requests.get("https://api.example.com/data", timeout=5)
        break  # Success, exit loop
    except requests.exceptions.RequestException as e:
        print(f"Attempt {attempt + 1} failed: {e}")
        if attempt < max_retries - 1:
            time.sleep(2 ** attempt)  # Exponential backoff
```

---

## Cookies

### Accessing Cookies

```python
response = requests.get("https://api.example.com/data")

# Get cookies from response
cookies = response.cookies
print(cookies)

# Access specific cookie
print(cookies.get("session_id"))
```

### Sending Cookies

```python
# Method 1: RequestsCookieJar
cookies = requests.cookies.RequestsCookieJar()
cookies.set("session_id", "abc123")
response = requests.get("https://api.example.com/data", cookies=cookies)

# Method 2: Dictionary
cookies = {"session_id": "abc123"}
response = requests.get("https://api.example.com/data", cookies=cookies)
```

### Persistent Cookies with Session

```python
with requests.Session() as session:
    # Login and get cookies
    session.post("https://api.example.com/login",
                data={"user": "john", "pass": "secret"})
    
    # Cookies automatically included in next request
    response = session.get("https://api.example.com/profile")
    print(response.json())
```

### Export and Import Cookies

```python
import pickle

session = requests.Session()
session.get("https://api.example.com/login")

# Save cookies
with open("cookies.pkl", "wb") as f:
    pickle.dump(session.cookies, f)

# Load cookies later
with open("cookies.pkl", "rb") as f:
    cookies = pickle.load(f)
    session.cookies.update(cookies)
```

---

## Advanced Features

### Streaming Large Files

```python
# Stream response (useful for large files)
response = requests.get(
    "https://example.com/largefile.zip",
    stream=True
)

# Read in chunks
with open("downloaded_file.zip", "wb") as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
```

### Event Hooks

```python
def print_response(r, *args, **kwargs):
    print(f"Status: {r.status_code}")
    print(f"URL: {r.url}")

hooks = {"response": print_response}
response = requests.get("https://api.example.com/data", hooks=hooks)
```

### Request Inspection Before Sending

```python
req = requests.Request(
    "GET",
    "https://api.example.com/data",
    params={"id": 1}
)

# Prepare the request
prepared = req.prepare()
print(prepared.url)

# Now send it
session = requests.Session()
response = session.send(prepared)
```

### Automatic Redirects

```python
# Allow redirects (default is True)
response = requests.get("https://api.example.com/data", allow_redirects=True)

# Disable redirects
response = requests.get("https://api.example.com/data", allow_redirects=False)
```

---

## Error Handling

### Common Exceptions

```python
import requests

try:
    response = requests.get("https://api.example.com/data", timeout=5)
    response.raise_for_status()  # Raises error if status >= 400
    
except requests.exceptions.Timeout:
    print("Request timed out")
    
except requests.exceptions.ConnectionError:
    print("Connection failed")
    
except requests.exceptions.HTTPError as e:
    print(f"HTTP Error: {e.response.status_code}")
    
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

### Raise for Status

```python
response = requests.get("https://api.example.com/users/999")

# Raises HTTPError if status is 404, 500, etc.
try:
    response.raise_for_status()
except requests.exceptions.HTTPError:
    print(f"Request failed: {response.status_code}")
```

### Check Status Code

```python
response = requests.get("https://api.example.com/data")

if response.status_code == 200:
    data = response.json()
    print(data)
elif response.status_code == 404:
    print("Resource not found")
elif response.status_code == 500:
    print("Server error")
else:
    print(f"Unexpected status: {response.status_code}")
```

---

## Real-World Examples

### Example 1: Weather API

```python
import requests

def get_weather(city):
    """Fetch weather data for a city"""
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": city,
        "appid": "your_api_key_here",
        "units": "metric"
    }
    
    try:
        response = requests.get(url, params=params, timeout=5)
        response.raise_for_status()
        
        data = response.json()
        temp = data["main"]["temp"]
        description = data["weather"][0]["description"]
        
        return f"{city}: {temp}°C, {description}"
    
    except requests.exceptions.RequestException as e:
        return f"Error fetching weather: {e}"

print(get_weather("London"))
```

### Example 2: GitHub API Search

```python
import requests

def search_github_repos(language, stars=1000):
    """Search GitHub repositories"""
    url = "https://api.github.com/search/repositories"
    
    params = {
        "q": f"language:{language} stars:>{stars}",
        "sort": "stars",
        "order": "desc"
    }
    
    headers = {
        "Accept": "application/vnd.github.v3+json"
    }
    
    try:
        response = requests.get(url, params=params, headers=headers, timeout=5)
        response.raise_for_status()
        
        repos = response.json()["items"][:5]
        
        for repo in repos:
            print(f"{repo['name']}: {repo['stargazers_count']} ⭐")
    
    except requests.exceptions.RequestException as e:
        print(f"Error: {e}")

search_github_repos("python")
```

### Example 3: API with Authentication

```python
import requests
import json

class TodoAPI:
    def __init__(self, api_key):
        self.base_url = "https://api.example.com"
        self.api_key = api_key
    
    def get_todos(self):
        """Get all todos"""
        url = f"{self.base_url}/todos"
        headers = {"Authorization": f"Bearer {self.api_key}"}
        
        response = requests.get(url, headers=headers)
        return response.json()
    
    def create_todo(self, title, description):
        """Create a new todo"""
        url = f"{self.base_url}/todos"
        headers = {"Authorization": f"Bearer {self.api_key}"}
        
        data = {
            "title": title,
            "description": description
        }
        
        response = requests.post(url, json=data, headers=headers)
        return response.json()
    
    def delete_todo(self, todo_id):
        """Delete a todo"""
        url = f"{self.base_url}/todos/{todo_id}"
        headers = {"Authorization": f"Bearer {self.api_key}"}
        
        response = requests.delete(url, headers=headers)
        return response.status_code == 204

# Usage
api = TodoAPI("your_api_key_here")
todos = api.get_todos()
print(todos)
```

### Example 4: Web Scraping with Requests

```python
import requests
from html.parser import HTMLParser

def fetch_page_title(url):
    """Fetch website title"""
    try:
        response = requests.get(url, timeout=5)
        response.raise_for_status()
        
        # Simple way: look for <title> tag
        html = response.text
        start = html.find("<title>") + 7
        end = html.find("</title>")
        
        if start != 6 and end != -1:
            return html[start:end]
        return "No title found"
    
    except requests.exceptions.RequestException as e:
        return f"Error: {e}"

print(fetch_page_title("https://example.com"))
```

### Example 5: Retry with Exponential Backoff

```python
import requests
import time

def fetch_with_retry(url, max_retries=3):
    """Fetch URL with exponential backoff"""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=5)
            response.raise_for_status()
            return response.json()
        
        except requests.exceptions.RequestException as e:
            if attempt == max_retries - 1:
                raise
            
            wait_time = 2 ** attempt
            print(f"Attempt {attempt + 1} failed. Retrying in {wait_time}s...")
            time.sleep(wait_time)

data = fetch_with_retry("https://api.example.com/data")
print(data)
```

### Example 6: Batch Requests

```python
import requests

def batch_fetch_users(user_ids):
    """Fetch multiple users"""
    base_url = "https://api.example.com/users"
    
    with requests.Session() as session:
        users = []
        
        for user_id in user_ids:
            url = f"{base_url}/{user_id}"
            response = session.get(url)
            
            if response.status_code == 200:
                users.append(response.json())
        
        return users

users = batch_fetch_users([1, 2, 3, 4, 5])
for user in users:
    print(user["name"])
```

---

## Quick Reference Cheat Sheet

```python
# Basic requests
requests.get(url)
requests.post(url, json={})
requests.put(url, json={})
requests.patch(url, json={})
requests.delete(url)

# With parameters
requests.get(url, params={"key": "value"})

# With headers
requests.get(url, headers={"User-Agent": "MyApp"})

# With authentication
requests.get(url, auth=("user", "pass"))

# With timeout
requests.get(url, timeout=5)

# Response methods
response.json()
response.text
response.content
response.status_code
response.headers
response.raise_for_status()

# Sessions
with requests.Session() as session:
    session.get(url)

# Error handling
try:
    response = requests.get(url)
    response.raise_for_status()
except requests.exceptions.RequestException as e:
    print(e)
```

---

## Summary

You now understand:
- ✅ All HTTP methods (GET, POST, PUT, PATCH, DELETE, HEAD, OPTIONS)
- ✅ Response objects and status codes
- ✅ Headers and authentication methods
- ✅ URL parameters and request data
- ✅ Sessions for persistent connections
- ✅ Proxies and SSL handling
- ✅ Timeouts and retry logic
- ✅ Cookies management
- ✅ Advanced features like streaming
- ✅ Error handling best practices
- ✅ Real-world practical examples

Master requests by practicing with real APIs!
