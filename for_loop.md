# Python For Loops: Complete Mastery Guide

Master Python for loops from basics to advanced with simple, practical examples and functions.

---

## 1. Basic For Loop Fundamentals

### 1.1 Simple For Loop with Range

**Concept:** Loop through a sequence of numbers using `range()`

```python
# Example 1: Count from 0 to 4
for i in range(5):
    print(i)
# Output: 0 1 2 3 4

# Function to print numbers
def print_numbers(n):
    for i in range(n):
        print(i)

print_numbers(3)  # Output: 0 1 2
```

**Key Points:**
- `range(5)` generates 0, 1, 2, 3, 4 (stops before 5)
- `i` is the loop variable
- Code inside loop must be indented

---

### 1.2 For Loop with Lists

**Concept:** Loop through items in a list one by one

```python
# Example 2: Print fruits
fruits = ["apple", "banana", "orange"]
for fruit in fruits:
    print(fruit)
# Output: apple banana orange

# Function to find longest fruit name
def longest_fruit(fruit_list):
    longest = ""
    for fruit in fruit_list:
        if len(fruit) > len(longest):
            longest = fruit
    return longest

print(longest_fruit(["apple", "watermelon", "kiwi"]))  # watermelon
```

---

### 1.3 For Loop with Strings

**Concept:** Loop through each character in a string

```python
# Example 3: Print each letter
word = "hello"
for letter in word:
    print(letter)
# Output: h e l l o

# Function to count vowels
def count_vowels(text):
    vowels = "aeiou"
    count = 0
    for letter in text:
        if letter.lower() in vowels:
            count += 1
    return count

print(count_vowels("python"))  # 1
```

---

## 2. Range Function Deep Dive

### 2.1 Range with Start and Stop

**Concept:** Control where the loop starts and stops

```python
# Example 4: Count from 2 to 5
for i in range(2, 6):
    print(i)
# Output: 2 3 4 5

# Function to sum numbers in range
def sum_range(start, end):
    total = 0
    for i in range(start, end):
        total += i
    return total

print(sum_range(1, 5))  # 10 (1+2+3+4)
```

---

### 2.2 Range with Step

**Concept:** Skip numbers by using step value

```python
# Example 5: Count by 2s
for i in range(0, 10, 2):
    print(i)
# Output: 0 2 4 6 8

# Function to print odd numbers
def print_odd_numbers(max_num):
    for i in range(1, max_num, 2):
        print(i)

print_odd_numbers(10)  # 1 3 5 7 9
```

---

### 2.3 Range Backwards (Negative Step)

**Concept:** Loop in reverse using negative step

```python
# Example 6: Countdown
for i in range(5, 0, -1):
    print(i)
# Output: 5 4 3 2 1

# Function to reverse numbers
def print_reversed(n):
    for i in range(n, 0, -1):
        print(i)

print_reversed(4)  # 4 3 2 1
```

---

## 3. Loop Index and Enumeration

### 3.1 Using Index with enumerate()

**Concept:** Get both position and item in a list

```python
# Example 7: Print position and value
colors = ["red", "green", "blue"]
for index, color in enumerate(colors):
    print(f"{index}: {color}")
# Output:
# 0: red
# 1: green
# 2: blue

# Function to find item position
def find_position(items, target):
    for index, item in enumerate(items):
        if item == target:
            return index
    return -1

print(find_position(["a", "b", "c"], "b"))  # 1
```

---

### 3.2 Custom Starting Index with enumerate()

**Concept:** Start enumeration from a different number

```python
# Example 8: Start counting from 1
items = ["first", "second", "third"]
for count, item in enumerate(items, start=1):
    print(f"{count}. {item}")
# Output:
# 1. first
# 2. second
# 3. third

# Function to create numbered list
def create_list(items):
    for num, item in enumerate(items, 1):
        print(f"{num}. {item}")

create_list(["Task 1", "Task 2"])
```

---

## 4. Nested For Loops

### 4.1 Simple Nested Loops

**Concept:** Loop inside a loop

