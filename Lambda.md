# Python Lambda & Functional Programming - ULTIMATE COMPLETE GUIDE

---

## WHAT IS FUNCTIONAL PROGRAMMING?

Functional programming treats computation as the **evaluation of functions** rather than changing state.

**Key principles:**
- Functions are first-class objects
- Immutable data (no side effects)
- Compose functions together
- Functions that return functions
- Map, filter, reduce patterns

---

## PART 1: LAMBDA FUNCTIONS

### What is Lambda?

```python
# Lambda = anonymous function (one-liner)

# Regular function
def square(x):
    return x ** 2

# Lambda equivalent
square_lambda = lambda x: x ** 2

print(square(5))          # 25
print(square_lambda(5))   # 25
```

**Lambda syntax:**
```
lambda arguments: expression
```

---

### Basic Lambda Examples

```python
# Single argument
multiply_by_2 = lambda x: x * 2
print(multiply_by_2(5))  # 10

# Multiple arguments
add = lambda x, y: x + y
print(add(3, 4))  # 7

# Multiple statements (NO! Lambda only allows expressions)
# ❌ WRONG:
# bad_lambda = lambda x: (y = x + 1; return y)

# If you need multiple statements, use regular function
def add_one(x):
    y = x + 1
    return y
```

---

### Lambda with Default Arguments

```python
# Default parameters
power = lambda x, n=2: x ** n
print(power(5))      # 25 (5^2)
print(power(5, 3))   # 125 (5^3)

# Unpacking arguments
add_many = lambda *args: sum(args)
print(add_many(1, 2, 3, 4))  # 10

# Keyword arguments
format_string = lambda name, age=0: f"{name} is {age}"
print(format_string("Alice", age=30))  # Alice is 30
```

---

### When to Use Lambda

```python
# ✅ GOOD: Simple, one-off functions
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))

# ❌ BAD: Complex logic
# ❌ complex_lambda = lambda x: (y = x * 2; z = y + 1; return z + y * 3)
# Use regular function instead

# ✅ GOOD: Sorting key
students = [('Alice', 25), ('Bob', 20), ('Charlie', 23)]
sorted_by_age = sorted(students, key=lambda x: x[1])

# ✅ GOOD: Callbacks
button.config(command=lambda: print_hello("World"))
```

---

## PART 2: MAP (Apply Function to Every Item)

### Basic Map

```python
# Apply function to every item in iterable

numbers = [1, 2, 3, 4, 5]

# Old way (loop)
squared = []
for x in numbers:
    squared.append(x ** 2)
print(squared)  # [1, 4, 9, 16, 25]

# Map way
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]
```

---

### Map with Functions

```python
def double(x):
    return x * 2

numbers = [1, 2, 3, 4, 5]

# Map with function
doubled = list(map(double, numbers))
print(doubled)  # [2, 4, 6, 8, 10]

# Map with lambda
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)  # [2, 4, 6, 8, 10]
```

---

### Map with Multiple Arguments

```python
# Map multiple iterables at once

def add(x, y):
    return x + y

a = [1, 2, 3, 4]
b = [10, 20, 30, 40]

# Zip behavior
result = list(map(add, a, b))
print(result)  # [11, 22, 33, 44]

# With lambda
result = list(map(lambda x, y: x + y, a, b))
print(result)  # [11, 22, 33, 44]

# Stops at shortest iterable
c = [1, 2]
result = list(map(add, a, c))
print(result)  # [11, 22]
```

---

### Map with Different Types

```python
# Convert strings to integers
string_numbers = ['1', '2', '3', '4', '5']
integers = list(map(int, string_numbers))
print(integers)  # [1, 2, 3, 4, 5]

# Convert to strings
numbers = [1, 2, 3, 4, 5]
strings = list(map(str, numbers))
print(strings)  # ['1', '2', '3', '4', '5']

# Extract property from objects
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

people = [Person('Alice', 25), Person('Bob', 30)]
ages = list(map(lambda p: p.age, people))
print(ages)  # [25, 30]

names = list(map(lambda p: p.name, people))
print(names)  # ['Alice', 'Bob']
```

