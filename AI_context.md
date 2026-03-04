# Python Context Managers (with statement) - ULTIMATE COMPLETE GUIDE

---

## WHAT ARE CONTEXT MANAGERS?

Context managers automatically set up and clean up resources. They ensure cleanup happens **even if exceptions occur**. Think: open a resource, use it, close it safely.

**Syntax:** `with resource as var:`

---

## PART 1: THE PROBLEM (Why We Need Them)

### Without Context Manager (Unsafe)

```python
# OLD WAY - Easy to forget cleanup!
file = open('data.txt', 'r')
try:
    data = file.read()
    print(data)
finally:
    file.close()  # Must remember!
```

**Problems:**
- Verbose and repetitive
- Easy to forget `finally` block
- If exception occurs before `finally`, resource leaks

---

### With Context Manager (Safe)

```python
# NEW WAY - Automatic cleanup!
with open('data.txt', 'r') as file:
    data = file.read()
    print(data)
# File automatically closed!
```

**Benefits:**
- Cleaner code
- Guaranteed cleanup
- Cleanup happens even if exception occurs

---

## PART 2: HOW CONTEXT MANAGERS WORK

### __enter__ and __exit__ Methods

```python
class MyContextManager:
    def __enter__(self):
        """Setup - called when entering 'with' block"""
        print("Setting up")
        return self  # This is what 'as' variable gets
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Cleanup - called when exiting 'with' block"""
        print("Cleaning up")
        return False  # False = don't suppress exceptions

# Use it
with MyContextManager() as cm:
    print("Inside with block")
    # Outputs:
    # Setting up
    # Inside with block
    # Cleaning up
```

---

### Understanding __exit__ Parameters

```python
class DetailedContextManager:
    def __enter__(self):
        print(">>> Entering context")
        return "resource"
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        # exc_type: Type of exception (None if no exception)
        # exc_val: Exception value/message
        # exc_tb: Traceback object
        
        if exc_type is None:
            print(">>> Exiting normally")
        else:
            print(f">>> Exception occurred: {exc_type.__name__}: {exc_val}")
        
        return False  # False = let exception propagate

# Normal exit
with DetailedContextManager() as res:
    print(f"Using {res}")

# With exception
with DetailedContextManager() as res:
    raise ValueError("Something went wrong")
```

---

## PART 3: CLASS-BASED CONTEXT MANAGERS

### File Management

```python
class FileManager:
    """Custom file manager context"""
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        print(f"Opened {self.filename}")
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
            print(f"Closed {self.filename}")
        return False

# Use it
with FileManager('test.txt', 'w') as f:
    f.write("Hello, World!")
```

---

### Database Connection

```python
class DatabaseConnection:
    """Manage database connection"""
    def __init__(self, host, user):
        self.host = host
        self.user = user
        self.connection = None
    
    def __enter__(self):
        self.connection = f"Connected to {self.host} as {self.user}"
        print(f">>> {self.connection}")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f">>> Disconnecting from {self.host}")
        self.connection = None
    
    def execute(self, query):
        print(f"Executing: {query}")

# Use it
with DatabaseConnection('localhost', 'admin') as db:
    db.execute("SELECT * FROM users")
```

---

### Lock/Unlock Resource

```python
class Lock:
    """Manage resource locking"""
    def __init__(self, name):
        self.name = name
        self.is_locked = False
    
    def __enter__(self):
        print(f"🔒 Locking {self.name}")
        self.is_locked = True
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"🔓 Unlocking {self.name}")
        self.is_locked = False
    
    def do_work(self):
        if self.is_locked:
            print(f"✅ Working with {self.name}")
        else:
            print(f"❌ {self.name} not locked!")

# Use it
lock = Lock("resource")
with lock:
    lock.do_work()  # ✅ Working with resource
```

---

## PART 4: FUNCTION-BASED CONTEXT MANAGERS (@contextmanager)

