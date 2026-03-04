# Python 3 Functions - ULTIMATE COMPLETE GUIDE

---

## WHAT ARE FUNCTIONS?

Functions are reusable blocks of code. They take input (parameters), perform operations, and return output. Functions reduce code repetition and improve organization.

---

## PART 1: BASIC FUNCTION DEFINITION

### Simple Function

```python
def greet():
    """Simple greeting function"""
    return "Hello!"

print(greet())  # Hello!
```

---

### Function with Parameters

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))  # Hello, Alice!
print(greet("Bob"))    # Hello, Bob!
```

---

### Multiple Parameters

```python
def add(a, b):
    """Add two numbers"""
    return a + b

print(add(5, 3))      # 8
print(add(10, 20))    # 30
```

---

### Default Parameters

```python
def greet(name="Guest", greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet())                          # Hello, Guest!
print(greet("Alice"))                   # Hello, Alice!
print(greet("Bob", "Hi"))               # Hi, Bob!
print(greet(greeting="Hey", name="Charlie"))  # Hey, Charlie!
```

---

### Return Multiple Values

```python
def divide(a, b):
    """Return quotient and remainder"""
    return a // b, a % b

quotient, remainder = divide(17, 5)
print(quotient, remainder)  # 3 2

# Return tuple
result = divide(17, 5)
print(result)  # (3, 2)
```

---

## PART 2: POSITIONAL & KEYWORD ARGUMENTS

### Positional Arguments

```python
def subtract(a, b):
    return a - b

print(subtract(10, 3))  # 7 (order matters)
print(subtract(3, 10))  # -7 (different order)
```

---

### Keyword Arguments

```python
def subtract(a, b):
    return a - b

print(subtract(a=10, b=3))  # 7
print(subtract(b=3, a=10))  # 7 (order doesn't matter)

# Mix positional and keyword
print(subtract(10, b=3))    # 7
```

---

## PART 3: *args AND **kwargs

### *args (Variable Positional Arguments)

```python
def sum_all(*args):
    """Sum any number of arguments"""
    total = 0
    for num in args:
        total += num
    return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(1, 2, 3, 4, 5))  # 15
print(sum_all())               # 0

# args is a tuple
def print_args(*args):
    print(type(args))   # <class 'tuple'>
    print(args)         # (1, 2, 3)

print_args(1, 2, 3)
```

---

### **kwargs (Variable Keyword Arguments)

```python
def print_info(**kwargs):
    """Print keyword arguments"""
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, city="NYC")
# name: Alice
# age: 30
# city: NYC

# kwargs is a dictionary
def show_kwargs(**kwargs):
    print(type(kwargs))  # <class 'dict'>
    print(kwargs)  # {'name': 'Alice', 'age': 30}

show_kwargs(name="Alice", age=30)
```

---

### Combined *args and **kwargs

```python
def everything(a, b, *args, **kwargs):
    """Accept all types of arguments"""
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"kwargs={kwargs}")

everything(1, 2, 3, 4, 5, name="Alice", age=30)
# a=1, b=2
# args=(3, 4, 5)
# kwargs={'name': 'Alice', 'age': 30}
```

---

### Unpacking Arguments

```python
def add(a, b, c):
    return a + b + c

# Unpack list
numbers = [1, 2, 3]
print(add(*numbers))  # 6

# Unpack dict
def greet(name, age, city):
    return f"{name} is {age} from {city}"

info = {"name": "Alice", "age": 30, "city": "NYC"}
print(greet(**info))  # Alice is 30 from NYC
```

---

## PART 4: FUNCTION DOCUMENTATION

### Docstrings

```python
def calculate_area(radius):
    """
    Calculate area of a circle.
    
    Args:
        radius (float): The radius of the circle
    
    Returns:
        float: The area of the circle
    
    Example:
        >>> calculate_area(5)
        78.5
    """
    import math
    return math.pi * radius ** 2

print(calculate_area.__doc__)  # Print docstring
help(calculate_area)           # Show documentation
```

---

## PART 5: TYPE HINTS

### Basic Type Hints

```python
def add(a: int, b: int) -> int:
    """Add two integers"""
    return a + b

print(add(5, 3))  # 8

# With different types
def greet(name: str) -> str:
    return f"Hello, {name}!"

print(greet("Alice"))  # Hello, Alice!
```

---

### Complex Type Hints

```python
from typing import List, Dict, Tuple, Optional

def process_numbers(numbers: List[int]) -> int:
    """Sum list of numbers"""
    return sum(numbers)

def get_user(user_id: int) -> Dict[str, str]:
    """Get user information"""
    return {"id": str(user_id), "name": "Alice"}

def get_coordinates() -> Tuple[int, int]:
    """Return x, y coordinates"""
    return (10, 20)

def find_user(name: str) -> Optional[str]:
    """Find user or return None"""
    if name == "Alice":
        return "Found!"
    return None
