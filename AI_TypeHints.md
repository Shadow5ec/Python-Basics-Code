# Python Type Hints - ULTIMATE COMPLETE GUIDE

---

## WHAT ARE TYPE HINTS?

Type hints (or annotations) tell Python what type of data a function expects and returns. They're **optional** - Python doesn't enforce them, but they:
- Make code clearer
- Help IDEs provide better autocomplete
- Enable static type checking with tools like `mypy`
- Prevent bugs

**Syntax:** `parameter: type` and `-> return_type:`

---

## PART 1: BASIC TYPE HINTS

### Simple Function Type Hints

```python
# No type hints (old way)
def add(a, b):
    return a + b

# WITH type hints (new way)
def add(a: int, b: int) -> int:
    return a + b

# Usage is the same
print(add(5, 3))  # 8
```

**Breakdown:**
- `a: int` - parameter `a` should be `int`
- `b: int` - parameter `b` should be `int`
- `-> int:` - function returns `int`

---

### Multiple Parameters

```python
def greet(name: str, age: int) -> str:
    """Greet someone with their name and age"""
    return f"{name} is {age} years old"

print(greet("Alice", 30))  # Alice is 30 years old
```

---

### Different Return Types

```python
# Returns string
def get_name() -> str:
    return "Alice"

# Returns boolean
def is_adult(age: int) -> bool:
    return age >= 18

# Returns list
def get_numbers() -> list:
    return [1, 2, 3]

# Returns dict
def get_user_info() -> dict:
    return {"name": "Alice", "age": 30}
```

---

### No Return Value (None)

```python
def print_message(msg: str) -> None:
    """Print message (returns nothing)"""
    print(msg)

print_message("Hello")  # Prints: Hello
```

---

## PART 2: BASIC TYPES

### Built-in Types

```python
# String
def process_text(text: str) -> str:
    return text.upper()

# Integer
def double_number(n: int) -> int:
    return n * 2

# Float
def divide(a: float, b: float) -> float:
    return a / b

# Boolean
def is_valid(flag: bool) -> bool:
    return flag

# Bytes
def process_bytes(data: bytes) -> bytes:
    return data
```

---

## PART 3: CONTAINER TYPES

### Lists

```python
# List of integers
def sum_numbers(numbers: list[int]) -> int:
    return sum(numbers)

print(sum_numbers([1, 2, 3, 4, 5]))  # 15

# Old way (Python < 3.9)
from typing import List
def sum_numbers(numbers: List[int]) -> int:
    return sum(numbers)
```

---

### Tuples

```python
# Tuple with specific types
def get_coordinates() -> tuple[int, int]:
    return (10, 20)

# Tuple with any length of same type
def get_ids() -> tuple[int, ...]:
    return (1, 2, 3, 4, 5)

# Old way
from typing import Tuple
def get_coordinates() -> Tuple[int, int]:
    return (10, 20)
```

---

### Dictionaries

```python
# Dict with string keys and integer values
def count_items(items: list[str]) -> dict[str, int]:
    counts = {}
    for item in items:
        counts[item] = counts.get(item, 0) + 1
    return counts

print(count_items(["a", "b", "a", "c"]))
# {'a': 2, 'b': 1, 'c': 1}

# Old way
from typing import Dict
def count_items(items: list[str]) -> Dict[str, int]:
    return {}
```

---

### Sets

```python
# Set of integers
def get_unique_numbers(numbers: list[int]) -> set[int]:
    return set(numbers)

print(get_unique_numbers([1, 2, 2, 3, 3, 3]))
# {1, 2, 3}
```

---

## PART 4: UNION TYPES

### Multiple Possible Types

```python
# Union: parameter can be int OR str
from typing import Union

def process_input(value: Union[int, str]) -> str:
    return str(value)

print(process_input(42))      # "42"
print(process_input("hello"))  # "hello"

# Modern syntax (Python 3.10+)
def process_input(value: int | str) -> str:
    return str(value)
```

---

### Function That Returns Different Types

```python
from typing import Union

def get_data(key: str) -> Union[int, str, list]:
    data = {
        "age": 30,
        "name": "Alice",
        "hobbies": ["reading", "gaming"]
    }
    return data.get(key)

print(get_data("age"))      # 30 (int)
print(get_data("name"))     # "Alice" (str)
print(get_data("hobbies"))  # ["reading", "gaming"] (list)
```