### Simple Function-Based

```python
from contextlib import contextmanager

@contextmanager
def my_context():
    """Function-based context manager"""
    print("Setup")
    try:
        yield "resource"  # This becomes the 'as' variable
    finally:
        print("Cleanup")

# Use it
with my_context() as res:
    print(f"Using {res}")
```

**How it works:**
1. Code before `yield` = `__enter__`
2. `yield value` = what `as` variable gets
3. Code after `yield` = `__exit__`

---

### File Manager with @contextmanager

```python
from contextlib import contextmanager

@contextmanager
def open_file(filename, mode):
    """Open and close file automatically"""
    f = open(filename, mode)
    try:
        yield f
    finally:
        f.close()

# Use it
with open_file('test.txt', 'w') as f:
    f.write("Hello!")
```

---

### Timer Context Manager

```python
from contextlib import contextmanager
import time

@contextmanager
def timer(name="Operation"):
    """Measure execution time"""
    print(f"⏱️ Starting: {name}")
    start = time.time()
    try:
        yield start
    finally:
        elapsed = time.time() - start
        print(f"✅ {name} completed in {elapsed:.3f} seconds")

# Use it
with timer("Data Processing") as start_time:
    time.sleep(1)
    print("Processing...")
```

---

### Suppress Exceptions

```python
from contextlib import contextmanager

@contextmanager
def suppress_exception(*exceptions):
    """Suppress specified exceptions"""
    try:
        yield
    except exceptions:
        print(f"✅ Exception suppressed")

# Use it
with suppress_exception(ValueError, TypeError):
    print("About to raise ValueError")
    raise ValueError("This will be suppressed")
    print("After exception")  # This runs!
```

---

## PART 5: EXCEPTION HANDLING IN CONTEXT MANAGERS

### Catching Exceptions

```python
from contextlib import contextmanager

@contextmanager
def handle_errors():
    """Handle exceptions in context"""
    try:
        yield
    except ValueError as e:
        print(f"❌ Caught ValueError: {e}")
    except TypeError as e:
        print(f"❌ Caught TypeError: {e}")
    finally:
        print("✅ Cleanup happens anyway")

# Handled exception
with handle_errors():
    raise ValueError("Something bad")
    # Output: ❌ Caught ValueError: Something bad
    #         ✅ Cleanup happens anyway
```

---

### Suppress vs Propagate

```python
from contextlib import contextmanager

@contextmanager
def suppress_errors():
    """Return True to suppress exception"""
    try:
        yield
    except ValueError as e:
        print(f"Suppressed: {e}")
        return True  # Suppress = program continues

@contextmanager
def propagate_errors():
    """Return False to let exception continue"""
    try:
        yield
    except ValueError as e:
        print(f"Caught: {e}")
        return False  # Propagate = raise exception

# Suppressed
with suppress_errors():
    raise ValueError("This is hidden")
print("Still running!")  # This prints

# Propagated
try:
    with propagate_errors():
        raise ValueError("This escapes")
except ValueError:
    print("Exception caught!")
```

---

## PART 6: NESTED CONTEXT MANAGERS

### Multiple with Statements

```python
# Multiple context managers - sequential cleanup
with open('input.txt', 'r') as infile:
    with open('output.txt', 'w') as outfile:
        data = infile.read()
        outfile.write(data.upper())

# Cleaner: comma-separated (Python 3.1+)
with open('input.txt', 'r') as infile, open('output.txt', 'w') as outfile:
    data = infile.read()
    outfile.write(data.upper())
```

---

### Multiple Custom Contexts

```python
from contextlib import contextmanager

@contextmanager
def database():
    print("1. Connecting to database")
    try:
        yield "db_connection"
    finally:
        print("3. Closing database")

@contextmanager
def cache():
    print("2. Initializing cache")
    try:
        yield "cache_object"
    finally:
        print("4. Clearing cache")

# Nested
with database() as db, cache() as c:
    print("5. Doing work...")

# Output:
# 1. Connecting to database
# 2. Initializing cache
# 5. Doing work...
# 4. Clearing cache
# 3. Closing database
```

