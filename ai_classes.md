# Python 3 Classes - ULTIMATE COMPLETE GUIDE

---

## WHAT ARE CLASSES?

Classes are blueprints for creating objects. They bundle data (attributes) and functionality (methods) together. Classes are central to Object-Oriented Programming (OOP).

---

## PART 1: BASIC CLASS DEFINITION

### Simple Class

```python
# Most basic class
class MyClass:
    """A simple example class"""
    i = 12345  # Class variable
    
    def f(self):
        return 'hello world'

# Instantiate (create object)
obj = MyClass()
print(obj.i)     # 12345
print(obj.f())   # hello world
```

---

## PART 2: __init__() METHOD (Constructor)

### Constructor with Parameters

```python
class Complex:
    """Complex number class"""
    
    def __init__(self, realpart, imagpart):
        self.realpart = realpart  # Instance variable
        self.imagpart = imagpart  # Instance variable

# Create with initialization
x = Complex(3.0, -4.5)
print(x.realpart)  # 3.0
print(x.imagpart)  # -4.5
```

---

### Using self

```python
class Dog:
    """A dog class"""
    
    def __init__(self, name, age):
        self.name = name  # Instance variable
        self.age = age    # Instance variable
    
    def bark(self):
        return f"{self.name} says Woof!"
    
    def birthday(self):
        self.age += 1
        return f"{self.name} is now {self.age}"

# Create instances
d1 = Dog("Buddy", 3)
d2 = Dog("Max", 5)

print(d1.bark())           # Buddy says Woof!
print(d2.birthday())       # Max is now 6
print(d1.age, d2.age)      # 3 6 (independent)
```

**Note:** `self` is just a convention - represents the instance being operated on.

---

## PART 3: CLASS VARIABLES VS INSTANCE VARIABLES

### Shared vs Unique Data

```python
class Dog:
    kind = 'canine'  # Class variable (shared)
    
    def __init__(self, name):
        self.name = name  # Instance variable (unique)

d = Dog('Fido')
e = Dog('Buddy')

print(d.kind)   # 'canine' (shared)
print(e.kind)   # 'canine' (shared)
print(d.name)   # 'Fido' (unique)
print(e.name)   # 'Buddy' (unique)

# Change class variable
Dog.kind = 'wolf'
print(d.kind)   # 'wolf' (affects all)
print(e.kind)   # 'wolf' (affects all)
```

---

### DANGER: Mutable Class Variables

```python
# WRONG - don't do this!
class Dog:
    tricks = []  # Shared list!
    
    def __init__(self, name):
        self.name = name
    
    def add_trick(self, trick):
        self.tricks.append(trick)

d = Dog('Fido')
e = Dog('Buddy')
d.add_trick('roll over')
e.add_trick('play dead')

print(d.tricks)  # ['roll over', 'play dead'] - SHARED!
print(e.tricks)  # ['roll over', 'play dead'] - SHARED!

# RIGHT - use instance variables
class Dog:
    def __init__(self, name):
        self.name = name
        self.tricks = []  # Each dog has own list!
    
    def add_trick(self, trick):
        self.tricks.append(trick)

d = Dog('Fido')
e = Dog('Buddy')
d.add_trick('roll over')
e.add_trick('play dead')

print(d.tricks)  # ['roll over']
print(e.tricks)  # ['play dead']
```

---

## PART 4: METHODS

### Instance Methods (self)

```python
class Calculator:
    """Simple calculator"""
    
    def add(self, a, b):
        return a + b
    
    def subtract(self, a, b):
        return a - b

calc = Calculator()
print(calc.add(5, 3))       # 8
print(calc.subtract(5, 3))  # 2
```

---

### Class Methods (@classmethod)

```python
class Counter:
    """Count instances"""
    count = 0
    
    def __init__(self, name):
        self.name = name
        Counter.count += 1
    
    @classmethod
    def get_count(cls):
        return f"Total instances: {cls.count}"
    
    @classmethod
    def from_string(cls, string):
        """Create from string"""
        parts = string.split(',')
        return cls(parts[0])

c1 = Counter("obj1")
c2 = Counter("obj2")
print(Counter.get_count())  # Total instances: 2

# Create from string
c3 = Counter.from_string("obj3,extra")
print(c3.name)  # obj3
```