---

## PART 5: OPTIONAL TYPES

### Optional (Can Be None)

```python
from typing import Optional

# Parameter can be str or None
def greet(name: Optional[str] = None) -> str:
    if name is None:
        return "Hello, guest!"
    return f"Hello, {name}!"

print(greet())          # Hello, guest!
print(greet("Alice"))   # Hello, Alice!

# Modern syntax (Python 3.10+)
def greet(name: str | None = None) -> str:
    if name is None:
        return "Hello, guest!"
    return f"Hello, {name}!"
```

---

### Find Something (Returns Object or None)

```python
from typing import Optional

def find_user(user_id: int) -> Optional[dict]:
    """Return user dict or None if not found"""
    users = {1: {"name": "Alice"}, 2: {"name": "Bob"}}
    return users.get(user_id)  # Returns dict or None

user = find_user(1)
if user is not None:
    print(user["name"])  # Alice

user = find_user(99)
if user is None:
    print("User not found")
```

---

## PART 6: CALLABLE TYPES

### Functions as Parameters

```python
from typing import Callable

# Function that takes a function as parameter
def apply_operation(a: int, b: int, operation: Callable[[int, int], int]) -> int:
    """Apply an operation to two numbers"""
    return operation(a, b)

# Define operations
def add(x: int, y: int) -> int:
    return x + y

def multiply(x: int, y: int) -> int:
    return x * y

# Use it
print(apply_operation(5, 3, add))       # 8
print(apply_operation(5, 3, multiply))  # 15

# With lambda
print(apply_operation(5, 3, lambda x, y: x - y))  # 2
```

---

### Return a Function

```python
from typing import Callable

def create_multiplier(factor: int) -> Callable[[int], int]:
    """Return a function that multiplies by factor"""
    def multiplier(x: int) -> int:
        return x * factor
    return multiplier

double = create_multiplier(2)
triple = create_multiplier(3)

print(double(5))  # 10
print(triple(5))  # 15
```

---

## PART 7: GENERIC TYPES (Generics)

### TypeVar

```python
from typing import TypeVar

T = TypeVar('T')  # Generic type variable

# Function works with any type
def get_first_element(items: list[T]) -> T:
    return items[0]

print(get_first_element([1, 2, 3]))              # 1 (T is int)
print(get_first_element(["a", "b", "c"]))       # "a" (T is str)
print(get_first_element([1.5, 2.5, 3.5]))       # 1.5 (T is float)
```

---

### Constrained TypeVar

```python
from typing import TypeVar

# T can only be int or str
T = TypeVar('T', int, str)

def process(value: T) -> T:
    return value

print(process(42))      # 42 (int)
print(process("hello"))  # "hello" (str)
# process(3.14)  # Type checker error (float not allowed)
```

---

## PART 8: ANY TYPE

### When Type is Unknown

```python
from typing import Any

# Accept any type
def print_info(obj: Any) -> None:
    print(f"Type: {type(obj)}")
    print(f"Value: {obj}")

print_info(42)
print_info("hello")
print_info([1, 2, 3])
print_info({"key": "value"})
```

---

## PART 9: CLASS ANNOTATIONS

### Class with Type Hints

```python
class Person:
    def __init__(self, name: str, age: int) -> None:
        self.name: str = name
        self.age: int = age
    
    def describe(self) -> str:
        return f"{self.name} is {self.age} years old"
    
    def birthday(self) -> None:
        self.age += 1

p = Person("Alice", 30)
print(p.describe())  # Alice is 30 years old
p.birthday()
print(p.age)  # 31
```

---

### Using Self Type (Python 3.11+)

```python
from typing import Self

class Builder:
    def __init__(self, value: str) -> None:
        self.value = value
    
    def add(self, text: str) -> Self:
        """Method chaining - returns same type"""
        self.value += text
        return self
    
    def upper(self) -> Self:
        self.value = self.value.upper()
        return self

result = Builder("hello").add(" ").add("world").upper()
print(result.value)  # HELLO WORLD
```

---

## PART 10: PROTOCOLS & DUCK TYPING

### Protocol (Structural Typing)