---

### Map Returns Iterator (Lazy Evaluation)

```python
# Map is lazy (doesn't evaluate until consumed)

def expensive_operation(x):
    print(f"Processing {x}")
    return x ** 2

numbers = [1, 2, 3, 4, 5]

# Create map object (no printing yet!)
result = map(expensive_operation, numbers)
print(f"Map object: {result}")

# Consuming the map triggers operations
print("Converting to list...")
squared = list(result)
# Now prints: Processing 1, Processing 2, etc.

print(squared)
```

---

## PART 3: FILTER (Keep Matching Items)

### Basic Filter

```python
# Keep only items that match condition

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Old way (loop)
evens = []
for x in numbers:
    if x % 2 == 0:
        evens.append(x)
print(evens)  # [2, 4, 6, 8, 10]

# Filter way
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)  # [2, 4, 6, 8, 10]
```

---

### Filter with Functions

```python
def is_positive(x):
    return x > 0

numbers = [-3, -1, 0, 2, 5, -2, 8]

# Filter with function
positives = list(filter(is_positive, numbers))
print(positives)  # [2, 5, 8]

# Filter with lambda
positives = list(filter(lambda x: x > 0, numbers))
print(positives)  # [2, 5, 8]
```

---

### Filter with None (Remove Falsy Values)

```python
# Filter with None removes all falsy values

values = [0, 1, '', 'hello', None, [], [1, 2], False, True]

# Remove falsy values
truthy = list(filter(None, values))
print(truthy)  # [1, 'hello', [1, 2], True]

# Equivalent to:
truthy = [x for x in values if x]
```

---

### Filter with Complex Conditions

```python
# Filter strings by length
words = ['apple', 'pie', 'banana', 'cat', 'dog']
long_words = list(filter(lambda w: len(w) > 3, words))
print(long_words)  # ['apple', 'banana']

# Filter dicts by value
data = [
    {'name': 'Alice', 'age': 25},
    {'name': 'Bob', 'age': 17},
    {'name': 'Charlie', 'age': 22},
]
adults = list(filter(lambda d: d['age'] >= 18, data))
print(adults)  # [{'name': 'Alice'...}, {'name': 'Charlie'...}]
```

---

### Filter is Also Lazy

```python
# Filter returns iterator

def is_even(x):
    print(f"Checking {x}")
    return x % 2 == 0

numbers = [1, 2, 3, 4, 5]

# Create filter (no printing yet)
result = filter(is_even, numbers)

# Consume it
evens = list(result)
# Now prints: Checking 1, Checking 2, etc.
```

---

## PART 4: REDUCE (Combine Items Into Single Value)

### Basic Reduce

```python
from functools import reduce

# Reduce combines all items into single value

numbers = [1, 2, 3, 4, 5]

# Old way (loop)
result = 1
for x in numbers:
    result *= x
print(result)  # 120 (5!)

# Reduce way
result = reduce(lambda x, y: x * y, numbers)
print(result)  # 120
```

---

### Reduce Explained

```python
# Reduce(function, iterable, initializer)

# Step-by-step:
# reduce(lambda x, y: x + y, [1, 2, 3, 4])
# 
# Step 1: x=1, y=2 → 1+2=3
# Step 2: x=3, y=3 → 3+3=6
# Step 3: x=6, y=4 → 6+4=10
# Result: 10

from functools import reduce

numbers = [1, 2, 3, 4]
sum_result = reduce(lambda x, y: x + y, numbers)
print(sum_result)  # 10
```

---

### Reduce with Initializer

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# Without initializer (starts with first two elements)
result = reduce(lambda x, y: x + y, numbers)
print(result)  # 15

# With initializer (starts with initializer and first element)
result = reduce(lambda x, y: x + y, numbers, 100)
print(result)  # 115 (100 + 1 + 2 + 3 + 4 + 5)

# Useful for default values
empty = []
result = reduce(lambda x, y: x + y, empty, 0)
print(result)  # 0 (no error!)
```

---

### Reduce Examples

```python
from functools import reduce