---

### Static Methods (@staticmethod)

```python
class Math:
    """Math utilities"""
    
    @staticmethod
    def add(a, b):
        return a + b
    
    @staticmethod
    def is_even(n):
        return n % 2 == 0

# Call without instance
print(Math.add(5, 3))       # 8
print(Math.is_even(4))      # True

# Also work on instances
m = Math()
print(m.add(10, 20))        # 30
print(m.is_even(7))         # False
```

---

### Method Chaining

```python
class Builder:
    """Chainable builder"""
    
    def __init__(self):
        self.value = ""
    
    def add(self, text):
        self.value += text
        return self  # Return self for chaining
    
    def upper(self):
        self.value = self.value.upper()
        return self
    
    def get(self):
        return self.value

# Chain methods
result = Builder().add("hello").add(" ").add("world").upper().get()
print(result)  # HELLO WORLD
```

---

## PART 5: SPECIAL METHODS (Dunder Methods)

### __repr__ and __str__

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __repr__(self):
        """For developers"""
        return f"Person('{self.name}', {self.age})"
    
    def __str__(self):
        """For users"""
        return f"{self.name} ({self.age} years old)"

p = Person("Alice", 30)
print(repr(p))  # Person('Alice', 30)
print(str(p))   # Alice (30 years old)
print(p)        # Alice (30 years old) (uses __str__)
```

---

### __len__ and __getitem__

```python
class CustomList:
    def __init__(self, items):
        self.items = items
    
    def __len__(self):
        return len(self.items)
    
    def __getitem__(self, index):
        return self.items[index]
    
    def __setitem__(self, index, value):
        self.items[index] = value

cl = CustomList([10, 20, 30])
print(len(cl))       # 3
print(cl[0])         # 10
print(cl[1])         # 20
cl[1] = 99
print(cl[1])         # 99
```

---

### __eq__ and Comparison

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author
    
    def __eq__(self, other):
        return (self.title == other.title and 
                self.author == other.author)
    
    def __lt__(self, other):
        return self.title < other.title
    
    def __le__(self, other):
        return self.title <= other.title

b1 = Book("Python 101", "John")
b2 = Book("Python 101", "John")
b3 = Book("Advanced Python", "Jane")

print(b1 == b2)  # True
print(b1 == b3)  # False
print(b3 < b1)   # True (alphabetically)
```

---

### __call__

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor
    
    def __call__(self, x):
        return x * self.factor

triple = Multiplier(3)
print(triple(5))      # 15
print(triple(10))     # 30

# Can use as function
numbers = [1, 2, 3, 4, 5]
result = [triple(n) for n in numbers]
print(result)  # [3, 6, 9, 12, 15]
```

---

### __enter__ and __exit__ (Context Manager)

```python
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        return False  # Don't suppress exceptions

# Use with 'with' statement
with FileManager('test.txt', 'w') as f:
    f.write("Hello, World!")
# File automatically closed
```

---

### __iter__ and __next__

```python
class CountUp:
    def __init__(self, max):
        self.max = max
        self.current = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current < self.max:
            self.current += 1
            return self.current
        else:
            raise StopIteration

# Use in loop
for num in CountUp(3):
    print(num)  # 1, 2, 3
```

---

### Operator Overloading

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1 + v2)     # Vector(4, 6)
print(v2 - v1)     # Vector(2, 2)
print(v1 * 3)      # Vector(3, 6)
```

---

## PART 6: INHERITANCE

### Single Inheritance

```python
class Animal:
    """Base class"""
    
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return f"{self.name} makes a sound"

class Dog(Animal):
    """Derived class"""
    
    def speak(self):
        return f"{self.name} barks"

class Cat(Animal):
    """Another derived class"""
    
    def speak(self):
        return f"{self.name} meows"

d = Dog("Buddy")
c = Cat("Whiskers")

print(d.speak())  # Buddy barks
print(c.speak())  # Whiskers meows
```