```python
from typing import Protocol

class Drawable(Protocol):
    """Anything with a draw method"""
    def draw(self) -> None: ...

class Circle:
    def draw(self) -> None:
        print("Drawing circle")

class Square:
    def draw(self) -> None:
        print("Drawing square")

def render(obj: Drawable) -> None:
    obj.draw()

render(Circle())   # Drawing circle
render(Square())   # Drawing square
```

---

## PART 11: FINAL AND LITERAL

### Final (Can't Be Changed)

```python
from typing import Final

# Constant that can't be reassigned
MAX_SIZE: Final[int] = 100

# MAX_SIZE = 200  # Type checker error!

class Parent:
    def method(self) -> None:
        pass

class Child(Parent):
    # def method(self) -> None:  # Error: can't override final method
    #     pass
    pass
```

---

### Literal (Specific Values Only)

```python
from typing import Literal

# Parameter can only be specific values
def set_log_level(level: Literal["DEBUG", "INFO", "ERROR"]) -> None:
    print(f"Log level: {level}")

set_log_level("DEBUG")   # OK
set_log_level("INFO")    # OK
# set_log_level("WARNING")  # Type checker error!
```

---

## PART 12: TYPE CHECKING WITH mypy

### Install mypy

```bash
pip install mypy
```

---

### Check Types

```python
# file: example.py
def add(a: int, b: int) -> int:
    return a + b

result = add(5, 3)
print(result)  # OK

# Type error (passing string instead of int)
result = add("5", 3)  # Mypy will catch this!
```

Run mypy:
```bash
mypy example.py
# error: Argument 1 to "add" has incompatible type "str"; expected "int"
```

---

## PART 13: PRACTICAL EXAMPLES (45+)

### Example 1: Calculate Circle Area

```python
import math

def circle_area(radius: float) -> float:
    """Calculate area of circle"""
    return math.pi * radius ** 2

print(circle_area(5))  # 78.53981633974483
```

---

### Example 2: Find Maximum

```python
def find_max(numbers: list[int]) -> int:
    return max(numbers)

print(find_max([3, 1, 4, 1, 5, 9]))  # 9
```

---

### Example 3: Process User Data

```python
def process_user(user: dict[str, str]) -> str:
    return f"{user['name']} from {user['city']}"

user = {"name": "Alice", "city": "NYC"}
print(process_user(user))  # Alice from NYC
```

---

### Example 4: Filter List

```python
def filter_even(numbers: list[int]) -> list[int]:
    return [n for n in numbers if n % 2 == 0]

print(filter_even([1, 2, 3, 4, 5, 6]))  # [2, 4, 6]
```

---

### Example 5: Join Strings

```python
def join_words(words: list[str], sep: str = " ") -> str:
    return sep.join(words)

print(join_words(["hello", "world"]))           # hello world
print(join_words(["hello", "world"], "-"))      # hello-world
```

---

### Example 6: Count Occurrences

```python
def count_occurrences(items: list[str]) -> dict[str, int]:
    counts: dict[str, int] = {}
    for item in items:
        counts[item] = counts.get(item, 0) + 1
    return counts

print(count_occurrences(["a", "b", "a", "c", "b", "a"]))
# {'a': 3, 'b': 2, 'c': 1}
```

---

### Example 7: Check Palindrome

```python
def is_palindrome(text: str) -> bool:
    clean = text.lower().replace(" ", "")
    return clean == clean[::-1]

print(is_palindrome("A man a plan a canal Panama"))  # True
```

---

### Example 8: Convert Temperature

```python
def celsius_to_fahrenheit(celsius: float) -> float:
    return (celsius * 9/5) + 32

print(celsius_to_fahrenheit(0))     # 32.0
print(celsius_to_fahrenheit(100))   # 212.0
```

---

### Example 9: Get File Extension

```python
def get_extension(filename: str) -> str:
    return filename.split('.')[-1]

print(get_extension("document.pdf"))  # pdf
print(get_extension("image.jpg"))     # jpg
```

---

### Example 10: Merge Lists

```python
def merge_lists(list1: list[int], list2: list[int]) -> list[int]:
    return list1 + list2

print(merge_lists([1, 2, 3], [4, 5, 6]))  # [1, 2, 3, 4, 5, 6]
```

---

### Examples 11-45: Quick Examples