```python
# Example 9: Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i}*{j}={i*j}", end=" ")
    print()
# Output:
# 1*1=1 1*2=2 1*3=3
# 2*1=2 2*2=4 2*3=6
# 3*1=3 3*2=6 3*3=9

# Function to create simple pattern
def create_pattern(size):
    for i in range(1, size + 1):
        for j in range(1, i + 1):
            print("*", end="")
        print()

create_pattern(3)
# Output:
# *
# **
# ***
```

---

### 4.2 Nested Loops with Lists

**Concept:** Loop through multiple lists simultaneously

```python
# Example 10: Find all pairs
list1 = [1, 2]
list2 = ["a", "b"]
for num in list1:
    for letter in list2:
        print(f"({num}, {letter})", end=" ")
# Output: (1, a) (1, b) (2, a) (2, b)

# Function to find duplicates in two lists
def find_common(list_a, list_b):
    common = []
    for item_a in list_a:
        for item_b in list_b:
            if item_a == item_b:
                common.append(item_a)
    return common

print(find_common([1, 2, 3], [2, 3, 4]))  # [2, 3]
```

---

## 5. Dictionary Loops

### 5.1 Loop Through Dictionary Keys

**Concept:** Get each key in a dictionary

```python
# Example 11: Print keys
student = {"name": "John", "age": 20, "city": "NYC"}
for key in student:
    print(key)
# Output: name age city

# Function to print all keys
def print_keys(dictionary):
    for key in dictionary:
        print(f"Key: {key}")

print_keys({"a": 1, "b": 2})
```

---

### 5.2 Loop Through Dictionary Values

**Concept:** Get each value in a dictionary

```python
# Example 12: Print values
student = {"name": "John", "age": 20, "city": "NYC"}
for value in student.values():
    print(value)
# Output: John 20 NYC

# Function to sum all numeric values
def sum_values(dictionary):
    total = 0
    for value in dictionary.values():
        if isinstance(value, int):
            total += value
    return total

print(sum_values({"a": 5, "b": 10, "c": 15}))  # 30
```

---

### 5.3 Loop Through Key-Value Pairs

**Concept:** Get both key and value using `.items()`

```python
# Example 13: Print both key and value
student = {"name": "John", "age": 20, "city": "NYC"}
for key, value in student.items():
    print(f"{key}: {value}")
# Output:
# name: John
# age: 20
# city: NYC

# Function to create formatted output
def format_dict(data):
    for key, value in data.items():
        print(f"  {key} = {value}")

format_dict({"python": 95, "java": 87})
```

---

## 6. List Comprehensions

### 6.1 Basic List Comprehension

**Concept:** Create lists using a for loop in one line

```python
# Example 14: Square numbers
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]
print(squares)
# Output: [1, 4, 9, 16, 25]

# Function using list comprehension
def square_list(nums):
    return [n**2 for n in nums]

print(square_list([1, 2, 3]))  # [1, 4, 9]
```

---

### 6.2 List Comprehension with Condition

**Concept:** Filter items while creating a list

```python
# Example 15: Get only even numbers
numbers = [1, 2, 3, 4, 5, 6]
evens = [x for x in numbers if x % 2 == 0]
print(evens)
# Output: [2, 4, 6]

# Function to filter list
def get_evens(nums):
    return [n for n in nums if n % 2 == 0]

print(get_evens([1, 2, 3, 4, 5]))  # [2, 4]
```

---

### 6.3 Nested List Comprehension

**Concept:** Create nested structures using list comprehension

```python
# Example 16: Create 2D list
matrix = [[x + y for x in range(3)] for y in range(3)]
print(matrix)
# Output: [[0, 1, 2], [1, 2, 3], [2, 3, 4]]

# Function to create multiplication table
def times_table(size):
    return [[i*j for j in range(1, size+1)] for i in range(1, size+1)]

print(times_table(3))
# Output: [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

---

## 7. Dictionary and Set Comprehensions

### 7.1 Dictionary Comprehension

**Concept:** Create dictionaries using comprehension

```python
# Example 17: Create key-value pairs
numbers = [1, 2, 3, 4]
squares_dict = {x: x**2 for x in numbers}
print(squares_dict)
# Output: {1: 1, 2: 4, 3: 9, 4: 16}

# Function to create frequency dictionary
def count_chars(text):
    return {char: text.count(char) for char in set(text)}

