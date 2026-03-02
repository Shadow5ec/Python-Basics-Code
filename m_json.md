# Master Python JSON - Complete Guide
## All Topics, All Modules, Super Simple Functions

---

## Table of Contents
1. [JSON Basics](#json-basics)
2. [Core `json` Module](#core-json-module)
3. [`json` Module Deep Dive](#json-module-deep-dive)
4. [Advanced `json` Module](#advanced-json-module)
5. [Other JSON Libraries](#other-json-libraries)
6. [Real-World Examples](#real-world-examples)
7. [Best Practices](#best-practices)

---

## JSON Basics

### What is JSON?
JSON (JavaScript Object Notation) is a lightweight format for storing and transporting data. It's human-readable and language-independent.

**JSON Data Types:**
- **String**: `"hello"`
- **Number**: `42`, `3.14`
- **Boolean**: `true`, `false`
- **Null**: `null`
- **Array**: `[1, 2, 3]`
- **Object**: `{"key": "value"}`

**JSON Structure:**
```json
{
  "name": "John",
  "age": 30,
  "is_student": false,
  "hobbies": ["reading", "coding"],
  "address": {
    "city": "New York",
    "zip": "10001"
  }
}
```

---

## Core `json` Module

The `json` module is built into Python. It converts between Python objects and JSON strings.

### 1. `json.dumps()` - Python to JSON String

**Purpose:** Convert Python dictionary/list to a JSON string.

```python
import json

# Simple dictionary
person = {"name": "Alice", "age": 25}
json_string = json.dumps(person)
print(json_string)
# Output: {"name": "Alice", "age": 25}
```

**More Examples:**

```python
import json

# List
numbers = [1, 2, 3, 4, 5]
json_string = json.dumps(numbers)
print(json_string)
# Output: [1, 2, 3, 4, 5]

# Nested structure
student = {
    "name": "Bob",
    "grades": [90, 85, 88],
    "passed": True
}
json_string = json.dumps(student)
print(json_string)
# Output: {"name": "Bob", "grades": [90, 85, 88], "passed": true}
```

---

### 2. `json.loads()` - JSON String to Python

**Purpose:** Convert JSON string to Python dictionary/list.

```python
import json

# JSON string to dictionary
json_string = '{"name": "Charlie", "age": 30}'
person = json.loads(json_string)
print(person)
# Output: {'name': 'Charlie', 'age': 30}
print(person["name"])
# Output: Charlie
```

**More Examples:**

```python
import json

# JSON array to list
json_array = '[1, 2, 3, 4, 5]'
numbers = json.loads(json_array)
print(numbers)
# Output: [1, 2, 3, 4, 5]

# Complex JSON
json_data = '{"user": {"name": "Diana", "age": 28}, "active": true}'
data = json.loads(json_data)
print(data["user"]["name"])
# Output: Diana
```

---

### 3. `json.dump()` - Write to File

**Purpose:** Save Python object to a JSON file.

```python
import json

person = {"name": "Eve", "age": 35, "city": "Boston"}

# Write to file
with open("person.json", "w") as file:
    json.dump(person, file)

# File contents: {"name": "Eve", "age": 35, "city": "Boston"}
```

**More Examples:**

```python
import json

students = [
    {"name": "Frank", "score": 95},
    {"name": "Grace", "score": 87}
]

with open("students.json", "w") as file:
    json.dump(students, file)
```

---

### 4. `json.load()` - Read from File

**Purpose:** Load JSON from a file into Python object.

```python
import json

# Read from file
with open("person.json", "r") as file:
    person = json.load(file)

print(person)
# Output: {'name': 'Eve', 'age': 35, 'city': 'Boston'}
```

**More Examples:**

```python
import json

with open("students.json", "r") as file:
    students = json.load(file)

for student in students:
    print(f"{student['name']}: {student['score']}")
```

---

## `json` Module Deep Dive

### 5. `json.dumps()` with `indent` Parameter

**Purpose:** Pretty-print JSON with indentation for readability.

```python
import json

person = {"name": "Henry", "age": 40, "hobbies": ["hiking", "cooking"]}

# Without indent (compact)
compact = json.dumps(person)
print(compact)
# {"name": "Henry", "age": 40, "hobbies": ["hiking", "cooking"]}

# With indent=2 (readable)
pretty = json.dumps(person, indent=2)
print(pretty)
# {
#   "name": "Henry",
#   "age": 40,
#   "hobbies": [
#     "hiking",
#     "cooking"
#   ]
# }
```

---

### 6. `json.dumps()` with `sort_keys` Parameter

**Purpose:** Sort dictionary keys alphabetically in JSON output.

```python
import json

person = {"name": "Iris", "age": 26, "city": "Seattle"}

# Without sorting
unsorted = json.dumps(person)
print(unsorted)
# {"name": "Iris", "age": 26, "city": "Seattle"}

# With sorting
sorted_json = json.dumps(person, sort_keys=True)
print(sorted_json)
# {"age": 26, "city": "Seattle", "name": "Iris"}
```

---

### 7. `json.dumps()` with `separators` Parameter

**Purpose:** Control spacing in JSON output.

```python
import json

person = {"name": "Jack", "age": 33}

# Default separators (', ' and ': ')
default = json.dumps(person)
print(default)
# {"name": "Jack", "age": 33}

# Compact separators (',', ':')
compact = json.dumps(person, separators=(',', ':'))
print(compact)
# {"name":"Jack","age":33}

# Custom separators
custom = json.dumps(person, separators=('; ', ' = '))
print(custom)
# {"name" = "Jack"; "age" = 33}
```

---

### 8. `json.dumps()` with `ensure_ascii` Parameter

**Purpose:** Handle non-ASCII characters (like emojis, special symbols).

```python
import json

data = {"greeting": "Hello 👋", "city": "Montréal"}

# Default (ASCII only)
ascii_only = json.dumps(data, ensure_ascii=True)
print(ascii_only)
# {"greeting": "Hello \\ud83d\\udc4b", "city": "Montr\\u00e9al"}

# Unicode characters
with_unicode = json.dumps(data, ensure_ascii=False)
print(with_unicode)
# {"greeting": "Hello 👋", "city": "Montréal"}
```

---

### 9. `json.dumps()` with `default` Parameter

**Purpose:** Handle non-serializable objects (datetime, custom classes, etc.).

```python
import json
from datetime import datetime

# Without default - this would raise an error
# json.dumps({"time": datetime.now()})  # TypeError!

# With default function
def convert_datetime(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError(f"Object of type {type(obj)} is not JSON serializable")

data = {"time": datetime.now(), "name": "Kelly"}
json_string = json.dumps(data, default=convert_datetime)
print(json_string)
# {"time": "2024-01-15T10:30:45.123456", "name": "Kelly"}
```

**Another Example with Custom Class:**

```python
import json

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

def serialize_person(obj):
    if isinstance(obj, Person):
        return {"name": obj.name, "age": obj.age}
    raise TypeError("Object is not JSON serializable")

person = Person("Leo", 28)
json_string = json.dumps(person, default=serialize_person)
print(json_string)
# {"name": "Leo", "age": 28}
```

---

### 10. `json.loads()` with `object_hook` Parameter

**Purpose:** Transform dictionaries during deserialization.

```python
import json

def add_title(obj):
    if "name" in obj:
        obj["title"] = "Mr./Ms."
    return obj

json_string = '{"name": "Mike", "age": 45}'
person = json.loads(json_string, object_hook=add_title)
print(person)
# {'name': 'Mike', 'age': 45, 'title': 'Mr./Ms.'}
```

---

### 11. `json.loads()` with `parse_float` Parameter

**Purpose:** Convert JSON numbers to a specific type (e.g., Decimal for precision).

```python
import json
from decimal import Decimal

json_string = '{"price": 19.99, "tax": 1.50}'

# Default (float)
data1 = json.loads(json_string)
print(data1)
# {'price': 19.99, 'tax': 1.5}

# Using Decimal for precision
data2 = json.loads(json_string, parse_float=Decimal)
print(data2)
# {'price': Decimal('19.99'), 'tax': Decimal('1.50')}
```

---

### 12. `json.loads()` with `parse_int` Parameter

**Purpose:** Convert JSON integers to a specific type.

```python
import json

json_string = '{"year": 2024, "quantity": 100}'

# Default (int)
data1 = json.loads(json_string)
print(data1)
# {'year': 2024, 'quantity': 100}

# Convert to string
data2 = json.loads(json_string, parse_int=str)
print(data2)
# {'year': '2024', 'quantity': '100'}
```

---

## Advanced `json` Module

### 13. JSONEncoder Class (Custom Serialization)

**Purpose:** Create custom encoder for complex objects.

```python
import json
from datetime import datetime

class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.strftime("%Y-%m-%d %H:%M:%S")
        return super().default(obj)

data = {"event": "Meeting", "time": datetime.now()}
json_string = json.dumps(data, cls=DateTimeEncoder)
print(json_string)
# {"event": "Meeting", "time": "2024-01-15 10:30:45"}
```

---

### 14. JSONDecoder Class (Custom Deserialization)

**Purpose:** Create custom decoder for complex data.

```python
import json
from datetime import datetime

class DateTimeDecoder(json.JSONDecoder):
    def __init__(self, *args, **kwargs):
        super().__init__(object_hook=self.object_hook, *args, **kwargs)
    
    def object_hook(self, obj):
        for key, val in obj.items():
            if key == "time" and isinstance(val, str):
                obj[key] = datetime.strptime(val, "%Y-%m-%d %H:%M:%S")
        return obj

json_string = '{"event": "Meeting", "time": "2024-01-15 10:30:45"}'
data = json.loads(json_string, cls=DateTimeDecoder)
print(data["time"])
# 2024-01-15 10:30:45
```

---

### 15. Handling Errors - `JSONDecodeError`

**Purpose:** Catch JSON parsing errors gracefully.

```python
import json

invalid_json = '{"name": "Noah", "age": 35,}'  # Trailing comma

try:
    data = json.loads(invalid_json)
except json.JSONDecodeError as e:
    print(f"Error: {e}")
    print(f"Line: {e.lineno}, Column: {e.colno}")
    print(f"Message: {e.msg}")
# Output: Error: Expecting property name enclosed in double quotes
# Line: 1, Column: 35
```

---

### 16. Streaming with `JSONDecoder.raw_decode()`

**Purpose:** Parse multiple JSON objects from a string.

```python
import json

json_string = '{"name": "Olivia"} {"name": "Peter"} {"name": "Quinn"}'
decoder = json.JSONDecoder()
idx = 0
objects = []

while idx < len(json_string):
    try:
        obj, end_idx = decoder.raw_decode(json_string, idx)
        objects.append(obj)
        idx = end_idx
        # Skip whitespace
        while idx < len(json_string) and json_string[idx].isspace():
            idx += 1
    except json.JSONDecodeError:
        break

for obj in objects:
    print(obj)
# {'name': 'Olivia'}
# {'name': 'Peter'}
# {'name': 'Quinn'}
```

---

## Other JSON Libraries

### 17. `simplejson` - Extended JSON Support

**Installation:** `pip install simplejson`

```python
import simplejson as json
from datetime import datetime

# simplejson supports more types
data = {
    "name": "Rachel",
    "timestamp": datetime.now(),
    "infinity": float('inf')
}

# simplejson can handle these
json_string = json.dumps(data, default=str)
print(json_string)
```

---

### 18. `ujson` - Ultra-Fast JSON (for performance)

**Installation:** `pip install ujson`

```python
import ujson as json

data = {"name": "Sam", "scores": [90, 85, 88, 92]}

# ujson is faster for large datasets
json_string = json.dumps(data)
parsed = json.loads(json_string)
print(parsed)
```

---

### 19. `orjson` - Fast JSON with Extra Features

**Installation:** `pip install orjson`

```python
import orjson
from datetime import datetime

data = {
    "name": "Tina",
    "created": datetime.now(),
    "values": [1.5, 2.3, 3.7]
}

# orjson returns bytes
json_bytes = orjson.dumps(data, default=str)
print(json_bytes)
# b'{"name":"Tina","created":"2024-01-15T10:30:45.123456","values":[1.5,2.3,3.7]}'

# Decode back
decoded = orjson.loads(json_bytes)
print(decoded)
```

---

### 20. `jsonschema` - Validate JSON Structure

**Installation:** `pip install jsonschema`

```python
from jsonschema import validate, ValidationError

schema = {
    "type": "object",
    "properties": {
        "name": {"type": "string"},
        "age": {"type": "integer", "minimum": 0}
    },
    "required": ["name", "age"]
}

# Valid data
valid_data = {"name": "Uma", "age": 30}
validate(instance=valid_data, schema=schema)
print("Valid!")

# Invalid data
invalid_data = {"name": "Victor", "age": -5}
try:
    validate(instance=invalid_data, schema=schema)
except ValidationError as e:
    print(f"Validation Error: {e.message}")
# Validation Error: -5 is less than the minimum of 0
```

---

### 21. `yaml` - YAML (Human-Friendly Alternative)

**Installation:** `pip install pyyaml`

```python
import yaml
import json

data = {
    "name": "Wendy",
    "hobbies": ["reading", "cycling"],
    "address": {"city": "Portland", "zip": "97201"}
}

# YAML format (more readable)
yaml_string = yaml.dump(data)
print(yaml_string)
# name: Wendy
# hobbies:
# - reading
# - cycling
# address:
#   city: Portland
#   zip: '97201'

# Convert back
parsed = yaml.safe_load(yaml_string)
print(parsed)
```

---

### 22. `toml` - TOML (Config File Format)

**Installation:** `pip install tomli_w`

```python
import json

# TOML is great for config files
toml_string = """
[user]
name = "Xavier"
age = 35

[user.preferences]
theme = "dark"
notifications = true
"""

# Convert to JSON
import tomli  # for reading (built-in in Python 3.11+)
# data = tomli.loads(toml_string)
print("TOML is best for config files, not general data")
```

---

## Real-World Examples

### Example 1: API Response Handling

```python
import json
import requests  # You might use this with real APIs

# Simulating an API response
api_response = '''
{
    "status": "success",
    "user": {
        "id": 123,
        "name": "Yara",
        "email": "yara@example.com"
    },
    "posts": [
        {"id": 1, "title": "First Post", "likes": 10},
        {"id": 2, "title": "Second Post", "likes": 25}
    ]
}
'''

data = json.loads(api_response)

# Access nested data
print(f"User: {data['user']['name']}")
for post in data['posts']:
    print(f"  - {post['title']} ({post['likes']} likes)")

# Output:
# User: Yara
#   - First Post (10 likes)
#   - Second Post (25 likes)
```

---

### Example 2: Configuration File Management

```python
import json

# Load config
with open("config.json", "r") as f:
    config = json.load(f)

# Use config
print(config["database"]["host"])
print(config["debug_mode"])

# Update config
config["debug_mode"] = False
config["version"] = "2.0"

# Save updated config
with open("config.json", "w") as f:
    json.dump(config, f, indent=2)
```

**Sample config.json:**
```json
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "myapp"
  },
  "debug_mode": true,
  "version": "1.0"
}
```

---

### Example 3: Database to JSON

```python
import json

# Simulating database records
users = [
    {"id": 1, "name": "Zara", "email": "zara@example.com"},
    {"id": 2, "name": "Aaron", "email": "aaron@example.com"}
]

# Convert to JSON and save
with open("users.json", "w") as f:
    json.dump(users, f, indent=2)

# Read and process
with open("users.json", "r") as f:
    users = json.load(f)

for user in users:
    print(f"{user['name']} ({user['email']})")
```

---

### Example 4: CSV to JSON

```python
import json
import csv

# Read CSV
csv_file = "data.csv"
json_file = "data.json"

data = []
with open(csv_file, "r") as f:
    csv_reader = csv.DictReader(f)
    for row in csv_reader:
        data.append(row)

# Save as JSON
with open(json_file, "w") as f:
    json.dump(data, f, indent=2)
```

---

### Example 5: JSON to CSV

```python
import json
import csv

json_file = "data.json"
csv_file = "data.csv"

# Load JSON
with open(json_file, "r") as f:
    data = json.load(f)

# Save as CSV
if data:
    keys = data[0].keys()
    with open(csv_file, "w", newline="") as f:
        csv_writer = csv.DictWriter(f, fieldnames=keys)
        csv_writer.writeheader()
        csv_writer.writerows(data)
```

---

### Example 6: Pretty-Print Large JSON

```python
import json

# Large nested data
large_data = {
    "users": [
        {
            "id": 1,
            "name": "Bailey",
            "profile": {"age": 28, "city": "Denver"},
            "posts": [
                {"id": 1, "content": "Hello!"},
                {"id": 2, "content": "Hi again!"}
            ]
        }
    ]
}

# Pretty print with 4-space indent
pretty_json = json.dumps(large_data, indent=4, sort_keys=True)
print(pretty_json)
```

---

### Example 7: Merge JSON Objects

```python
import json

# Two JSON objects
user1 = {"name": "Cassidy", "age": 30}
user2 = {"email": "cassidy@example.com", "city": "Chicago"}

# Merge dictionaries
merged = {**user1, **user2}
json_string = json.dumps(merged, indent=2)
print(json_string)
# {
#   "name": "Cassidy",
#   "age": 30,
#   "email": "cassidy@example.com",
#   "city": "Chicago"
# }
```

---

### Example 8: Filter JSON Data

```python
import json

# Original data
data = {
    "users": [
        {"name": "Dakota", "age": 25, "active": True},
        {"name": "Emery", "age": 30, "active": False},
        {"name": "Finley", "age": 28, "active": True}
    ]
}

# Filter active users
active_users = [u for u in data["users"] if u["active"]]

# Save filtered data
with open("active_users.json", "w") as f:
    json.dump(active_users, f, indent=2)
```

---

### Example 9: Extract Data from Nested JSON

```python
import json

data = {
    "company": "TechCorp",
    "departments": [
        {
            "name": "Engineering",
            "employees": [
                {"name": "Gray", "salary": 100000},
                {"name": "Harper", "salary": 95000}
            ]
        }
    ]
}

# Extract all employee names
employees = []
for dept in data["departments"]:
    for emp in dept["employees"]:
        employees.append(emp["name"])

print(employees)
# ['Gray', 'Harper']
```

---

### Example 10: Batch Process JSON Files

```python
import json
import os
from pathlib import Path

# Process all JSON files in a directory
json_files = Path("./data").glob("*.json")

for json_file in json_files:
    with open(json_file, "r") as f:
        data = json.load(f)
    
    # Process data
    print(f"Processing {json_file.name}: {len(data)} items")
    
    # Save modified data
    with open(f"processed_{json_file.name}", "w") as f:
        json.dump(data, f, indent=2)
```

---

## Best Practices

### 1. **Always Validate Input**

```python
import json

def safe_json_load(json_string):
    try:
        return json.loads(json_string)
    except json.JSONDecodeError as e:
        print(f"Invalid JSON: {e}")
        return None

data = safe_json_load('{"key": "value"}')
```

---

### 2. **Use `with` Statement for Files**

```python
import json

# Good
with open("file.json", "r") as f:
    data = json.load(f)

# Bad (file not properly closed)
# f = open("file.json", "r")
# data = json.load(f)
```

---

### 3. **Set Indent for Readability**

```python
import json

data = {"name": "Iris", "age": 25}

# Always use indent when saving for human readability
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)
```

---

### 4. **Handle Large Files Efficiently**

```python
import json

# For large files, read line by line (JSONL format)
def read_jsonl(filename):
    with open(filename, "r") as f:
        for line in f:
            yield json.loads(line)

# Usage
for obj in read_jsonl("large_file.jsonl"):
    # Process one object at a time
    pass
```

---

### 5. **Use `default` Parameter for Custom Objects**

```python
import json
from datetime import datetime

def json_serializer(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()
    raise TypeError(f"Type {type(obj)} not serializable")

data = {"time": datetime.now()}
json_string = json.dumps(data, default=json_serializer)
```

---

### 6. **Validate JSON Schema**

```python
from jsonschema import validate, ValidationError

schema = {
    "type": "object",
    "properties": {
        "name": {"type": "string"},
        "age": {"type": "integer"}
    },
    "required": ["name"]
}

data = {"name": "Jack", "age": 35}

try:
    validate(instance=data, schema=schema)
    print("Valid!")
except ValidationError as e:
    print(f"Invalid: {e.message}")
```

---

### 7. **Use Enums for Consistent Values**

```python
import json
from enum import Enum

class Status(Enum):
    ACTIVE = "active"
    INACTIVE = "inactive"

data = {"user": "Kelly", "status": Status.ACTIVE.value}
json_string = json.dumps(data)
print(json_string)
# {"user": "Kelly", "status": "active"}
```

---

### 8. **Compress Large JSON**

```python
import json
import gzip

data = {"users": [{"name": f"User{i}", "age": 20 + i} for i in range(1000)]}

# Save compressed
with gzip.open("data.json.gz", "wt") as f:
    json.dump(data, f)

# Load compressed
with gzip.open("data.json.gz", "rt") as f:
    data = json.load(f)
```

---

### 9. **Use `json.tool` for Command-Line Pretty-Printing**

```bash
# Pretty-print JSON from command line
python -m json.tool input.json

# Pretty-print and save
python -m json.tool input.json > output.json

# Validate JSON
python -m json.tool input.json > /dev/null && echo "Valid JSON"
```

---

### 10. **Keep Sensitive Data Secure**

```python
import json
import base64

# Never store passwords in JSON directly
# Use environment variables or secrets management

# If you must store encrypted data:
def encrypt_data(data):
    json_string = json.dumps(data)
    # Use proper encryption library (cryptography, etc.)
    return base64.b64encode(json_string.encode()).decode()

sensitive_data = {"password": "secret123"}
encrypted = encrypt_data(sensitive_data)
print(encrypted)
```

---

## Quick Reference

| Function | Purpose | Input | Output |
|----------|---------|-------|--------|
| `json.dumps()` | Convert to JSON string | Python object | JSON string |
| `json.loads()` | Parse JSON string | JSON string | Python object |
| `json.dump()` | Save to file | Python object, file | None (writes to file) |
| `json.load()` | Read from file | file | Python object |
| `JSONEncoder` | Custom serialization | - | - |
| `JSONDecoder` | Custom deserialization | - | - |

---

## Common Errors & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| `JSONDecodeError` | Invalid JSON syntax | Validate JSON before parsing |
| `TypeError` | Non-serializable object | Use `default` parameter |
| `FileNotFoundError` | File doesn't exist | Check file path |
| `KeyError` | Missing dictionary key | Use `.get()` method |

---

## Summary

You now have a complete guide to mastering Python JSON:

✅ **Core Functions:** `dumps()`, `loads()`, `dump()`, `load()`
✅ **Parameters:** `indent`, `sort_keys`, `separators`, `ensure_ascii`, `default`, `object_hook`
✅ **Advanced:** Custom encoders, decoders, error handling
✅ **Libraries:** `simplejson`, `ujson`, `orjson`, `jsonschema`, `yaml`, `toml`
✅ **Real-World:** API responses, config files, database conversion, filtering, merging
✅ **Best Practices:** Validation, error handling, security, optimization

**Keep practicing with these examples, and you'll master Python JSON! 🚀**
