# Master Python If Statements - Complete Guide

## 1. Basic If Statement

### What is an If Statement?
An if statement lets your code make decisions. It runs code only if a condition is true.

### Simple Example
```python
age = 18

if age >= 18:
    print("You are an adult")
```

**Output:** `You are an adult`

### Another Example
```python
score = 50

if score > 40:
    print("You passed!")
```

**Output:** `You passed!`

---

## 2. If-Else Statement

### What is Else?
Else runs code when the if condition is false.

### Simple Example
```python
age = 15

if age >= 18:
    print("You can vote")
else:
    print("You cannot vote yet")
```

**Output:** `You cannot vote yet`

### Another Example
```python
number = 10

if number > 20:
    print("Number is big")
else:
    print("Number is small")
```

**Output:** `Number is small`

---

## 3. If-Elif-Else Statement

### What is Elif?
Elif means "else if". It checks another condition if the first one is false.

### Simple Example
```python
marks = 75

if marks >= 90:
    print("Grade: A")
elif marks >= 80:
    print("Grade: B")
elif marks >= 70:
    print("Grade: C")
else:
    print("Grade: F")
```

**Output:** `Grade: C`

### Another Example
```python
temperature = 15

if temperature > 30:
    print("Very Hot")
elif temperature > 20:
    print("Warm")
elif temperature > 10:
    print("Cool")
else:
    print("Cold")
```

**Output:** `Cool`

---

## 4. Comparison Operators

### What are Comparison Operators?
They compare two values and return True or False.

### All Comparison Operators

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| == | Equal to | 5 == 5 | True |
| != | Not equal to | 5 != 3 | True |
| > | Greater than | 5 > 3 | True |
| < | Less than | 5 < 3 | False |
| >= | Greater than or equal to | 5 >= 5 | True |
| <= | Less than or equal to | 3 <= 5 | True |

### Examples
```python
# Equal to
if 10 == 10:
    print("Numbers are equal")
# Output: Numbers are equal

# Not equal to
if 10 != 5:
    print("Numbers are different")
# Output: Numbers are different

# Greater than
if 20 > 15:
    print("20 is bigger")
# Output: 20 is bigger

# Less than
if 5 < 10:
    print("5 is smaller")
# Output: 5 is smaller

# Greater than or equal to
if 10 >= 10:
    print("10 is equal or bigger")
# Output: 10 is equal or bigger

# Less than or equal to
if 5 <= 10:
    print("5 is equal or smaller")
# Output: 5 is equal or smaller
```

---

## 5. Logical Operators (and, or, not)

### What are Logical Operators?
They combine multiple conditions.

### AND Operator
Both conditions must be true.

```python
age = 25
has_license = True

if age >= 18 and has_license == True:
    print("You can drive")
else:
    print("You cannot drive")
```

**Output:** `You can drive`

### OR Operator
At least one condition must be true.

```python
student = True
senior = False

if student == True or senior == True:
    print("You get a discount")
else:
    print("No discount")
```

**Output:** `You get a discount`

### NOT Operator
Reverses the condition (true becomes false, false becomes true).

```python
is_raining = False

if not is_raining:
    print("Go outside and play")
else:
    print("Stay inside")
```

**Output:** `Go outside and play`

### Complex Example
```python
age = 20
has_money = True
is_holiday = False

if age >= 18 and has_money == True and is_holiday == True:
    print("Go to the party")
elif age >= 18 and has_money == True:
    print("Go shopping")
else:
    print("Stay home")
```

**Output:** `Go shopping`

---

## 6. Nested If Statements

### What are Nested If Statements?
An if statement inside another if statement.

### Simple Example
```python
age = 25
has_car = True

if age >= 18:
    print("You are an adult")
    
    if has_car == True:
        print("You have a car")
    else:
        print("You don't have a car")
else:
    print("You are not an adult")
```

**Output:**
```
You are an adult
You have a car
```

### Another Example
```python
score = 85

if score >= 60:
    print("You passed")
    
    if score >= 80:
        print("You passed with distinction")
    elif score >= 70:
        print("You passed with credit")
    else:
        print("You just passed")
else:
    print("You failed")
```

**Output:**
```
You passed
You passed with distinction
```

---

## 7. String Comparison in If Statements