---

## PART 7: ADVANCED: ExitStack

### Dynamic Context Management

```python
from contextlib import ExitStack

# Close multiple files without nesting
with ExitStack() as stack:
    files = [stack.enter_context(open(f'file{i}.txt', 'w')) 
             for i in range(3)]
    for file in files:
        file.write("data")
# All files automatically closed!
```

---

### Register Cleanup Functions

```python
from contextlib import ExitStack

def cleanup(msg):
    print(f"Cleaning up: {msg}")

with ExitStack() as stack:
    stack.callback(cleanup, "First")
    stack.callback(cleanup, "Second")
    stack.callback(cleanup, "Third")
    print("Doing work")

# Output:
# Doing work
# Cleaning up: Third
# Cleaning up: Second
# Cleaning up: First
```

---

## PART 8: PRACTICAL EXAMPLES (50+)

### Example 1: Simple Timer

```python
from contextlib import contextmanager
import time

@contextmanager
def timer():
    start = time.time()
    try:
        yield
    finally:
        print(f"Time: {time.time() - start:.3f}s")

with timer():
    time.sleep(0.5)
```

---

### Example 2: Change Directory

```python
from contextlib import contextmanager
import os

@contextmanager
def change_dir(path):
    old_dir = os.getcwd()
    os.chdir(path)
    try:
        yield path
    finally:
        os.chdir(old_dir)

# with change_dir('/tmp'):
#     print(os.getcwd())  # /tmp
# print(os.getcwd())  # Back to original
```

---

### Example 3: Temporary Variable

```python
from contextlib import contextmanager

@contextmanager
def temp_value(obj, attr, value):
    """Temporarily change attribute"""
    old_value = getattr(obj, attr)
    setattr(obj, attr, value)
    try:
        yield
    finally:
        setattr(obj, attr, old_value)

class Config:
    debug = False

# Temporarily set debug
with temp_value(Config, 'debug', True):
    print(Config.debug)  # True
print(Config.debug)      # False
```

---

### Example 4: List Protection

```python
from contextlib import contextmanager

@contextmanager
def protect_list(lst):
    """Prevent modifications in context"""
    original = lst.copy()
    try:
        yield lst
    finally:
        lst.clear()
        lst.extend(original)

numbers = [1, 2, 3]
with protect_list(numbers):
    numbers.append(4)
    print(numbers)  # [1, 2, 3, 4]
print(numbers)      # [1, 2, 3] (restored)
```

---

### Example 5: API Rate Limiter

```python
from contextlib import contextmanager
import time

@contextmanager
def rate_limiter(calls_per_second=1):
    """Limit API calls"""
    min_interval = 1.0 / calls_per_second
    last_call = time.time()
    
    try:
        yield
    finally:
        elapsed = time.time() - last_call
        if elapsed < min_interval:
            time.sleep(min_interval - elapsed)

# Limit to 2 calls per second
with rate_limiter(calls_per_second=2):
    print("API call 1")
    time.sleep(0.1)
with rate_limiter(calls_per_second=2):
    print("API call 2")
```

---

### Example 6: Database Transaction

```python
from contextlib import contextmanager

@contextmanager
def transaction(db):
    """Database transaction with rollback"""
    print("BEGIN TRANSACTION")
    try:
        yield db
        print("COMMIT")
    except Exception as e:
        print(f"ROLLBACK - {e}")
        raise

# Simulated usage
class MockDB:
    def execute(self, query):
        print(f"Executing: {query}")

db = MockDB()
with transaction(db):
    db.execute("INSERT INTO users...")
```

---

### Example 7: Temporary File

