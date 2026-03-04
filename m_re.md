# Python Regular Expressions (re module) - ULTIMATE COMPLETE GUIDE

---

## WHAT IS REGEX?

Regular Expressions are patterns for matching text. Use the `re` module to find, replace, and manipulate strings.

**Import:**
```python
import re
```

**Always use raw strings (r'...') to avoid backslash escaping issues.**

---

## PART 1: BASIC METACHARACTERS & PATTERNS

### Literal Characters
```python
pattern = r'hello'
re.search(pattern, 'hello world')  # Matches 'hello'
```

### Special Metacharacters
```
.       Any single character (except newline)
^       Start of string
$       End of string
*       0 or more of previous
+       1 or more of previous
?       0 or 1 of previous
|       OR (alternation)
()      Group (capture)
[]      Character class (any one character)
\       Escape special character
```

---

## PART 2: CHARACTER CLASSES

### Basic Classes
```python
[abc]           # a OR b OR c
[a-z]           # Any lowercase letter
[A-Z]           # Any uppercase letter
[0-9]           # Any digit
[a-zA-Z0-9]    # Letters or digits

# Special sequences (same as character classes):
\d              # Digit [0-9]
\D              # Non-digit [^0-9]
\w              # Word char [a-zA-Z0-9_]
\W              # Non-word char
\s              # Whitespace [ \t\n\r\f\v]
\S              # Non-whitespace
\b              # Word boundary
\B              # Non-word boundary

# Negation (inside [])
[^abc]          # NOT a, b, or c
[^0-9]          # Not a digit
```

---

## PART 3: QUANTIFIERS (Repetition)

```python
x*              # 0 or more x
x+              # 1 or more x
x?              # 0 or 1 x (optional)
x{n}            # Exactly n x
x{n,}           # n or more x
x{n,m}          # Between n and m x
x*?             # 0 or more (lazy/non-greedy)
x+?             # 1 or more (lazy)
x??             # 0 or 1 (lazy)

# Examples
re.findall(r'\d+', '123 456')           # ['123', '456'] - one or more digits
re.findall(r'a*', 'aaa bb aa')          # ['aaa', '', '', 'aa', '']
re.findall(r'a{2,4}', 'aa aaa aaaa')    # ['aa', 'aaa', 'aaaa']
```

---

## PART 4: ALTERNATION & GROUPING

### Alternation (|)
```python
cat|dog         # Match 'cat' or 'dog'

re.search(r'cat|dog', 'I have a dog')   # Matches 'dog'
re.search(r'cat|dog', 'I have a cat')   # Matches 'cat'
```

### Groups ()
```python
(abc)+          # Match 'abc' one or more times
(a|b)c          # Match 'ac' or 'bc'

# Capture group (retrieve matched text)
match = re.search(r'(\d+)-(\d+)', '123-456')
match.group()    # '123-456' - entire match
match.group(1)   # '123' - first group
match.group(2)   # '456' - second group
match.groups()   # ('123', '456') - all groups
```

---

## PART 5: ANCHORS & BOUNDARIES

```python
^text           # Text at start of string
text$           # Text at end of string
\b              # Word boundary
\B              # Non-word boundary

# Examples
re.search(r'^hello', 'hello world')     # Match (starts with 'hello')
re.search(r'world$', 'hello world')     # Match (ends with 'world')
re.search(r'\bhello\b', 'hello there')  # Match (whole word 'hello')
```

---

## PART 6: MAIN RE FUNCTIONS

### re.search() - Find First Match

```python
import re

# Returns Match object or None
match = re.search(r'\d+', 'I have 123 apples')
if match:
    print(match.group())     # '123'
    print(match.start())     # 7
    print(match.end())       # 10
    print(match.span())      # (7, 10)
else:
    print('No match')
```

---

### re.match() - Match at Beginning

```python
# Must match at START of string
re.match(r'hello', 'hello world')       # Match
re.match(r'hello', 'say hello world')   # No match (doesn't start with 'hello')

# Same as: re.search(r'^hello', ...)
```

---

### re.findall() - Find All Matches (List)

```python
# Returns list of all matches
matches = re.findall(r'\d+', 'I have 12 apples and 34 oranges')
print(matches)  # ['12', '34']

# With groups
matches = re.findall(r'(\w+):(\d+)', 'name:25 age:30')
print(matches)  # [('name', '25'), ('age', '30')]

# Without groups, returns strings
matches = re.findall(r'\d+', '1 2 3')
print(matches)  # ['1', '2', '3']
```

---

### re.finditer() - Find All Matches (Iterator)

```python
# Returns iterator of Match objects (memory efficient)
for match in re.finditer(r'\d+', '12 34 56'):
    print(match.group())  # '12', then '34', then '56'
    print(match.span())   # (0, 2), then (3, 5), then (6, 8)
```