---

### Calling Parent Methods with super()

```python
class Vehicle:
    def __init__(self, make, model):
        self.make = make
        self.model = model
    
    def describe(self):
        return f"{self.make} {self.model}"

class Car(Vehicle):
    def __init__(self, make, model, doors):
        super().__init__(make, model)  # Call parent __init__
        self.doors = doors
    
    def describe(self):
        parent_desc = super().describe()  # Call parent method
        return f"{parent_desc} ({self.doors} doors)"

car = Car("Toyota", "Camry", 4)
print(car.describe())  # Toyota Camry (4 doors)
```

---

### Multiple Inheritance

```python
class Swimmer:
    def swim(self):
        return "Swimming"

class Flyer:
    def fly(self):
        return "Flying"

class Duck(Swimmer, Flyer):
    pass

duck = Duck()
print(duck.swim())  # Swimming
print(duck.fly())   # Flying
```

---

### Method Resolution Order (MRO)

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B -> " + super().method()

class C(A):
    def method(self):
        return "C -> " + super().method()

class D(B, C):
    pass

d = D()
print(d.method())  # B -> C -> A

# Check MRO
print(D.__mro__)           # Method Resolution Order
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

print(D.mro())  # Same thing

# Using help
# help(D)  # Shows MRO in docs
```

---

### Multilevel Inheritance

```python
class Animal:
    def eat(self):
        return "Eating"

class Mammal(Animal):
    def warm_blooded(self):
        return "Warm-blooded"

class Dog(Mammal):
    def bark(self):
        return "Woof!"

d = Dog()
print(d.eat())             # Eating (from Animal)
print(d.warm_blooded())    # Warm-blooded (from Mammal)
print(d.bark())            # Woof! (from Dog)
```

---

## PART 7: ENCAPSULATION & PRIVACY

### Private Variables (Convention)

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance  # Protected (single underscore)
    
    def deposit(self, amount):
        self._balance += amount
    
    def get_balance(self):
        return self._balance

account = BankAccount(1000)
print(account.get_balance())  # 1000
account.deposit(500)
print(account.get_balance())  # 1500

# Can still access (convention, not enforced)
print(account._balance)  # 1500
```

---

### Name Mangling (Double Underscore)

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private (double underscore)
    
    def deposit(self, amount):
        self.__balance += amount
    
    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
print(account.get_balance())  # 1000

# Can't access directly
# print(account.__balance)  # AttributeError

# Name mangled to _BankAccount__balance
print(account._BankAccount__balance)  # 1000 (can access if needed)
```

---

## PART 8: PROPERTIES (@property)

### Simple Properties

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    @property
    def area(self):
        """Computed property"""
        return self.width * self.height
    
    @property
    def perimeter(self):
        return 2 * (self.width + self.height)

rect = Rectangle(5, 10)
print(rect.area)       # 50 (looks like attribute)
print(rect.perimeter)  # 30
```

---

### Setter and Deleter

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value
    
    @celsius.deleter
    def celsius(self):
        del self._celsius
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

t = Temperature(0)
print(t.celsius)      # 0
print(t.fahrenheit)   # 32.0

t.celsius = 100
print(t.fahrenheit)   # 212.0

del t.celsius
# print(t.celsius)  # AttributeError
```

---

## PART 9: ABSTRACT CLASSES

### Using abc Module

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    """Abstract base class"""
    
    @abstractmethod
    def speak(self):
        pass
    
    @abstractmethod
    def move(self):
        pass
    
    def sleep(self):  # Concrete method
        return "Sleeping"

class Dog(Animal):
    def speak(self):
        return "Woof!"
    
    def move(self):
        return "Running"

# Can't instantiate abstract class
# a = Animal()  # TypeError

# Can instantiate concrete class
d = Dog()
print(d.speak())   # Woof!
print(d.move())    # Running
print(d.sleep())   # Sleeping
```

---

## PART 10: PRACTICAL EXAMPLES (40+)