print(count_chars("hello"))  # {'h': 1, 'e': 1, 'l': 2, 'o': 1}
```

---

### 7.2 Set Comprehension

**Concept:** Create sets using comprehension (no duplicates)

```python
# Example 18: Get unique squared numbers
numbers = [1, 2, 2, 3, 3, 3]
unique_squares = {x**2 for x in numbers}
print(unique_squares)
# Output: {1, 4, 9}

# Function to get unique items
def unique_vowels(text):
    vowels = "aeiou"
    return {char for char in text if char in vowels}

print(unique_vowels("hello"))  # {'e', 'o'}
```

---

## 8. Loop Control Statements

### 8.1 Break Statement

**Concept:** Exit the loop early when condition is met

```python
# Example 19: Stop when found
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num == 3:
        break
    print(num)
# Output: 1 2

# Function to find first match
def find_first(items, target):
    for item in items:
        if item == target:
            return f"Found {target}!"
        print(f"Checking {item}")
    return "Not found"

find_first([1, 2, 3, 4], 3)
```

---

### 8.2 Continue Statement

**Concept:** Skip current iteration and go to next

```python
# Example 20: Skip specific items
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num == 3:
        continue
    print(num)
# Output: 1 2 4 5

# Function to print non-zero numbers
def print_nonzero(nums):
    for num in nums:
        if num == 0:
            continue
        print(num)

print_nonzero([1, 0, 2, 0, 3])
```

---

### 8.3 Else with Loops

**Concept:** Execute code if loop completes without break

```python
# Example 21: Else block
for i in range(3):
    print(i)
else:
    print("Loop completed!")
# Output: 0 1 2 Loop completed!

# Function to check if item exists
def check_item(items, target):
    for item in items:
        if item == target:
            print(f"Found {target}!")
            break
    else:
        print(f"{target} not found")

check_item([1, 2, 3], 2)  # Found 2!
```

---

## 9. Advanced Loop Techniques

### 9.1 Zip Function (Multiple Lists)

**Concept:** Loop through multiple lists together

```python
# Example 22: Pair items from two lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
for name, age in zip(names, ages):
    print(f"{name} is {age}")
# Output:
# Alice is 25
# Bob is 30
# Charlie is 35

# Function to create pairs
def pair_items(list1, list2):
    pairs = []
    for item1, item2 in zip(list1, list2):
        pairs.append((item1, item2))
    return pairs

print(pair_items([1, 2, 3], ['a', 'b', 'c']))
# [(1, 'a'), (2, 'b'), (3, 'c')]
```

---

### 9.2 Reversed Function

**Concept:** Loop through items in reverse order

```python
# Example 23: Loop backwards
items = ["a", "b", "c", "d"]
for item in reversed(items):
    print(item)
# Output: d c b a

# Function to process in reverse
def reverse_sum(numbers):
    total = 0
    for num in reversed(numbers):
        total += num
    return total

print(reverse_sum([1, 2, 3, 4]))  # 10
```

---

### 9.3 Sorted Function

**Concept:** Loop through sorted items

```python
# Example 24: Sort then loop
numbers = [3, 1, 4, 1, 5, 9]
for num in sorted(numbers):
    print(num)
# Output: 1 1 3 4 5 9

# Function to process sorted list
def print_sorted(items):
    for item in sorted(items, reverse=True):
        print(item)

print_sorted([3, 1, 4, 1, 5])
```

---

## 10. Practical Real-World Examples

### 10.1 Process Data

**Concept:** Use loops to transform data

```python
# Function to calculate total and average
def stats(numbers):
    total = 0
    count = 0
    for num in numbers:
        total += num
        count += 1
    average = total / count if count > 0 else 0
    return total, average

result = stats([10, 20, 30, 40])
print(f"Total: {result[0]}, Average: {result[1]}")
# Total: 100, Average: 25.0
```

---

### 10.2 Validate Data

**Concept:** Check conditions in loops

```python
# Function to validate passwords
def check_password_strength(password):
    has_upper = False
    has_lower = False
    has_digit = False
    
    for char in password:
        if char.isupper():
            has_upper = True
        if char.islower():
            has_lower = True
        if char.isdigit():
            has_digit = True
    
    return has_upper and has_lower and has_digit