---

### re.sub() - Replace Matches

```python
# re.sub(pattern, replacement, string)
text = 'I have 12 apples and 34 oranges'
result = re.sub(r'\d+', 'X', text)
print(result)  # 'I have X apples and X oranges'

# With count limit
result = re.sub(r'\d+', 'X', text, count=1)
print(result)  # 'I have X apples and 34 oranges'

# With function (replacement)
def add_one(match):
    return str(int(match.group()) + 1)

result = re.sub(r'\d+', add_one, '10 20 30')
print(result)  # '11 21 31'

# With groups in replacement
result = re.sub(r'(\w+):(\d+)', r'\2-\1', 'name:25 age:30')
print(result)  # '25-name 30-age'
```

---

### re.split() - Split String

```python
# Split on pattern matches
result = re.split(r',', 'apple,banana,orange')
print(result)  # ['apple', 'banana', 'orange']

# Split on whitespace
result = re.split(r'\s+', 'hello   world  test')
print(result)  # ['hello', 'world', 'test']

# With groups (include groups in result)
result = re.split(r'([,;])', 'a,b;c')
print(result)  # ['a', ',', 'b', ';', 'c']

# With limit
result = re.split(r',', 'a,b,c,d', maxsplit=2)
print(result)  # ['a', 'b', 'c,d']
```

---

### re.compile() - Precompile Pattern

```python
# Compile once, use many times
pattern = re.compile(r'\d+')

# Use with pattern object
matches = pattern.findall('123 456 789')
print(matches)  # ['123', '456', '789']

# More efficient for repeated use
for i in range(1000):
    pattern.search(some_string)
```

---

## PART 7: MATCH OBJECT METHODS

```python
match = re.search(r'(\d+)-(\w+)', '123-abc')

# Get matched text
match.group()            # '123-abc' - entire match
match.group(0)           # '123-abc' - same as group()
match.group(1)           # '123' - first group
match.group(2)           # 'abc' - second group
match.groups()           # ('123', 'abc') - all groups

# Get position
match.start()            # 0 - start position
match.end()              # 7 - end position
match.span()             # (0, 7) - tuple of (start, end)

# Get original pattern and string
match.pattern            # Pattern object
match.re                 # Same as pattern
match.string             # '123-abc' - original string
```

---

## PART 8: FLAGS (Modifiers)

```python
# Case-insensitive
re.search(r'hello', 'HELLO', re.IGNORECASE)  # Match
re.search(r'hello', 'HELLO', re.I)           # Same

# Multiline (^ and $ match line start/end)
text = 'hello\nworld'
re.search(r'^world', text)                   # No match
re.search(r'^world', text, re.MULTILINE)    # Match

# Dotall (. matches newline too)
re.search(r'a.b', 'a\nb')                   # No match
re.search(r'a.b', 'a\nb', re.DOTALL)       # Match

# ASCII (only ASCII matching)
re.search(r'\w+', 'café', re.ASCII)         # Matches 'caf' only

# VERBOSE (allow whitespace and comments in pattern)
pattern = re.compile(r'''
    \d+      # digits
    -        # dash
    \w+      # word chars
''', re.VERBOSE)

# Combine flags
re.search(r'hello', 'HELLO', re.I | re.MULTILINE)
```

---

## PART 9: LOOKAHEADS & LOOKBEHINDS (Advanced)

### Lookahead (?=...) - Positive
```python
# Match 'hello' only if followed by ' world'
re.search(r'hello(?= world)', 'hello world')      # Match
re.search(r'hello(?= world)', 'hello there')      # No match

# Match digits only if followed by 'px'
re.findall(r'\d+(?=px)', '10px 20em 30px')       # ['10', '30']
```

### Negative Lookahead (?!...)
```python
# Match 'hello' only if NOT followed by ' world'
re.search(r'hello(?! world)', 'hello there')      # Match
re.search(r'hello(?! world)', 'hello world')      # No match
```

### Lookbehind (?<=...) - Positive
```python
# Match '$100' only if preceded by 'Price:'
re.search(r'(?<=Price:)\$\d+', 'Price:$100')    # Match
re.search(r'(?<=Price:)\$\d+', 'Cost:$100')     # No match
```

### Negative Lookbehind (?<!...)
```python
# Match '$100' only if NOT preceded by 'Price:'
re.search(r'(?<!Price:)\$\d+', 'Cost:$100')     # Match
re.search(r'(?<!Price:)\$\d+', 'Price:$100')    # No match
```

---

## PART 10: PRACTICAL EXAMPLES (30+)

### Example 1: Extract Email