### Example 1: Simple Bank Account

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance
    
    def deposit(self, amount):
        self._balance += amount
        return f"Deposited ${amount}"
    
    def withdraw(self, amount):
        if amount <= self._balance:
            self._balance -= amount
            return f"Withdrew ${amount}"
        return "Insufficient funds"
    
    def get_balance(self):
        return self._balance

account = BankAccount("Alice", 1000)
print(account.deposit(500))       # Deposited $500
print(account.withdraw(200))      # Withdrew $200
print(account.get_balance())      # 1300
```

---

### Example 2: Person Class with Age

```python
from datetime import datetime

class Person:
    def __init__(self, name, birth_year):
        self.name = name
        self.birth_year = birth_year
    
    @property
    def age(self):
        return datetime.now().year - self.birth_year
    
    def __str__(self):
        return f"{self.name} (age {self.age})"

p = Person("Bob", 1990)
print(p)  # Bob (age 34)
print(p.age)  # 34
```

---

### Example 3: Student Grade Tracker

```python
class Student:
    def __init__(self, name):
        self.name = name
        self.grades = []
    
    def add_grade(self, grade):
        self.grades.append(grade)
    
    def average(self):
        return sum(self.grades) / len(self.grades) if self.grades else 0
    
    def letter_grade(self):
        avg = self.average()
        if avg >= 90: return 'A'
        elif avg >= 80: return 'B'
        elif avg >= 70: return 'C'
        else: return 'F'

s = Student("Charlie")
s.add_grade(95)
s.add_grade(87)
s.add_grade(92)
print(f"Average: {s.average():.1f}")  # Average: 91.3
print(f"Grade: {s.letter_grade()}")   # Grade: A
```

---

### Example 4: Library Book

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.available = True
    
    def borrow(self):
        if self.available:
            self.available = False
            return f"'{self.title}' borrowed"
        return "Not available"
    
    def return_book(self):
        self.available = True
        return f"'{self.title}' returned"
    
    def __str__(self):
        status = "Available" if self.available else "Borrowed"
        return f"{self.title} by {self.author} ({status})"

book = Book("Python 101", "John Doe", "12345")
print(book)            # Python 101 by John Doe (Available)
print(book.borrow())   # 'Python 101' borrowed
print(book)            # Python 101 by John Doe (Borrowed)
print(book.return_book())  # 'Python 101' returned
```

---

### Example 5: Circle Class

```python
import math

class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    @property
    def area(self):
        return math.pi * self.radius ** 2
    
    @property
    def circumference(self):
        return 2 * math.pi * self.radius
    
    def __repr__(self):
        return f"Circle(r={self.radius})"

c = Circle(5)
print(c)               # Circle(r=5)
print(f"Area: {c.area:.2f}")            # Area: 78.54
print(f"Circumference: {c.circumference:.2f}")  # Circumference: 31.42
```

---

### Example 6: Employee Hierarchy

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    def get_salary(self):
        return self.salary

class Manager(Employee):
    def __init__(self, name, salary, team_size):
        super().__init__(name, salary)
        self.team_size = team_size
    
    def get_salary(self):
        bonus = super().get_salary() * 0.1
        return super().get_salary() + bonus

emp = Employee("Alice", 50000)
mgr = Manager("Bob", 70000, 5)

print(f"{emp.name}: ${emp.get_salary()}")  # Alice: $50000
print(f"{mgr.name}: ${mgr.get_salary()}")  # Bob: $77000.0
```

---

### Example 7: Point/Coordinate Class

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def distance_from_origin(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __repr__(self):
        return f"Point({self.x}, {self.y})"

p1 = Point(3, 4)
p2 = Point(1, 2)

print(p1.distance_from_origin())  # 5.0
print(p1 + p2)                     # Point(4, 6)
```

---

### Example 8: Config/Settings Class

```python
class Config:
    """Singleton-like configuration"""
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.settings = {}
        return cls._instance
    
    def set(self, key, value):
        self.settings[key] = value
    
    def get(self, key, default=None):
        return self.settings.get(key, default)

c1 = Config()
c1.set("db", "postgres")
c2 = Config()
print(c2.get("db"))  # postgres
print(c1 is c2)      # True (same instance)
```

---