### Comparing Strings
```python
color = "blue"

if color == "blue":
    print("The color is blue")
else:
    print("The color is not blue")
```

**Output:** `The color is blue`

### Case Sensitive
```python
name = "John"

if name == "john":
    print("Match")
else:
    print("No match - case matters!")
```

**Output:** `No match - case matters!`

### Using in Operator
```python
word = "hello"

if "l" in word:
    print("The letter 'l' is in the word")
else:
    print("The letter 'l' is not in the word")
```

**Output:** `The letter 'l' is in the word`

---

## 8. Checking if Variable Exists

### Using is None
```python
x = 10

if x is None:
    print("x is empty")
else:
    print("x has a value")
```

**Output:** `x has a value`

### Another Example
```python
name = None

if name is None:
    print("Name is not set")
else:
    print("Name is:", name)
```

**Output:** `Name is not set`

---

## 9. Checking Boolean Values

### Checking True or False
```python
is_student = True

if is_student:
    print("You are a student")
else:
    print("You are not a student")
```

**Output:** `You are a student`

### Simpler Way
```python
is_student = True

if is_student:
    print("Student")
# Same as: if is_student == True:
```

**Output:** `Student`

### Checking False
```python
is_holiday = False

if not is_holiday:
    print("It's a normal day")
else:
    print("It's a holiday")
```

**Output:** `It's a normal day`

---

## 10. Checking Empty Collections

### Empty List
```python
items = []

if items:
    print("List has items")
else:
    print("List is empty")
```

**Output:** `List is empty`

### Empty Dictionary
```python
person = {}

if person:
    print("Dictionary has data")
else:
    print("Dictionary is empty")
```

**Output:** `Dictionary is empty`

### List with Items
```python
fruits = ["apple", "banana"]

if fruits:
    print("We have fruits:", fruits)
else:
    print("No fruits")
```

**Output:** `We have fruits: ['apple', 'banana']`

---

## 11. Checking Data Types

### Using type()
```python
age = 25

if type(age) == int:
    print("Age is an integer")
else:
    print("Age is not an integer")
```

**Output:** `Age is an integer`

### Another Example
```python
name = "John"

if type(name) == str:
    print("Name is a string")
else:
    print("Name is not a string")
```

**Output:** `Name is a string`

### Using isinstance()
```python
score = 95.5

if isinstance(score, float):
    print("Score is a float")
else:
    print("Score is not a float")
```

**Output:** `Score is a float`

---

## 12. Multiple Conditions in One Line

### Using Parentheses
```python
age = 20
has_license = True

if (age >= 18) and (has_license == True):
    print("Can drive")
```

**Output:** `Can drive`

### Without Too Many Parentheses
```python
x = 10
y = 20

if x > 5 and y < 30:
    print("Both conditions are true")
```

**Output:** `Both conditions are true`

---

## 13. Using in with Lists

### Check if Item is in List
```python
colors = ["red", "green", "blue"]

if "red" in colors:
    print("Red is in the list")
else:
    print("Red is not in the list")
```

**Output:** `Red is in the list`

### Check if Item is NOT in List
```python
numbers = [1, 2, 3, 4, 5]

if 10 not in numbers:
    print("10 is not in the list")
else:
    print("10 is in the list")
```

**Output:** `10 is not in the list`

---

## 14. Ternary Operator (One-Line If-Else)

### What is Ternary Operator?
A short way to write if-else in one line.

### Simple Example
```python
age = 18

status = "Adult" if age >= 18 else "Child"
print(status)
```

**Output:** `Adult`

### Another Example
```python
score = 50

result = "Pass" if score >= 40 else "Fail"
print(result)
```

**Output:** `Pass`

### With Variables
```python
x = 10
y = 20

larger = x if x > y else y
print("Larger number is:", larger)
```

**Output:** `Larger number is: 20`

---

## 15. Practical Examples

### Example 1: Check Login
```python
username = "admin"
password = "1234"

if username == "admin" and password == "1234":
    print("Login successful")
else:
    print("Login failed")
```

**Output:** `Login successful`

### Example 2: Grade Calculator
```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Your grade is: {grade}")
```

**Output:** `Your grade is: B`

### Example 3: Even or Odd
```python
number = 7

if number % 2 == 0:
    print("Even number")
else:
    print("Odd number")
```