```python
email_pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
text = 'Contact me at john@example.com or jane_doe@test.co.uk'
emails = re.findall(email_pattern, text)
print(emails)  # ['john@example.com', 'jane_doe@test.co.uk']
```

---

### Example 2: Extract Phone Numbers

```python
phone_pattern = r'\(\d{3}\) \d{3}-\d{4}'
text = 'Call (123) 456-7890 or (987) 654-3210'
phones = re.findall(phone_pattern, text)
print(phones)  # ['(123) 456-7890', '(987) 654-3210']
```

---

### Example 3: Remove HTML Tags

```python
text = '<p>Hello <b>world</b>!</p>'
clean = re.sub(r'<[^>]+>', '', text)
print(clean)  # 'Hello world!'
```

---

### Example 4: Validate Strong Password

```python
def is_strong_password(pwd):
    pattern = r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$'
    return bool(re.match(pattern, pwd))

print(is_strong_password('Weak123'))      # False (no special char)
print(is_strong_password('Strong@123'))   # True
```

---

### Example 5: Extract URLs

```python
url_pattern = r'https?://[^\s]+'
text = 'Visit https://example.com or http://test.org for more info'
urls = re.findall(url_pattern, text)
print(urls)  # ['https://example.com', 'http://test.org']
```

---

### Example 6: Replace Date Format

```python
text = 'Dates: 2024-01-15, 2024-03-20, 2024-12-25'
result = re.sub(r'(\d{4})-(\d{2})-(\d{2})', r'\3/\2/\1', text)
print(result)  # 'Dates: 15/01/2024, 20/03/2024, 25/12/2024'
```

---

### Example 7: Extract Numbers

```python
text = 'Prices: $100, $25.50, $1000'
numbers = re.findall(r'\$\d+\.?\d*', text)
print(numbers)  # ['$100', '$25.50', '$1000']
```

---

### Example 8: Split by Multiple Delimiters

```python
text = 'apple,banana;orange:grape'
result = re.split(r'[,;:]', text)
print(result)  # ['apple', 'banana', 'orange', 'grape']
```

---

### Example 9: Remove Extra Whitespace

```python
text = 'Hello    world   test'
result = re.sub(r'\s+', ' ', text).strip()
print(result)  # 'Hello world test'
```

---

### Example 10: Validate IP Address

```python
ip_pattern = r'^(\d{1,3}\.){3}\d{1,3}$'
print(re.match(ip_pattern, '192.168.1.1'))      # Match
print(re.match(ip_pattern, '256.1.1.1'))        # Match (not perfect validation)
```

---

### Example 11: Extract Words

```python
text = 'Hello, world! How are you?'
words = re.findall(r'\b\w+\b', text)
print(words)  # ['Hello', 'world', 'How', 'are', 'you']
```

---

### Example 12: Camel Case to Snake Case

```python
text = 'camelCaseVariable'
result = re.sub(r'([a-z])([A-Z])', r'\1_\2', text).lower()
print(result)  # 'camel_case_variable'
```

---

### Example 13: Match Whole Words Only

```python
text = 'the theme theater'
matches = re.findall(r'\bthe\b', text)
print(matches)  # ['the'] (not 'the' in 'theme' or 'theater')
```

---

### Example 14: Extract Hashtags

```python
text = 'Check out #python #coding #regex for tips!'
hashtags = re.findall(r'#\w+', text)
print(hashtags)  # ['#python', '#coding', '#regex']
```

---

### Example 15: Match IPv4 Address (Better)

```python
text = '192.168.1.1 and 256.1.1.1'
# More accurate (0-255)
pattern = r'\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b'
matches = re.findall(pattern, text)
print(matches)  # ['192.168.1.1']
```

---

### Example 16: Extract Credit Card (Masked)

```python
cards = ['1234-5678-9012-3456', '4111 1111 1111 1111']
result = re.sub(r'(\d{4})[- ]?(\d{4})[- ]?(\d{4})[- ]?(\d{4})',
                r'****-****-****-\4', ''.join(cards))
```

---

### Example 17: Remove Consecutive Duplicates

```python
text = 'aabbccddee'
result = re.sub(r'(.)\1+', r'\1', text)
print(result)  # 'abcde'
```

---

### Example 18: Check for Palindrome

```python
def is_palindrome(s):
    clean = re.sub(r'[^a-z0-9]', '', s.lower())
    return clean == clean[::-1]

print(is_palindrome('A man, a plan, a canal: Panama'))  # True
```

---

### Example 19: Extract All Links from HTML

```python
html = '<a href="https://example.com">Link 1</a><a href="https://test.org">Link 2</a>'
links = re.findall(r'href=["\'](https?://[^"\']+)', html)
print(links)  # ['https://example.com', 'https://test.org']
```