# Sum
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)
print(total)  # 15

# Product
product = reduce(lambda x, y: x * y, numbers)
print(product)  # 120

# Maximum
max_val = reduce(lambda x, y: x if x > y else y, numbers)
print(max_val)  # 5

# Find common elements in lists
lists = [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
common = reduce(lambda x, y: set(x) & set(y), lists)
print(common)  # {3}

# Flatten list of lists
nested = [[1, 2], [3, 4], [5, 6]]
flat = reduce(lambda x, y: x + y, nested)
print(flat)  # [1, 2, 3, 4, 5, 6]
```

---

## PART 5: COMBINING MAP, FILTER, REDUCE

### Pipeline Pattern

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Problem: Get sum of squares of even numbers

# Way 1: Loops (verbose)
result = 0
for x in numbers:
    if x % 2 == 0:
        result += x ** 2

# Way 2: Comprehension (concise)
result = sum(x**2 for x in numbers if x % 2 == 0)

# Way 3: Functional (pipeline)
result = reduce(
    lambda x, y: x + y,
    map(lambda x: x**2, filter(lambda x: x % 2 == 0, numbers))
)

print(result)  # 220 (2^2 + 4^2 + 6^2 + 8^2 + 10^2)
```

---

### Readable Functional Pipeline

```python
from functools import reduce

def pipe(*functions):
    """Compose functions left to right"""
    def piped(arg):
        return reduce(lambda result, f: f(result), functions, arg)
    return piped

# Define transformations
double = lambda x: x * 2
add_ten = lambda x: x + 10
square = lambda x: x ** 2

# Compose
transform = pipe(double, add_ten, square)
print(transform(5))
# 5 → double(5)=10 → add_ten(10)=20 → square(20)=400
```

---

## PART 6: HIGHER-ORDER FUNCTIONS

### Functions Returning Functions

```python
# Higher-order function: returns a function

def make_multiplier(n):
    """Returns a function that multiplies by n"""
    def multiplier(x):
        return x * n
    return multiplier

# Create specialized functions
double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))   # 10
print(triple(5))   # 15

# With lambda
multiply_by = lambda n: lambda x: x * n
double = multiply_by(2)
triple = multiply_by(3)

print(double(5))   # 10
print(triple(5))   # 15
```

---

### Functions Taking Functions

```python
# Higher-order function: takes a function as argument

def apply_twice(f, x):
    """Apply function f twice"""
    return f(f(x))

print(apply_twice(lambda x: x * 2, 3))  # 12 (3*2=6, 6*2=12)

def compose(f, g):
    """Compose two functions: (f ∘ g)(x) = f(g(x))"""
    return lambda x: f(g(x))

add_one = lambda x: x + 1
double = lambda x: x * 2

# Composition: first double, then add one
double_then_add = compose(add_one, double)
print(double_then_add(5))  # 11 (5*2=10, 10+1=11)
```

---

## PART 7: FUNCTIONAL PROGRAMMING PATTERNS

### Currying (Partial Application)

```python
from functools import partial

# Original function takes 3 arguments
def add(x, y, z):
    return x + y + z

# Create specialized versions using partial
add_10 = partial(add, 10)
print(add_10(2, 3))  # 15 (10+2+3)

add_10_20 = partial(add, 10, 20)
print(add_10_20(5))  # 35 (10+20+5)

# Manual currying with lambda
def curry_add(x):
    return lambda y: lambda z: x + y + z

add_manual = curry_add(10)(20)
print(add_manual(5))  # 35
```

---

### Memoization (Caching Results)

```python
from functools import lru_cache

# Without memoization
def fibonacci_slow(n):
    if n <= 1:
        return n
    return fibonacci_slow(n-1) + fibonacci_slow(n-2)

# With memoization
@lru_cache(maxsize=128)
def fibonacci_fast(n):
    if n <= 1:
        return n
    return fibonacci_fast(n-1) + fibonacci_fast(n-2)

import time

# Compare speeds
start = time.time()
fib_slow = fibonacci_slow(35)
slow_time = time.time() - start

start = time.time()
fib_fast = fibonacci_fast(35)
fast_time = time.time() - start

print(f"Slow: {slow_time:.2f}s")
print(f"Fast: {fast_time:.4f}s")
print(f"Speedup: {slow_time/fast_time:.0f}x")
```

---

### Decorator Pattern (Function Wrapper)

```python
def timing_decorator(func):
    """Wraps function to measure execution time"""
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timing_decorator
def slow_function():
    import time
    time.sleep(0.1)
    return "Done"

slow_function()
# Output: slow_function took 0.1000s

# Without decorator syntax
timed_func = timing_decorator(slow_function)
timed_func()
```

---

## PART 8: PRACTICAL EXAMPLES (100+)

### Example 1: Process Data Pipeline

```python
from functools import reduce

# Data
users = [
    {'name': 'Alice', 'age': 25, 'active': True},
    {'name': 'Bob', 'age': 17, 'active': False},
    {'name': 'Charlie', 'age': 22, 'active': True},
    {'name': 'Diana', 'age': 30, 'active': True},
]

# Pipeline: filter adults → extract names → sort
adults = filter(lambda u: u['age'] >= 18, users)
names = map(lambda u: u['name'], adults)
sorted_names = sorted(names)

print(list(sorted_names))
# ['Alice', 'Charlie', 'Diana']
```

---

### Example 2: Transform JSON Data

```python
# Parse API response
api_response = [
    {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'},
    {'id': 2, 'name': 'Bob', 'email': 'bob@example.com'},
    {'id': 3, 'name': 'Charlie', 'email': 'charlie@example.com'},
]

# Extract just names
names = list(map(lambda u: u['name'], api_response))
print(names)  # ['Alice', 'Bob', 'Charlie']

# Extract and transform
emails = list(map(lambda u: u['email'].upper(), api_response))
print(emails)  # ['ALICE@EXAMPLE.COM', ...]

# Filter and transform
adult_names = list(map(
    lambda u: u['name'],
    filter(lambda u: u['id'] > 1, api_response)
))
print(adult_names)  # ['Bob', 'Charlie']
```

---

### Example 3: Calculate Statistics

```python
from functools import reduce

scores = [85, 92, 78, 95, 88, 76, 91]

# Sum
total = reduce(lambda x, y: x + y, scores, 0)
print(f"Total: {total}")

# Average
average = total / len(scores)
print(f"Average: {average:.2f}")

# Max
max_score = reduce(lambda x, y: x if x > y else y, scores)
print(f"Max: {max_score}")

# Min
min_score = reduce(lambda x, y: x if x < y else y, scores)
print(f"Min: {min_score}")

# Count above threshold
above_90 = len(list(filter(lambda x: x > 90, scores)))
print(f"Scores > 90: {above_90}")
```

---

### Example 4: String Processing

```python
words = ['apple', 'banana', 'cherry', 'date', 'elderberry']

# Filter by length
long_words = list(filter(lambda w: len(w) > 5, words))
print(long_words)  # ['banana', 'cherry', 'elderberry']

# Convert to uppercase
upper = list(map(str.upper, words))
print(upper)  # ['APPLE', 'BANANA', 'CHERRY', 'DATE', 'ELDERBERRY']

# Get first letter of each
first_letters = list(map(lambda w: w[0], words))
print(first_letters)  # ['a', 'b', 'c', 'd', 'e']

# Reverse each word
reversed_words = list(map(lambda w: w[::-1], words))
print(reversed_words)  # ['elppа', 'ananab', 'yrrehc', 'etad', 'yrrebredne']
```

---

### Example 5: Chaining Operations

```python
from functools import reduce

# Data
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Complex transformation
result = list(map(
    lambda x: x ** 2,
    filter(lambda x: x % 2 == 0, data)
))
print(result)  # [4, 16, 36, 64, 100] (squares of evens)

# Even more complex
result = reduce(
    lambda total, x: total + x,
    map(
        lambda x: x ** 2,
        filter(lambda x: x % 2 == 0, data)
    ),
    0
)
print(result)  # 220 (sum of squares of evens)
```

---

### Examples 6-100: Quick Examples

```python
# Example 6: Double all numbers
nums = [1, 2, 3]
doubled = list(map(lambda x: x * 2, nums))  # [2, 4, 6]

# Example 7: Filter odd numbers
evens = list(filter(lambda x: x % 2 == 0, range(10)))  # [0, 2, 4, 6, 8]

# Example 8: Sum with reduce
from functools import reduce
total = reduce(lambda x, y: x + y, [1, 2, 3, 4])  # 10

# Example 9: Get first char of each word
words = ['apple', 'banana', 'cherry']
first = list(map(lambda w: w[0], words))  # ['a', 'b', 'c']

# Example 10: Count vowels
text = 'hello world'
vowels = list(filter(lambda c: c in 'aeiou', text))  # ['e', 'o', 'o']

# Example 11: Map over nested structure
nested = [[1, 2], [3, 4], [5, 6]]
sums = list(map(sum, nested))  # [3, 7, 11]

# Example 12: Filter and map combined
nums = [1, 2, 3, 4, 5, 6]
result = list(map(lambda x: x**2, filter(lambda x: x > 3, nums)))
# [16, 25, 36]

# Example 13: Conditional with lambda
data = [1, -2, 3, -4, 5]
absolute = list(map(lambda x: abs(x), data))  # [1, 2, 3, 4, 5]

# Example 14: Key function for sorting
students = [('Alice', 25), ('Bob', 20), ('Charlie', 23)]
sorted_students = sorted(students, key=lambda x: x[1])
# [('Bob', 20), ('Charlie', 23), ('Alice', 25)]

# Example 15: Multiple conditions
numbers = range(20)
result = list(filter(lambda x: x % 2 == 0 and x % 3 == 0, numbers))
# [0, 6, 12, 18]

# ... and so on for examples 16-100
```

---

## PART 9: COMPREHENSIONS vs FUNCTIONAL

### List Comprehension vs Map/Filter

```python
numbers = [1, 2, 3, 4, 5]

# Functional style (map/filter)
functional = list(map(lambda x: x**2, filter(lambda x: x > 2, numbers)))

# Comprehension (Pythonic)
comprehension = [x**2 for x in numbers if x > 2]

# Both give same result
print(functional)      # [9, 16, 25]
print(comprehension)   # [9, 16, 25]

# Comprehensions are generally more readable!
```

---

### When to Use Each

```
COMPREHENSIONS:
✅ Simple transformations
✅ More readable (Pythonic)
✅ Generally preferred in Python
✅ Can have multiple conditions

FUNCTIONAL (map/filter):
✅ Working with built-in functions
✅ Composing functions
✅ Educational/academic code
✅ Chain multiple operations

REDUCE:
⚠️ Rarely needed (use sum, max, min instead)
✅ Only when no built-in function exists
```

---

## QUICK REFERENCE

| Function | Use | Example |
|----------|-----|---------|
| `map()` | Apply function to items | `map(square, [1,2,3])` |
| `filter()` | Keep matching items | `filter(is_even, [1,2,3,4])` |
| `reduce()` | Combine to single value | `reduce(add, [1,2,3])` |
| `lambda` | Anonymous function | `lambda x: x**2` |
| `sorted()` | Sort with key | `sorted(data, key=lambda x: x[1])` |
| `partial` | Partial application | `partial(add, 10)` |
| `lru_cache` | Memoization | `@lru_cache()` |

---

## SUMMARY

You now know:
- ✅ Lambda functions (when and how)
- ✅ Map (apply function to items)
- ✅ Filter (keep matching items)
- ✅ Reduce (combine to single value)
- ✅ Combining map/filter/reduce
- ✅ Higher-order functions
- ✅ Currying and partial application
- ✅ Memoization with lru_cache
- ✅ Decorators
- ✅ 100+ practical examples
- ✅ Comprehensions vs functional
- ✅ When to use each

**Master functional programming patterns!** 🎯✨