**Output:** `Odd number`

### Example 4: Discount Calculator
```python
purchase = 150

if purchase >= 100:
    discount = 10
elif purchase >= 50:
    discount = 5
else:
    discount = 0

print(f"Discount: {discount}%")
```

**Output:** `Discount: 10%`

### Example 5: Age Category
```python
age = 25

if age < 13:
    category = "Child"
elif age < 18:
    category = "Teen"
elif age < 60:
    category = "Adult"
else:
    category = "Senior"

print(f"Category: {category}")
```

**Output:** `Category: Adult`

---

## 16. Common Mistakes to Avoid

### Mistake 1: Using = Instead of ==
```python
# WRONG
if age = 18:
    print("Wrong")

# CORRECT
if age == 18:
    print("Correct")
```

### Mistake 2: Forgetting Colon (:)
```python
# WRONG
if age > 18
    print("Wrong")

# CORRECT
if age > 18:
    print("Correct")
```

### Mistake 3: Wrong Indentation
```python
# WRONG
if age > 18:
print("Wrong")  # Not indented

# CORRECT
if age > 18:
    print("Correct")  # Properly indented
```

### Mistake 4: Comparing String with Number
```python
# WRONG
age = "18"
if age > 18:  # Can't compare string with number
    print("Wrong")

# CORRECT
age = 18
if age > 18:
    print("Correct")
```

### Mistake 5: Using or with Multiple Values Wrong
```python
# WRONG
if color == "red" or "blue":  # Always True!
    print("Wrong")

# CORRECT
if color == "red" or color == "blue":
    print("Correct")
```

---

## 17. Quick Reference

### Basic Structure
```python
if condition:
    # Do something
elif condition:
    # Do something else
else:
    # Do something if all above are false
```

### Operators Cheat Sheet
```python
# Comparison
==  (equal)
!=  (not equal)
>   (greater)
<   (less)
>=  (greater or equal)
<=  (less or equal)

# Logical
and  (both must be true)
or   (at least one must be true)
not  (reverse the condition)

# Membership
in        (is in list/string)
not in    (is not in list/string)

# Identity
is        (is the same object)
is not    (is not the same object)
```

---

## 18. Practice Exercises

### Exercise 1: Traffic Light
```python
light = "green"

if light == "green":
    print("Go")
elif light == "yellow":
    print("Prepare to stop")
elif light == "red":
    print("Stop")
```

### Exercise 2: BMI Calculator
```python
height = 1.75
weight = 70

bmi = weight / (height ** 2)

if bmi < 18.5:
    print("Underweight")
elif bmi < 25:
    print("Normal weight")
elif bmi < 30:
    print("Overweight")
else:
    print("Obese")
```

### Exercise 3: Password Strength
```python
password = "Pass123@"

if len(password) < 6:
    strength = "Weak"
elif len(password) < 10:
    strength = "Medium"
else:
    strength = "Strong"

print(f"Password strength: {strength}")
```

### Exercise 4: Shopping Cart
```python
total = 250
is_member = True

if is_member and total > 200:
    discount = total * 0.1
elif total > 100:
    discount = total * 0.05
else:
    discount = 0

final_price = total - discount
print(f"Final price: {final_price}")
```

### Exercise 5: Temperature Advisory
```python
temperature = 38

if temperature < 0:
    advisory = "Freezing"
elif temperature < 10:
    advisory = "Cold"
elif temperature < 20:
    advisory = "Cool"
elif temperature < 30:
    advisory = "Warm"
else:
    advisory = "Hot"

print(f"Temperature advisory: {advisory}")
```

---

## Summary

You now know:
- ✅ Basic if statements
- ✅ If-else statements
- ✅ If-elif-else statements
- ✅ All comparison operators (==, !=, >, <, >=, <=)
- ✅ Logical operators (and, or, not)
- ✅ Nested if statements
- ✅ String comparison
- ✅ Checking None values
- ✅ Boolean checks
- ✅ Empty collections
- ✅ Data type checking
- ✅ Multiple conditions
- ✅ Using in with lists
- ✅ Ternary operators
- ✅ Real-world examples
- ✅ Common mistakes
- ✅ Practice exercises

**You are now a Python if statement master!** 🎉