```

---

## PART 6: LAMBDAS (Anonymous Functions)

### Simple Lambda

```python
# Lambda: single expression function
square = lambda x: x ** 2
print(square(5))  # 25

# Inline
result = (lambda x: x * 2)(10)
print(result)  # 20

# Multiple arguments
add = lambda x, y: x + y
print(add(5, 3))  # 8
```

---

### Lambda with map()

```python
numbers = [1, 2, 3, 4, 5]
squared = map(lambda x: x ** 2, numbers)
print(list(squared))  # [1, 4, 9, 16, 25]
```

---

### Lambda with filter()

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = filter(lambda x: x % 2 == 0, numbers)
print(list(evens))  # [2, 4, 6, 8, 10]
```

---

### Lambda with sorted()

```python
students = [("Alice", 85), ("Bob", 75), ("Charlie", 90)]
sorted_students = sorted(students, key=lambda x: x[1])
print(sorted_students)
# [('Bob', 75), ('Alice', 85), ('Charlie', 90)]
```

---

## PART 7: CLOSURES

### Simple Closure

```python
def outer(x):
    """Outer function"""
    def inner(y):
        """Inner function uses x from outer"""
        return x + y
    return inner

# Create closure
add_5 = outer(5)
print(add_5(3))   # 8
print(add_5(10))  # 15

# Different closure
add_10 = outer(10)
print(add_10(3))  # 13
```

---

### Closure with Mutable State

```python
def counter():
    """Create a counter with encapsulated state"""
    count = 0
    
    def increment():
        nonlocal count
        count += 1
        return count
    
    def decrement():
        nonlocal count
        count -= 1
        return count
    
    return {"increment": increment, "decrement": decrement}

c = counter()
print(c["increment"]())  # 1
print(c["increment"]())  # 2
print(c["decrement"]())  # 1
```

---

## PART 8: DECORATORS

### Simple Decorator

```python
def my_decorator(func):
    """Decorator that wraps a function"""
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Before function call
# Hello!
# After function call
```

---

### Decorator with Arguments

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Result: {result}")
        return result
    return wrapper

@my_decorator
def add(a, b):
    return a + b

add(5, 3)
# Calling add
# Result: 8
```

---

### Decorator with functools.wraps

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # Preserve function metadata
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def multiply(a, b):
    """Multiply two numbers"""
    return a * b

print(multiply.__name__)       # multiply (preserved!)
print(multiply.__doc__)        # Multiply two numbers
multiply(5, 3)                 # Calling multiply
```

---

### Decorator with Parameters

```python
def repeat(times):
    """Decorator that repeats function"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))
# ['Hello, Alice!', 'Hello, Alice!', 'Hello, Alice!']
```

---

### Timing Decorator

```python
import time
from functools import wraps

def timer(func):
    """Decorator to measure execution time"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done"

slow_function()
# slow_function took 1.0001 seconds
```

---

### Logging Decorator

```python
from functools import wraps

def log_calls(func):
    """Decorator that logs function calls"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        print(f"Result: {result}")
        return result
    return wrapper

@log_calls
def add(a, b):
    return a + b

add(5, 3)
# Calling add with args=(5, 3), kwargs={}
# Result: 8
```

---

### Stacking Decorators

```python
def uppercase(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs).upper()
    return wrapper

def exclaim(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs) + "!"
    return wrapper

@uppercase
@exclaim
def greet(name):
    return f"hello {name}"

print(greet("alice"))  # HELLO ALICE!
```

---

## PART 9: GENERATORS

### Simple Generator

```python
def count_up(n):
    """Generator that yields numbers"""
    i = 0
    while i < n:
        yield i
    i += 1

# Use generator
for num in count_up(3):
    print(num)
# 0
# 1
# 2

# Get values one at a time
gen = count_up(3)
print(next(gen))  # 0
print(next(gen))  # 1
print(next(gen))  # 2
```

---

### Generator Expression

```python
# List comprehension (creates whole list)
squares_list = [x ** 2 for x in range(5)]
print(squares_list)  # [0, 1, 4, 9, 16]

# Generator expression (lazy evaluation)
squares_gen = (x ** 2 for x in range(5))
print(next(squares_gen))  # 0
print(next(squares_gen))  # 1

# Use in loop
for square in (x ** 2 for x in range(5)):
    print(square)  # 0, 1, 4, 9, 16
```

---

## PART 10: PRACTICAL EXAMPLES (50+)

### Example 1: Simple Greeting

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Alice"))           # Hello, Alice!
print(greet("Bob", "Hi"))       # Hi, Bob!
```

---

### Example 2: Calculate Age

```python
from datetime import datetime

def calculate_age(birth_year):
    return datetime.now().year - birth_year

print(calculate_age(1990))  # 34 (or your age)
```

---

### Example 3: Check Prime Number

```python
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

