# Python ASCII & STRING Modules - COMPLETE SIMPLE GUIDE

---

## PART 1: ASCII MODULE

### What is ASCII?
Numbers = Characters

```
A = 65, B = 66, a = 97, 0 = 48, space = 32
```

### `ord(char)` - Character to Number

```python
ord('A')      # 65
ord('a')      # 97
ord('0')      # 48
ord(' ')      # 32
```

### `chr(num)` - Number to Character

```python
chr(65)       # A
chr(97)       # a
chr(48)       # 0
chr(32)       # (space)
```

### `ascii(obj)` - Make Text Safe

```python
ascii("Hello")        # 'Hello'
ascii("Hi 👋")        # 'Hi \U0001f44b'
```

### ASCII Quick Values

```
Space = 32
0-9 = 48-57
A-Z = 65-90
a-z = 97-122
```

### ASCII Functions

**Get value:**
```python
def get_ascii(char):
    return ord(char)
get_ascii('A')  # 65
```

**Get character:**
```python
def get_char(num):
    return chr(num)
get_char(65)  # A
```

**Check if ASCII (0-127):**
```python
def is_ascii(char):
    return ord(char) <= 127
is_ascii('A')    # True
is_ascii('😀')   # False
```

**Word to numbers:**
```python
def word_to_numbers(word):
    return [ord(c) for c in word]
word_to_numbers("CAT")  # [67, 65, 84]
```

**Numbers to word:**
```python
def numbers_to_word(nums):
    return "".join([chr(n) for n in nums])
numbers_to_word([67, 65, 84])  # CAT
```

**Shift characters (Caesar cipher):**
```python
def shift(text, amount):
    return "".join([chr(ord(c) + amount) for c in text])
shift("ABC", 1)  # BCD
shift("Hello", 3)  # Khoor
```

**Unshift:**
```python
def unshift(text, amount):
    return "".join([chr(ord(c) - amount) for c in text])
unshift("BCD", 1)  # ABC
```

**Count ASCII vs non-ASCII:**
```python
def count_ascii(text):
    ascii_count = sum(1 for c in text if ord(c) <= 127)
    non_ascii = len(text) - ascii_count
    return ascii_count, non_ascii
count_ascii("Hello 你好")  # (6, 2)
```

**Encrypt/Decrypt:**
```python
def encrypt(text):
    return "".join([chr(ord(c) + 10) for c in text])
def decrypt(text):
    return "".join([chr(ord(c) - 10) for c in text])
encrypt("HELLO")  # YVOOL
decrypt("YVOOL")  # HELLO
```

**Loop alphabet:**
```python
for i in range(65, 91):
    print(chr(i), end=' ')
# A B C D E F... Z
```

**Loop numbers 0-9:**
```python
for i in range(48, 58):
    print(chr(i), end=' ')
# 0 1 2 3 4 5 6 7 8 9
```

---

## PART 2: STRING MODULE

### Import First

```python
import string
```

### String Constants (Pre-made Collections)

**All letters:**
```python
string.ascii_letters
# 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
```

**Lowercase only:**
```python
string.ascii_lowercase
# 'abcdefghijklmnopqrstuvwxyz'
```

**Uppercase only:**
```python
string.ascii_uppercase
# 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
```

**Numbers:**
```python
string.digits
# '0123456789'
```

**Hex digits (0-9, a-f, A-F):**
```python
string.hexdigits
# '0123456789abcdefABCDEF'
```

**Octal digits (0-7):**
```python
string.octdigits
# '01234567'
```

**Punctuation:**
```python
string.punctuation
# '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
```

**Whitespace (space, tab, newline, etc):**
```python
string.whitespace
# ' \t\n\r\x0b\x0c'
```

**Everything printable:**
```python
string.printable
# All letters, digits, punctuation, whitespace combined
```

### String Functions

**Check if letter:**
```python
def is_letter(char):
    return char in string.ascii_letters
is_letter('A')    # True
is_letter('5')    # False
```

**Check if digit:**
```python
def is_digit(char):
    return char in string.digits
is_digit('5')     # True
is_digit('A')     # False
```

**Check if punctuation:**
```python
def is_punct(char):
    return char in string.punctuation
is_punct('!')      # True
is_punct('A')      # False
```

**Check if whitespace:**
```python
def is_ws(char):
    return char in string.whitespace
is_ws(' ')         # True
is_ws('\n')        # True
```

**Check password strength:**
```python
def check_password(pwd):
    has_upper = any(c in string.ascii_uppercase for c in pwd)
    has_lower = any(c in string.ascii_lowercase for c in pwd)
    has_digit = any(c in string.digits for c in pwd)
    return has_upper and has_lower and has_digit
check_password("Pass123")   # True
check_password("password")  # False
```