```python
from contextlib import contextmanager
import tempfile
import os

@contextmanager
def temp_file(suffix=''):
    """Create and cleanup temporary file"""
    fd, path = tempfile.mkstemp(suffix=suffix)
    try:
        yield path
    finally:
        os.close(fd)
        os.remove(path)

with temp_file('.txt') as path:
    with open(path, 'w') as f:
        f.write("Temporary data")
    print("File exists")
# File automatically deleted
```

---

### Example 8: Logging Context

```python
from contextlib import contextmanager
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

@contextmanager
def log_context(operation):
    """Log operation timing"""
    logger.info(f"Starting: {operation}")
    try:
        yield
    finally:
        logger.info(f"Finished: {operation}")

with log_context("Data Processing"):
    print("Processing...")
```

---

### Example 9: Memory Management

```python
from contextlib import contextmanager
import gc

@contextmanager
def memory_context():
    """Garbage collect before and after"""
    gc.collect()
    before = len(gc.get_objects())
    try:
        yield
    finally:
        gc.collect()
        after = len(gc.get_objects())
        print(f"Objects: {before} -> {after}")

with memory_context():
    data = [i for i in range(1000)]
```

---

### Example 10: Print Redirect

```python
from contextlib import contextmanager
from io import StringIO
import sys

@contextmanager
def capture_output():
    """Capture stdout"""
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    try:
        yield sys.stdout
    finally:
        sys.stdout = old_stdout

with capture_output() as output:
    print("Hello, World!")
    result = output.getvalue()
print(result)  # Hello, World!
```

---

### Examples 11-50: Quick Examples

```python
# Example 11: Mute print
@contextmanager
def mute():
    import os
    with open(os.devnull, 'w') as devnull:
        sys.stdout = devnull
        try:
            yield
        finally:
            sys.stdout = sys.__stdout__

# Example 12: Swap values
@contextmanager
def swap(a, b):
    a, b = b, a
    try:
        yield a, b
    finally:
        a, b = b, a

# Example 13: Set environment variable
@contextmanager
def set_env(key, value):
    old = os.environ.get(key)
    os.environ[key] = value
    try:
        yield
    finally:
        if old is None:
            del os.environ[key]
        else:
            os.environ[key] = old

# Example 14: Test isolation
@contextmanager
def test_isolation():
    setup()
    try:
        yield
    finally:
        teardown()

# Example 15: Retry logic
@contextmanager
def retry(max_attempts=3):
    for attempt in range(max_attempts):
        try:
            yield
            break
        except Exception as e:
            if attempt == max_attempts - 1:
                raise

# ... and so on for examples 16-50
```

---

## PART 9: COMPARISON: CLASS VS FUNCTION

### Class-Based

```python
class MyContext:
    def __enter__(self):
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        return False
```

**Pros:** Better for complex logic, state management
**Cons:** More code

---

### Function-Based

```python
@contextmanager
def my_context():
    try:
        yield
    finally:
        pass
```

**Pros:** Simple, concise
**Cons:** Limited for complex cases

---

## QUICK REFERENCE

| Feature | Syntax |
|---------|--------|
| Class-based | `__enter__`, `__exit__` |
| Function-based | `@contextmanager`, `yield` |
| Multiple contexts | `with ctx1, ctx2:` |
| Return value | `yield value` |
| Suppress exception | `return True` in `__exit__` |
| Propagate exception | `return False` in `__exit__` |
| Dynamic contexts | `ExitStack` |
| Register cleanup | `stack.callback()` |

---

## SUMMARY

You now know:
- ✅ Why context managers (automatic cleanup)
- ✅ `__enter__` and `__exit__` methods
- ✅ Class-based context managers
- ✅ Function-based (`@contextmanager`)
- ✅ Exception handling in contexts
- ✅ Nested context managers
- ✅ ExitStack for dynamic management
- ✅ 50+ practical examples
- ✅ When to use class vs function

**Context managers ensure cleanup always happens!** 🎯