print(check_password_strength("Abc123"))  # True
print(check_password_strength("abcdef"))  # False
```

---

### 10.3 Search and Filter

**Concept:** Find specific items

```python
# Function to search records
def search_students(students, grade_min):
    results = []
    for student in students:
        if student["grade"] >= grade_min:
            results.append(student["name"])
    return results

students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 75},
    {"name": "Charlie", "grade": 90}
]

print(search_students(students, 80))  # ['Alice', 'Charlie']
```

---

### 10.4 Build Collections

**Concept:** Create new structures from loops

```python
# Function to group items by length
def group_by_length(words):
    groups = {}
    for word in words:
        length = len(word)
        if length not in groups:
            groups[length] = []
        groups[length].append(word)
    return groups

result = group_by_length(["hi", "bye", "cat", "python", "code"])
print(result)
# {2: ['hi'], 3: ['bye', 'cat'], 6: ['python'], 4: ['code']}
```

---

## 11. Common Loop Patterns

### 11.1 Accumulator Pattern

**Concept:** Build a result by adding to it each iteration

```python
# Function to sum numbers
def add_all(numbers):
    total = 0  # Start with initial value
    for num in numbers:
        total += num  # Add each number
    return total

print(add_all([1, 2, 3, 4, 5]))  # 15
```

---

### 11.2 Counter Pattern

**Concept:** Count occurrences

```python
# Function to count matching items
def count_matches(items, target):
    count = 0
    for item in items:
        if item == target:
            count += 1
    return count

print(count_matches([1, 2, 2, 3, 2, 4], 2))  # 3
```

---

### 11.3 Finder Pattern

**Concept:** Search for specific value

```python
# Function to find maximum
def find_max(numbers):
    max_num = numbers[0] if numbers else None
    for num in numbers:
        if num > max_num:
            max_num = num
    return max_num

print(find_max([3, 7, 2, 9, 1]))  # 9
```

---

### 11.4 Filter Pattern

**Concept:** Select only items meeting criteria

```python
# Function to filter items
def get_positive(numbers):
    positive = []
    for num in numbers:
        if num > 0:
            positive.append(num)
    return positive

print(get_positive([-2, 3, -1, 5, -8, 2]))  # [3, 5, 2]
```

---

## 12. Tuples and Multiple Assignment

### 12.1 Loop Through Tuples

**Concept:** Access tuple elements in a loop

```python
# Example: Process coordinates
coordinates = [(1, 2), (3, 4), (5, 6)]
for x, y in coordinates:
    print(f"Point: ({x}, {y})")
# Output:
# Point: (1, 2)
# Point: (3, 4)
# Point: (5, 6)

# Function to calculate distances
def sum_coordinates(coords):
    total = 0
    for x, y in coords:
        total += x + y
    return total

print(sum_coordinates([(1, 2), (3, 4)]))  # 10
```

---

### 12.2 Multiple Assignment in Loops

**Concept:** Unpack multiple values in each iteration

```python
# Function to swap pairs
def reverse_pairs(pairs):
    result = []
    for a, b in pairs:
        result.append((b, a))
    return result

print(reverse_pairs([(1, 2), (3, 4), (5, 6)]))
# [(2, 1), (4, 3), (6, 5)]
```

---

## 13. Performance and Best Practices

### 13.1 Simple vs Complex Loops

**Concept:** Choose efficient approaches

```python
# ❌ Less efficient: Repeated list operations
def slow_copy(items):
    result = []
    for item in items:
        result = result + [item]  # Creates new list each time
    return result

# ✅ More efficient: Direct append
def fast_copy(items):
    result = []
    for item in items:
        result.append(item)  # Add to existing list
    return result

print(fast_copy([1, 2, 3]))  # [1, 2, 3]
```

---

### 13.2 When to Use List Comprehension

**Concept:** Use comprehension when creating lists

```python
# ❌ More verbose
def create_squares_verbose(n):
    result = []
    for i in range(n):
        result.append(i**2)
    return result

# ✅ More concise
def create_squares_concise(n):
    return [i**2 for i in range(n)]

print(create_squares_concise(5))  # [0, 1, 4, 9, 16]
```

---

## 14. Common Mistakes and Fixes

### 14.1 Off-by-One Errors

**Concept:** Remember range() excludes the end value

```python
# ❌ Wrong: range(5) gives 0-4, not 0-5
for i in range(5):
    print(i)  # 0 1 2 3 4