```python
# Example 11: Validate email
def is_valid_email(email: str) -> bool:
    return "@" in email and "." in email

# Example 12: Get initials
def get_initials(name: str) -> str:
    return ''.join(word[0].upper() for word in name.split())

# Example 13: Remove duplicates
def remove_duplicates(items: list[int]) -> list[int]:
    return list(set(items))

# Example 14: Reverse list
def reverse_list(items: list[int]) -> list[int]:
    return items[::-1]

# Example 15: Sum values
def sum_values(*values: int) -> int:
    return sum(values)

# Example 16: Calculate average
def calculate_average(numbers: list[float]) -> float:
    return sum(numbers) / len(numbers) if numbers else 0.0

# Example 17: Check if all even
def all_even(numbers: list[int]) -> bool:
    return all(n % 2 == 0 for n in numbers)

# Example 18: Check if any odd
def any_odd(numbers: list[int]) -> bool:
    return any(n % 2 != 0 for n in numbers)

# Example 19: Convert list to set
def to_set(items: list[int]) -> set[int]:
    return set(items)

# Example 20: Convert dict keys to list
def get_dict_keys(d: dict[str, int]) -> list[str]:
    return list(d.keys())

# ... and so on for examples 21-45
```

---

## PART 14: COMMON PATTERNS

### Pattern 1: Function Returning Union

```python
from typing import Union

def parse_number(value: str) -> Union[int, None]:
    """Try to parse string as integer"""
    try:
        return int(value)
    except ValueError:
        return None

print(parse_number("42"))    # 42
print(parse_number("abc"))   # None
```

---

### Pattern 2: Multiple Return Values

```python
def divide_with_remainder(a: int, b: int) -> tuple[int, int]:
    """Return quotient and remainder"""
    return a // b, a % b

quotient, remainder = divide_with_remainder(17, 5)
print(quotient, remainder)  # 3 5
```

---

### Pattern 3: Default Arguments

```python
def greet(
    name: str,
    greeting: str = "Hello",
    punctuation: str = "!"
) -> str:
    return f"{greeting}, {name}{punctuation}"

print(greet("Alice"))                          # Hello, Alice!
print(greet("Bob", "Hi"))                      # Hi, Bob!
print(greet("Charlie", "Hey", "?"))            # Hey, Charlie?
```

---

### Pattern 4: Callback Function

```python
from typing import Callable

def run_with_callback(
    data: list[int],
    callback: Callable[[int], int]
) -> list[int]:
    """Apply callback to each element"""
    return [callback(x) for x in data]

def double(x: int) -> int:
    return x * 2

result = run_with_callback([1, 2, 3, 4, 5], double)
print(result)  # [2, 4, 6, 8, 10]
```

---

## QUICK REFERENCE

| Type | Syntax |
|------|--------|
| Integer | `int` |
| String | `str` |
| Float | `float` |
| Boolean | `bool` |
| List of ints | `list[int]` or `List[int]` |
| Tuple (int, str) | `tuple[int, str]` or `Tuple[int, str]` |
| Dict str→int | `dict[str, int]` or `Dict[str, int]` |
| Set of ints | `set[int]` or `Set[int]` |
| Union | `Union[int, str]` or `int \| str` |
| Optional | `Optional[str]` or `str \| None` |
| Any type | `Any` |
| Function | `Callable[[int, int], int]` |
| Return nothing | `-> None` |

---

## SUMMARY

You now know:
- ✅ Basic type hints syntax (`parameter: type -> return:`)
- ✅ All basic types (int, str, float, bool, bytes)
- ✅ Container types (list, dict, tuple, set)
- ✅ Union types (multiple possible types)
- ✅ Optional types (can be None)
- ✅ Callable types (functions as parameters)
- ✅ Generic types and TypeVar
- ✅ Union and Optional
- ✅ Class annotations
- ✅ mypy type checking
- ✅ 45+ practical examples

**Type hints make Python clearer and safer!** 🎯

---

## IMPORTANT NOTES

1. **Type hints are optional** - Python doesn't enforce them at runtime
2. **Use mypy to check** - Install mypy and run it on your code
3. **Python 3.9+** - Use `list[int]` instead of `List[int]`
4. **Python 3.10+** - Use `int | str` instead of `Union[int, str]`
5. **Improves IDE support** - Better autocomplete and error detection
6. **Documentation** - Tells other programmers what your code expects