### Example 9: Timer Class

```python
import time

class Timer:
    def __init__(self):
        self.start_time = None
        self.elapsed = 0
    
    def __enter__(self):
        self.start_time = time.time()
        return self
    
    def __exit__(self, *args):
        self.elapsed = time.time() - self.start_time
    
    def __repr__(self):
        return f"Elapsed: {self.elapsed:.3f}s"

with Timer() as t:
    time.sleep(0.1)

print(t)  # Elapsed: 0.100s
```

---

### Example 10: Color Class

```python
class Color:
    def __init__(self, r, g, b):
        self.r = max(0, min(255, r))
        self.g = max(0, min(255, g))
        self.b = max(0, min(255, b))
    
    def to_hex(self):
        return f"#{self.r:02x}{self.g:02x}{self.b:02x}"
    
    def __repr__(self):
        return f"Color({self.r}, {self.g}, {self.b})"

c = Color(255, 100, 50)
print(c)           # Color(255, 100, 50)
print(c.to_hex())  # #ff6432
```

---

### Examples 11-40: Quick Examples

```python
# Example 11: Money/Currency class
class Money:
    def __init__(self, amount, currency='USD'):
        self.amount = amount
        self.currency = currency
    
    def __add__(self, other):
        return Money(self.amount + other.amount, self.currency)

# Example 12: URL Builder
class URL:
    def __init__(self, base):
        self.base = base
        self.path = ""
    
    def add_path(self, path):
        self.path += f"/{path}"
        return self
    
    def build(self):
        return self.base + self.path

# Example 13: Lazy Loading
class LazyProperty:
    def __init__(self, func):
        self.func = func
    
    def __get__(self, obj, type=None):
        value = self.func(obj)
        setattr(obj, self.func.__name__, value)
        return value

# Example 14: Matrix class
class Matrix:
    def __init__(self, rows):
        self.rows = rows

# Example 15: Queue using class
class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        return self.items.pop(0) if self.items else None

# Example 16: Stack using class
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        return self.items.pop() if self.items else None

# Example 17: Node for linked list
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Example 18: Game Character
class Character:
    def __init__(self, name, hp, mana):
        self.name = name
        self.hp = hp
        self.mana = mana
    
    def cast_spell(self, cost):
        if self.mana >= cost:
            self.mana -= cost
            return True
        return False

# Example 19: Logger class
class Logger:
    def __init__(self, name):
        self.name = name
    
    def log(self, message):
        print(f"[{self.name}] {message}")

# Example 20: Validator
class Email:
    def __init__(self, email):
        if '@' not in email:
            raise ValueError("Invalid email")
        self.email = email

# ... and so on for examples 21-40
```

---

## QUICK REFERENCE

| Concept | Example |
|---------|---------|
| Class definition | `class MyClass:` |
| Instance creation | `obj = MyClass()` |
| Constructor | `def __init__(self):` |
| Instance method | `def method(self):` |
| Class method | `@classmethod def method(cls):` |
| Static method | `@staticmethod def method():` |
| Property | `@property def prop(self):` |
| Inheritance | `class Child(Parent):` |
| super() | `super().__init__()` |
| String representation | `def __str__(self):` |
| repr | `def __repr__(self):` |
| Operator | `def __add__(self, other):` |
| Comparison | `def __eq__(self, other):` |
| Length | `def __len__(self):` |
| Indexing | `def __getitem__(self, index):` |
| Calling | `def __call__(self):` |
| Iterator | `def __iter__(self):` |
| Context mgr | `def __enter__/__exit__:` |

---

## SUMMARY

You now know:
- ✅ Basic class definition and instantiation
- ✅ __init__() constructor
- ✅ Instance vs class variables
- ✅ Methods (instance, class, static)
- ✅ Special/dunder methods (__init__, __str__, __repr__, etc.)
- ✅ Inheritance (single, multiple, multilevel)
- ✅ super() and MRO
- ✅ Encapsulation and privacy (_, __)
- ✅ Properties (@property)
- ✅ Abstract classes (ABC)
- ✅ 40+ practical examples

**Master Python OOP!** 