# ✅ Correct: Use range(6) if you want 0-5
for i in range(6):
    print(i)  # 0 1 2 3 4 5

# Function with correct range
def print_to(n):
    for i in range(1, n + 1):
        print(i)

print_to(5)  # 1 2 3 4 5
```

---

### 14.2 Modifying List While Looping

**Concept:** Avoid changing list size during iteration

```python
# ❌ Wrong: Don't modify list while looping
items = [1, 2, 3, 4, 5]
# for item in items:
#     if item > 2:
#         items.remove(item)  # Dangerous!

# ✅ Correct: Create new list or use list comprehension
items = [1, 2, 3, 4, 5]
filtered = [x for x in items if x <= 2]
print(filtered)  # [1, 2]
```

---

### 14.3 Forgetting Indentation

**Concept:** Code inside loop must be indented

```python
# ❌ Wrong: No indentation
# for i in range(3):
# print(i)  # This will cause IndentationError

# ✅ Correct: Proper indentation
for i in range(3):
    print(i)  # Indented correctly
```

---

## 15. Practice Exercises

### Exercise 1: Sum Numbers
```python
# Function to sum list of numbers
def sum_numbers(nums):
    total = 0
    for num in nums:
        total += num
    return total

print(sum_numbers([1, 2, 3, 4, 5]))  # 15
```

---

### Exercise 2: Count Item Occurrences
```python
# Function to count how many times an item appears
def count_item(items, target):
    count = 0
    for item in items:
        if item == target:
            count += 1
    return count

print(count_item(['a', 'b', 'a', 'c', 'a'], 'a'))  # 3
```

---

### Exercise 3: Find Maximum Number
```python
# Function to find largest number
def find_maximum(numbers):
    max_num = numbers[0]
    for num in numbers:
        if num > max_num:
            max_num = num
    return max_num

print(find_maximum([5, 2, 9, 1, 7]))  # 9
```

---

### Exercise 4: Create Frequency Map
```python
# Function to count frequency of each item
def frequency_map(items):
    freq = {}
    for item in items:
        if item in freq:
            freq[item] += 1
        else:
            freq[item] = 1
    return freq

print(frequency_map(['a', 'b', 'a', 'c', 'a']))
# {'a': 3, 'b': 1, 'c': 1}
```

---

### Exercise 5: Flatten Nested List
```python
# Function to flatten 2D list to 1D
def flatten(nested_list):
    result = []
    for sublist in nested_list:
        for item in sublist:
            result.append(item)
    return result

print(flatten([[1, 2], [3, 4], [5, 6]]))
# [1, 2, 3, 4, 5, 6]
```

---

## Summary Checklist

Master Python for loops by understanding:

- ✅ Basic for loop syntax with range, lists, strings
- ✅ Range function variations (start, stop, step)
- ✅ Enumerate for getting index and value
- ✅ Nested loops for complex iterations
- ✅ Dictionary loops (keys, values, items)
- ✅ List comprehensions for concise code
- ✅ Dictionary and set comprehensions
- ✅ Break and continue flow control
- ✅ Else clause with loops
- ✅ Zip for multiple lists
- ✅ Reversed and sorted in loops
- ✅ Real-world patterns and use cases
- ✅ Common mistakes and how to fix them
- ✅ Best practices and performance tips

You now have complete mastery of Python for loops!

---

## Quick Reference

| Topic | Syntax | Purpose |
|-------|--------|---------|
| Basic Loop | `for item in items:` | Iterate through sequence |
| Range | `for i in range(n):` | Loop n times |
| Range with step | `for i in range(0, 10, 2):` | Skip by step size |
| Enumerate | `for i, val in enumerate(list):` | Get index and value |
| Dictionary | `for k, v in dict.items():` | Get key and value |
| Zip | `for a, b in zip(l1, l2):` | Pair items from lists |
| List Comp | `[x for x in items if x > 5]` | Create filtered list |
| Break | `break` | Exit loop early |
| Continue | `continue` | Skip to next iteration |
| Else | `else:` | Execute if loop completes |