---

### Example 20: Match Hex Color

```python
hex_pattern = r'#(?:[0-9a-fA-F]{3}){1,2}'
colors = re.findall(hex_pattern, '#fff #123456 #abcdefg')
print(colors)  # ['#fff', '#123456']
```

---

### Example 21: Extract Version Numbers

```python
text = 'Version 1.2.3, v2.0.1, Version 3.5'
versions = re.findall(r'(?:v|Version)\s*(\d+\.\d+(?:\.\d+)?)', text)
print(versions)  # ['1.2.3', '2.0.1', '3.5']
```

---

### Example 22: Split on Uppercase

```python
text = 'helloWorldTest'
result = re.split(r'(?=[A-Z])', text)
print(result)  # ['hello', 'World', 'Test']
```

---

### Example 23: Match Nested Parentheses (Limited)

```python
text = 'func(arg1, arg2(nested))'
# Match simple cases
pattern = r'\([^)]*\)'
matches = re.findall(pattern, text)
print(matches)  # ['(arg1, arg2(nested))'] might not work perfectly
```

---

### Example 24: Replace Multiple Spaces

```python
text = 'Hello     world    test'
result = re.sub(r' {2,}', ' ', text)
print(result)  # 'Hello world test'
```

---

### Example 25: Username Validation

```python
def valid_username(name):
    return bool(re.match(r'^[a-zA-Z0-9_]{3,16}$', name))

print(valid_username('user_123'))    # True
print(valid_username('ab'))          # False (too short)
```

---

### Example 26: Extract Domain from URL

```python
url = 'https://www.example.com/path?query=value'
domain = re.search(r'https?://(?:www\.)?([^/]+)', url)
print(domain.group(1))  # 'example.com'
```

---

### Example 27: Match Currency Amount

```python
pattern = r'\$\d+(?:,\d{3})*(?:\.\d{2})?'
text = 'Prices: $100, $1,000, $2,500.99, $50000'
matches = re.findall(pattern, text)
print(matches)  # ['$100', '$1,000', '$2,500.99']
```

---

### Example 28: Case-Insensitive Search & Replace

```python
text = 'Hello HELLO hello HeLLo'
result = re.sub(r'hello', 'hi', text, flags=re.IGNORECASE)
print(result)  # 'hi hi hi hi'
```

---

### Example 29: Remove Accents

```python
import unicodedata
text = 'café naïve résumé'
result = ''.join(c for c in unicodedata.normalize('NFD', text)
                 if unicodedata.category(c) != 'Mn')
print(result)  # 'cafe naive resume'
```

---

### Example 30: Match Lines Starting with Pattern

```python
text = '''name: John
age: 30
name: Jane
city: NYC'''

# Multiline match
names = re.findall(r'^name: (.+)', text, re.MULTILINE)
print(names)  # ['John', 'Jane']
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Search | `re.search(pattern, string)` |
| Match (start) | `re.match(pattern, string)` |
| Find all | `re.findall(pattern, string)` |
| Find all (iterator) | `re.finditer(pattern, string)` |
| Replace | `re.sub(pattern, repl, string)` |
| Split | `re.split(pattern, string)` |
| Compile | `re.compile(pattern)` |
| Case-insensitive | `re.IGNORECASE` or `re.I` |
| Multiline | `re.MULTILINE` or `re.M` |
| Dotall | `re.DOTALL` or `re.S` |
| Get match | `match.group()` |
| Get groups | `match.groups()` |
| Get position | `match.span()` |

---

## COMMON PATTERNS CHEAT SHEET

```
Email:           r'^[\w\.-]+@[\w\.-]+\.\w+$'
Phone:           r'(\+\d{1,3})?\s?\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}'
URL:             r'https?://[^\s]+'
IP Address:      r'(?:[0-9]{1,3}\.){3}[0-9]{1,3}'
Date (YYYY-MM-DD): r'\d{4}-\d{2}-\d{2}'
Time (HH:MM:SS): r'\d{2}:\d{2}:\d{2}'
Username:        r'^[a-zA-Z0-9_]{3,16}$'
Strong Password: r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$'
Hex Color:       r'#(?:[0-9a-fA-F]{3}){1,2}'
```

---

## THAT'S EVERYTHING!

You now know:
- ✅ Basic regex patterns & metacharacters
- ✅ Character classes & quantifiers
- ✅ Alternation & grouping
- ✅ Anchors & boundaries
- ✅ All main re functions (search, match, findall, finditer, sub, split, compile)
- ✅ Match object methods
- ✅ Flags & modifiers
- ✅ Lookaheads & lookbehinds (advanced)
- ✅ 30+ practical examples

Regular expressions mastery achieved! 