**Remove punctuation:**
```python
def remove_punct(text):
    return "".join(c for c in text if c not in string.punctuation)
remove_punct("Hello, World!")  # Hello World
```

**Remove whitespace:**
```python
def remove_ws(text):
    return "".join(c for c in text if c not in string.whitespace)
remove_ws("Hello World")  # HelloWorld
```

**Keep only letters and numbers:**
```python
def keep_alphanum(text):
    allowed = string.ascii_letters + string.digits
    return "".join(c for c in text if c in allowed)
keep_alphanum("Hello123!@#")  # Hello123
```

**Valid username (letters, numbers, underscore):**
```python
def is_valid_username(name):
    allowed = string.ascii_letters + string.digits + "_"
    return all(c in allowed for c in name)
is_valid_username("user_123")   # True
is_valid_username("user-123")   # False
```

**Count character types:**
```python
def analyze_text(text):
    return {
        'letters': sum(1 for c in text if c in string.ascii_letters),
        'digits': sum(1 for c in text if c in string.digits),
        'punct': sum(1 for c in text if c in string.punctuation),
        'ws': sum(1 for c in text if c in string.whitespace)
    }
analyze_text("Hello123, World!")
# {'letters': 10, 'digits': 3, 'punct': 2, 'ws': 1}
```

**Generate random password:**
```python
import random
def gen_password(length=12):
    chars = string.ascii_letters + string.digits + string.punctuation
    return "".join(random.choice(chars) for _ in range(length))
gen_password()  # x@9dK#mL2pQ! (random)
```

**Valid hex number:**
```python
def is_hex(text):
    return all(c in string.hexdigits for c in text)
is_hex("FF")     # True
is_hex("GG")     # False
```

**Valid octal number:**
```python
def is_octal(text):
    return all(c in string.octdigits for c in text)
is_octal("77")   # True
is_octal("88")   # False
```

**Extract only numbers:**
```python
def extract_numbers(text):
    return "".join(c for c in text if c in string.digits)
extract_numbers("I have 2 cats and 3 dogs")  # 23
```

**Extract only letters:**
```python
def extract_letters(text):
    return "".join(c for c in text if c in string.ascii_letters)
extract_letters("Hello123World")  # HelloWorld
```

**Capitalize each word:**
```python
def smart_cap(text):
    result = ""
    cap_next = True
    for c in text:
        if c in string.whitespace:
            result += c
            cap_next = True
        elif cap_next and c in string.ascii_letters:
            result += c.upper()
            cap_next = False
        else:
            result += c
    return result
smart_cap("hello world")  # Hello World
```

**All uppercase:**
```python
def all_upper(text):
    return all(c in string.ascii_uppercase or c not in string.ascii_letters for c in text)
all_upper("HELLO")   # True
all_upper("Hello")   # False
```

**All lowercase:**
```python
def all_lower(text):
    return all(c in string.ascii_lowercase or c not in string.ascii_letters for c in text)
all_lower("hello")   # True
all_lower("Hello")   # False
```

**Is alphanumeric:**
```python
def is_alphanumeric(text):
    allowed = string.ascii_letters + string.digits
    return all(c in allowed for c in text)
is_alphanumeric("user123")   # True
is_alphanumeric("user-123")  # False
```

---

## QUICK REFERENCE

### ASCII
| Function | What | Example |
|----------|------|---------|
| `ord(c)` | Char to number | `ord('A')` = 65 |
| `chr(n)` | Number to char | `chr(65)` = A |
| `ascii(x)` | Safe repr | `ascii("Hi 👋")` = safe |

### String Module
| Constant | Contains |
|----------|----------|
| `ascii_letters` | a-z A-Z |
| `ascii_lowercase` | a-z |
| `ascii_uppercase` | A-Z |
| `digits` | 0-9 |
| `hexdigits` | 0-9 a-f A-F |
| `octdigits` | 0-7 |
| `punctuation` | !@#$%^ etc |
| `whitespace` | space tab newline |
| `printable` | everything |

---

## COMMON PATTERNS

**Check if in collection:**
```python
import string
'A' in string.ascii_letters       # True
'5' in string.digits              # True
'!' in string.punctuation         # True
```

**Loop through:**
```python
for char in string.ascii_uppercase:
    print(char, end=' ')
# A B C D E F... Z
```

**Join:**
```python
allowed = string.ascii_letters + string.digits
text = "user123"
clean = "".join(c for c in text if c in allowed)
```

**Check all match:**
```python
import string
all(c in string.digits for c in "12345")  # True
all(c in string.digits for c in "123a5")  # False
```

---

## THAT'S EVERYTHING!

You now know:
- ✅ ASCII: `ord()`, `chr()`, `ascii()`
- ✅ String module: All 9 constants
- ✅ 30+ practical functions ready to use

Copy any function and use it! 🎉