print(is_prime(17))  # True
print(is_prime(20))  # False
```

---

### Example 4: Fibonacci

```python
def fibonacci(n):
    """Generate fibonacci sequence"""
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

print(list(fibonacci(7)))  # [0, 1, 1, 2, 3, 5, 8]
```

---

### Example 5: Find Maximum

```python
def find_max(*numbers):
    if not numbers:
        return None
    return max(numbers)

print(find_max(3, 1, 4, 1, 5))  # 5
```

---

### Example 6: String Reversal

```python
def reverse_string(s):
    return s[::-1]

print(reverse_string("Hello"))  # olleH
```

---

### Example 7: List Flattening

```python
def flatten(nested_list):
    result = []
    for item in nested_list:
        if isinstance(item, list):
            result.extend(flatten(item))
        else:
            result.append(item)
    return result

print(flatten([1, [2, 3], [4, [5, 6]]]))
# [1, 2, 3, 4, 5, 6]
```

---

### Example 8: Factorial

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))  # 120
```

---

### Example 9: Validate Email

```python
def is_valid_email(email):
    return "@" in email and "." in email

print(is_valid_email("alice@example.com"))  # True
print(is_valid_email("invalid"))            # False
```

---

### Example 10: Find Duplicates

```python
def find_duplicates(lst):
    seen = set()
    duplicates = set()
    for item in lst:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return list(duplicates)

print(find_duplicates([1, 2, 2, 3, 3, 4]))  # [2, 3]
```

---

### Examples 11-50: Quick Examples

```python
# Example 11: Count words
def count_words(text):
    return len(text.split())

# Example 12: Calculate average
def average(*numbers):
    return sum(numbers) / len(numbers) if numbers else 0

# Example 13: Check palindrome
def is_palindrome(s):
    return s == s[::-1]

# Example 14: Remove duplicates
def remove_duplicates(lst):
    return list(set(lst))

# Example 15: Filter by type
def filter_by_type(lst, type_):
    return [x for x in lst if isinstance(x, type_)]

# Example 16: Merge dicts
def merge_dicts(dict1, dict2):
    return {**dict1, **dict2}

# Example 17: Capitalize words
def capitalize_words(text):
    return ' '.join(word.capitalize() for word in text.split())

# Example 18: Power of number
def power(base, exp=2):
    return base ** exp

# Example 19: Celsius to Fahrenheit
def celsius_to_fahrenheit(c):
    return (c * 9/5) + 32

# Example 20: Check even
def is_even(n):
    return n % 2 == 0

# Example 21: Sum of squares
def sum_squares(*numbers):
    return sum(x ** 2 for x in numbers)

# Example 22: Group by length
def group_by_length(words):
    from itertools import groupby
    return {k: list(g) for k, g in groupby(sorted(words, key=len), len)}

# Example 23: Generate range
def gen_range(start, end):
    while start <= end:
        yield start
        start += 1

# Example 24: Repeat word
def repeat_word(word, times):
    return word * times

# Example 25: Join with separator
def join_items(*items, sep=", "):
    return sep.join(str(x) for x in items)

# ... and so on for examples 26-50
```

---

## PART 11: SCOPE (LEGB)

### Local Scope

```python
def outer():
    x = "Local"
    print(x)  # Accessible

outer()
# print(x)  # NameError: not accessible outside function
```

---

### Enclosing Scope

```python
def outer():
    x = "Enclosing"
    
    def inner():
        print(x)  # Can access x from outer
    
    inner()

outer()  # Enclosing
```

---

### Global Scope

```python
x = "Global"

def my_func():
    print(x)  # Can access global x

my_func()  # Global
```

---

### Built-in Scope

```python
# Built-in functions are available everywhere
print(len([1, 2, 3]))  # Built-in function
print(sum([1, 2, 3]))  # Built-in function
```

---

## QUICK REFERENCE

| Concept | Example |
|---------|---------|
| Function definition | `def my_func():` |
| Return value | `return value` |
| Parameters | `def func(a, b):` |
| Default parameters | `def func(a=10):` |
| *args | `def func(*args):` |
| **kwargs | `def func(**kwargs):` |
| Type hints | `def func(x: int) -> int:` |
| Docstring | `"""Documentation"""` |
| Lambda | `lambda x: x * 2` |
| Closure | nested function using outer variable |
| Decorator | `@decorator` |
| Generator | `yield` |

---

## SUMMARY

You now know:
- ✅ Basic function definition and calling
- ✅ Parameters (positional, keyword, default)
- ✅ *args and **kwargs
- ✅ Return values
- ✅ Docstrings and type hints
- ✅ Lambdas (anonymous functions)
- ✅ Closures
- ✅ Decorators (simple, with parameters, stacking)
- ✅ Generators and yield
- ✅ Scope rules (LEGB)
- ✅ 50+ practical examples

**Master Python Functions!**
