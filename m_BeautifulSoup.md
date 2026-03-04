# Beautiful Soup 4 - COMPLETE ULTIMATE GUIDE (Everything!)

---

## INSTALLATION & SETUP

```bash
pip install beautifulsoup4
pip install lxml  # Faster parser (optional)
pip install html5lib  # More lenient parser (optional)
```

---

## PART 1: PARSING & BASICS

### Parse HTML/XML

```python
from bs4 import BeautifulSoup

html = "<html><title>Hello</title><body><p>Hi</p></body></html>"

# Parse with html.parser (built-in)
soup = BeautifulSoup(html, 'html.parser')

# Parse with lxml (faster)
soup = BeautifulSoup(html, 'lxml')

# Parse with html5lib (most lenient)
soup = BeautifulSoup(html, 'html5lib')

# Parse from file
with open('file.html') as f:
    soup = BeautifulSoup(f, 'html.parser')

# Parse from URL
import requests
response = requests.get('https://example.com')
soup = BeautifulSoup(response.content, 'html.parser')
```

### Direct Tag Access

```python
soup.title              # Get first <title>
soup.body               # Get first <body>
soup.p                  # Get first <p>
soup.p.string          # Get text of first <p>
soup.p.text            # Same as .string
soup.p.get_text()      # Same as .string
```

---

## PART 2: SEARCH METHODS (FIND & FILTER)

### `.find()` - Find First Match

```python
# Find by tag name
soup.find('a')
soup.find('div')

# Find by class
soup.find('div', class_='container')

# Find by id
soup.find('div', id='main')

# Find by attribute
soup.find('a', href='example.com')

# Find with multiple attributes
soup.find('a', {'href': 'example.com', 'class': 'link'})

# Find by attribute value dict
soup.find('div', attrs={'data-id': '123'})

# Find with limit
soup.find('a')  # Returns first match

# Find by text/string
soup.find('a', string='Click here')
soup.find('p', text='Hello')
```

---

### `.find_all()` - Find All Matches (Returns List)

```python
# Find all <a> tags
soup.find_all('a')

# Find all with specific class
soup.find_all('div', class_='card')

# Find multiple tags
soup.find_all(['a', 'button'])

# Limit results
soup.find_all('a', limit=5)

# Find by attribute
soup.find_all('div', id='box')

# Find by string
soup.find_all('p', string='Hello')

# Loop through results
for link in soup.find_all('a'):
    print(link.get('href'))
```

---

### `.select()` - CSS Selectors

```python
# By tag
soup.select('a')                    # All <a> tags

# By class
soup.select('.link')                # class="link"
soup.select('a.link')               # <a class="link">

# By id
soup.select('#main')                # id="main"
soup.select('div#main')             # <div id="main">

# By attribute
soup.select('a[href]')              # All <a> with href
soup.select('a[href="example.com"]')  # Specific value

# Child selector
soup.select('div > a')              # Direct children

# Descendant
soup.select('div a')                # All descendants

# Multiple selectors
soup.select('h1, h2, h3')           # Any heading

# Complex
soup.select('div.container a.link')  # <a class="link"> inside <div class="container">
```

---

### `.select_one()` - First CSS Match

```python
soup.select_one('a')                # First <a> tag
soup.select_one('.card')            # First with class="card"
```

---

### Filter Types (Advanced)

```python
import re

# String - exact match
soup.find('a', attrs={'class': 'link'})

# Regular expression
soup.find_all(re.compile('^a$'))    # Tags starting with 'a' (a, article)
soup.find_all(re.compile('link'))   # Tags containing 'link'

# List - match any item
soup.find_all(['a', 'button', 'input'])

# True - match everything
soup.find_all(True)                 # All tags
soup.find_all(name=True)            # All tags

# Function - custom filter
def has_href(tag):
    return tag.has_attr('href')

soup.find_all(has_href)
```

---

## PART 3: NAVIGATION

### Navigate Down (Children)

```python
tag = soup.find('div', class_='container')

# Direct children only
tag.children            # Iterator of direct children
list(tag.children)      # Convert to list
tag.contents            # List of children

# All descendants (recursive)
tag.descendants         # Iterator of all descendants
list(tag.descendants)   # Convert to list

# Loop through children
for child in tag.children:
    print(child)

# Get first child
tag.contents[0]         # First child
tag.contents[-1]        # Last child

# Check if has children
if tag.contents:
    print("Has children")
```

---

### Navigate Up (Parents)

```python
tag = soup.find('a')

# Get parent
tag.parent              # Immediate parent tag
tag.parent.parent       # Grandparent

# Get parent name
tag.parent.name         # 'p', 'div', etc

# Get all parents (up to root)
tag.parents             # Iterator
list(tag.parents)       # List of all parents

# Loop parents
for parent in tag.parents:
    print(parent.name)
```

---

### Navigate Sideways (Siblings)

```python
tag = soup.find_all('p')[0]  # First <p>

# Next sibling
tag.next_sibling        # Next element (might be text/whitespace)
tag.next_sibling.next_sibling  # Skip whitespace

# Previous sibling
tag.previous_sibling    # Previous element

# All next siblings
tag.next_siblings       # Iterator

# All previous siblings
tag.previous_siblings   # Iterator

# Loop siblings
for sibling in tag.next_siblings:
    print(sibling)
```

---

### Navigate by Parsing Order

```python
tag = soup.find('a')

# Next element in parse order
tag.next_element        # Next parsed element
tag.next_element.next_element  # And next

# Previous element
tag.previous_element    # Previous parsed element

# All next elements
tag.next_elements       # Iterator

# All previous elements  
tag.previous_elements   # Iterator
```

---

## PART 4: FIND VARIANTS (Searching from Specific Tag)

### `.find_parent()` & `.find_parents()` - Search Up

```python
tag = soup.find('b')

# Find parent matching criteria
tag.find_parent('div')                 # First parent <div>
tag.find_parent('div', class_='box')   # First parent <div class="box">

# Find all matching parents
tag.find_parents('div')                # All parent <div> tags
tag.find_parents('div', limit=2)       # First 2 matching parents
```

---

### `.find_next()` & `.find_all_next()` - Search Forward

```python
tag = soup.find('a')

# Find next matching element
tag.find_next('p')                     # Next <p> tag
tag.find_next(string='Hello')          # Next text "Hello"

# Find all following matching elements
tag.find_all_next('p')                 # All <p> tags after this one
tag.find_all_next('p', limit=3)        # First 3
```

---

### `.find_previous()` & `.find_all_previous()` - Search Backward

```python
tag = soup.find_all('p')[-1]  # Last <p>

# Find previous matching element
tag.find_previous('div')               # Previous <div>
tag.find_previous(string='Text')       # Previous text

# Find all preceding matching elements
tag.find_all_previous('p')             # All <p> tags before this one
```

---

### `.find_next_sibling()` & `.find_next_siblings()` - Siblings Forward

```python
tag = soup.find('p')

# Find next sibling matching
tag.find_next_sibling('p')             # Next <p> sibling
tag.find_next_sibling('p', class_='intro')  # Specific criteria

# Find all next siblings
tag.find_next_siblings('p')            # All <p> siblings after
tag.find_next_siblings('p', limit=2)   # First 2
```

---

### `.find_previous_sibling()` & `.find_previous_siblings()` - Siblings Backward

```python
tag = soup.find_all('p')[-1]

# Find previous sibling matching
tag.find_previous_sibling('p')         # Previous <p> sibling

# Find all previous siblings
tag.find_previous_siblings('p')        # All <p> siblings before
tag.find_previous_siblings('p', limit=2)  # First 2
```

---

## PART 5: TAG ATTRIBUTES

### Get Attributes

```python
tag = soup.find('a')

# Get single attribute
tag['href']                            # 'http://example.com'
tag.get('href')                        # Same thing
tag.get('href', 'default')             # With default

# Get all attributes
tag.attrs                              # {'href': '...', 'class': 'link'}
dict(tag.attrs)                        # Convert to dict

# Check if has attribute
tag.has_attr('href')                   # True/False
'href' in tag.attrs                    # Same
```

---

### Set/Modify Attributes

```python
tag = soup.find('a')

# Set attribute
tag['href'] = 'new-url.com'
tag.attrs['class'] = 'active'

# Add new attribute
tag['id'] = 'link1'
tag['data-value'] = '123'

# Multi-value attributes
tag['class'] = ['link', 'active']      # class="link active"

# Delete attribute
del tag['class']
del tag.attrs['id']
```

---

### Get Tag Name

```python
tag = soup.find('p')
tag.name                               # 'p'

# Rename tag
tag.name = 'div'                       # Now it's a <div>
```

---

## PART 6: TEXT & STRINGS

### Get Text

```python
tag = soup.find('div')

# Get all text
tag.string                             # First string only
tag.text                               # All text combined
tag.get_text()                         # Same as .text

# With separator
tag.get_text(separator=' | ')          # Join with separator
tag.get_text(separator='\n')           # Join with newlines

# Strip whitespace
tag.get_text(strip=True)               # Remove leading/trailing whitespace

# Generator of text
for text in tag.strings:               # All strings including whitespace
    print(text)

# Stripped strings only
for text in tag.stripped_strings:      # No whitespace strings
    print(text)
```

---

### Access String

```python
tag = soup.find('p')

# If only one string child
tag.string                             # Returns the string
tag.string.strip()                     # Strip whitespace

# If multiple children, .string is None
tag = soup.find('div')
if tag.string:
    print(tag.string)
```

---

## PART 7: MODIFYING THE TREE

### Change Text

```python
tag = soup.find('p')
tag.string = "New text"                # Replace all content
tag.text = "New"                       # Same thing
```

---

### Append Content

```python
from bs4 import NavigableString

tag = soup.find('p')

# Append string
tag.append(" more text")

# Append NavigableString
new_string = NavigableString(" added")
tag.append(new_string)

# Append multiple (extend)
tag.extend([" text1", " text2"])

# Result: <p>Original more text added text1 text2</p>
```

---

### Insert at Position

```python
tag = soup.find('p')

# Insert at index
tag.insert(0, "Start: ")               # At beginning
tag.insert(1, " Middle")               # At position 1

# Insert before/after
tag.insert_before("Before tag")        # Before entire tag
tag.insert_after("After tag")          # After entire tag
```

---

### Create New Tags

```python
soup = BeautifulSoup('', 'html.parser')

# Create tag
new_tag = soup.new_tag('div', attrs={'class': 'container', 'id': 'main'})
new_tag.string = "Content"

# Create string
new_string = soup.new_string("Text content")

# Create comment
from bs4 import Comment
comment = soup.new_string("This is a comment", Comment)

# Append to soup
soup.append(new_tag)
```

---

### Replace Content

```python
tag = soup.find('p')

# Replace tag
new_tag = soup.new_tag('div')
new_tag.string = "Replacement"
tag.replace_with(new_tag)

# Replace children
tag = soup.find('div')
tag.replaceWithChildren()              # Replace <div> with its contents

# Replace_with_children (same thing)
tag.replace_with_children()
```

---

### Remove/Extract Tags

```python
tag = soup.find('p')

# Remove from tree (extract)
tag.extract()                          # Remove and return it

# Decompose (remove completely)
tag.decompose()                        # Remove from tree entirely

# Clear children
tag = soup.find('div')
tag.clear()                            # Remove all children
```

---

### Wrap Content

```python
tag = soup.find('p')
text = tag.string

# Wrap in new tag
text.wrap(soup.new_tag('b'))           # <b>text</b>
tag.wrap(soup.new_tag('div'))          # <div><p>text</p></div>

# Unwrap (remove tag, keep content)
tag.unwrap()                           # Returns the tag, content stays
```

---

### Smooth Out the Tree

```python
from bs4 import NavigableString

tag = soup.find('div')

# Combine adjacent NavigableStrings
tag.smooth()  # Combines "Hello" + " " + "World" into one string

# Useful after modifying tree
tag.append("text1")
tag.append("text2")
tag.smooth()  # Now one combined string
```

---

## PART 8: SPECIAL STRING TYPES

### Comments

```python
from bs4 import Comment

# Find comments
html = "<p><!-- This is a comment --></p>"
soup = BeautifulSoup(html, 'html.parser')

comments = soup.find_all(string=lambda text: isinstance(text, Comment))
for comment in comments:
    print(comment)  # This is a comment

# Create comment
comment = soup.new_string("My comment", Comment)
```

---

### CData

```python
from bs4 import CData

tag = soup.find('data')
tag.string = CData("Text with < and >")
# <data><![CDATA[Text with < and >]]></data>
```

---

### ProcessingInstruction

```python
from bs4 import ProcessingInstruction

pi = soup.new_string("xml version='1.0'", ProcessingInstruction)
# <?xml version='1.0'?>
```

---

### Declaration

```python
from bs4 import Declaration

decl = soup.new_string('DOCTYPE html', Declaration)
# <!DOCTYPE html>
```

---

### Doctype

```python
from bs4 import Doctype

doctype = Doctype.for_name_and_ids('html', None, None)
# <!DOCTYPE html>
```

---

## PART 9: OUTPUT & FORMATTING

### prettify() - Format with Indentation

```python
html = "<div><p>Hello</p><p>World</p></div>"
soup = BeautifulSoup(html, 'html.parser')

# Default (one space indent)
print(soup.prettify())
# <div>
#  <p>
#   Hello
#  </p>
#  <p>
#   World
#  </p>
# </div>

# Custom indentation
print(soup.prettify(' '))  # 4 spaces
print(soup.prettify('\t'))  # Tabs

# On individual tags
print(soup.p.prettify())
```

---

### encode() & decode() - Encoding

```python
soup = BeautifulSoup("<p>Hello</p>", 'html.parser')

# Encode to bytes
encoded = soup.encode()                # UTF-8 bytes
encoded = soup.encode('utf-8')         # Explicit encoding
encoded = soup.encode('ascii')         # ASCII encoding

# Decode (pretty print)
decoded = soup.decode()                # Unicode string
decoded = soup.decode('utf-8')         # Decode to string
```

---

### Output with Formatters

```python
soup = BeautifulSoup('<p>café</p>', 'html.parser')

# minimal (default) - Only escapes necessary chars
soup.prettify(formatter='minimal')     # <p>café</p>

# html - Convert to HTML entities
soup.prettify(formatter='html')        # <p>caf&eacute;</p>

# html5 - HTML5 style (no self-closing tags)
soup.prettify(formatter='html5')       # <br> instead of <br/>

# None - No formatting
soup.prettify(formatter=None)          # Fastest, might be invalid

# Custom formatter
def my_formatter(string):
    return string.upper()

soup.prettify(formatter=my_formatter)   # Custom logic
```

---

## PART 10: ADVANCED FEATURES

### Parsing Only Certain Elements (SoupStrainer)

```python
from bs4 import SoupStrainer

# Only parse <a> tags
strainer = SoupStrainer('a')
soup = BeautifulSoup(html, 'html.parser', parse_only=strainer)

# Only parse tags with certain class
strainer = SoupStrainer(class_='important')
soup = BeautifulSoup(html, 'html.parser', parse_only=strainer)

# Only parse by string content
strainer = SoupStrainer(string=lambda text: len(text) < 20)
soup = BeautifulSoup(html, 'html.parser', parse_only=strainer)

# Complex filter
def has_data_attr(tag):
    return tag.has_attr('data-id')

strainer = SoupStrainer(has_data_attr)
soup = BeautifulSoup(html, 'html.parser', parse_only=strainer)
```

---

### Convert to String

```python
soup = BeautifulSoup("<p>Hello</p>", 'html.parser')

# Convert to string
str(soup)               # "<html><body><p>Hello</p></body></html>"
str(soup.p)             # "<p>Hello</p>"

# Prettified string
soup.prettify()         # With formatting
```

---

### has_attr() - Check Attributes

```python
tag = soup.find('a')

if tag.has_attr('href'):
    print("Has href")

# Same as
if 'href' in tag.attrs:
    print("Has href")
```

---

### index() - Find Position

```python
tag = soup.find('div')
children = list(tag.children)

# Find index of child
p_tag = soup.find('p')
index = tag.index(p_tag)  # Returns position
```

---

### is_empty_element - Check if Self-Closing

```python
br = soup.find('br')
br.is_empty_element        # True

p = soup.find('p')
p.is_empty_element         # False
```

---

## PART 11: PRACTICAL FUNCTIONS (20+)

### Function 1: Extract All Links

```python
def get_all_links(html):
    soup = BeautifulSoup(html, 'html.parser')
    links = []
    for a in soup.find_all('a'):
        links.append({
            'text': a.get_text(strip=True),
            'href': a.get('href', '#')
        })
    return links
```

### Function 2: Extract Table Data

```python
def parse_table(html):
    soup = BeautifulSoup(html, 'html.parser')
    table = soup.find('table')
    data = []
    for row in table.find_all('tr'):
        cells = [cell.get_text(strip=True) for cell in row.find_all(['td', 'th'])]
        data.append(cells)
    return data
```

### Function 3: Remove All Scripts/Styles

```python
def clean_html(html):
    soup = BeautifulSoup(html, 'html.parser')
    for tag in soup.find_all(['script', 'style']):
        tag.decompose()
    return str(soup)
```

### Function 4: Get All Meta Tags

```python
def get_meta_tags(html):
    soup = BeautifulSoup(html, 'html.parser')
    metas = {}
    for meta in soup.find_all('meta'):
        name = meta.get('name', meta.get('property', 'unknown'))
        content = meta.get('content', '')
        metas[name] = content
    return metas
```

### Function 5: Find Broken Links

```python
def find_broken_links(html, timeout=5):
    import requests
    soup = BeautifulSoup(html, 'html.parser')
    broken = []
    for a in soup.find_all('a'):
        url = a.get('href', '')
        if url.startswith('http'):
            try:
                resp = requests.head(url, timeout=timeout)
                if resp.status_code >= 400:
                    broken.append(url)
            except:
                broken.append(url)
    return broken
```

### Function 6: Extract Headings

```python
def get_headings(html):
    soup = BeautifulSoup(html, 'html.parser')
    headings = {}
    for level in range(1, 7):
        tag_name = f'h{level}'
        headings[tag_name] = [h.get_text(strip=True) for h in soup.find_all(tag_name)]
    return headings
```

### Function 7: Find All Images

```python
def get_all_images(html):
    soup = BeautifulSoup(html, 'html.parser')
    images = []
    for img in soup.find_all('img'):
        images.append({
            'src': img.get('src', ''),
            'alt': img.get('alt', ''),
            'title': img.get('title', '')
        })
    return images
```

### Function 8: Extract Text Between Tags

```python
def get_text_after(html, tag_name, class_name=None):
    soup = BeautifulSoup(html, 'html.parser')
    if class_name:
        tag = soup.find(tag_name, class_=class_name)
    else:
        tag = soup.find(tag_name)
    return tag.find_next('p').get_text(strip=True) if tag else None
```

### Function 9: Count Tags

```python
def count_tags(html, tag_name):
    soup = BeautifulSoup(html, 'html.parser')
    return len(soup.find_all(tag_name))
```

### Function 10: Remove Duplicates

```python
def remove_duplicate_tags(html, tag_name):
    soup = BeautifulSoup(html, 'html.parser')
    seen = set()
    for tag in soup.find_all(tag_name):
        text = tag.get_text()
        if text in seen:
            tag.decompose()
        else:
            seen.add(text)
    return str(soup)
```

### Function 11: Extract Form Data

```python
def get_form_fields(html, form_id=None):
    soup = BeautifulSoup(html, 'html.parser')
    form = soup.find('form', id=form_id) if form_id else soup.find('form')
    fields = {}
    for input_tag in form.find_all('input'):
        name = input_tag.get('name')
        value = input_tag.get('value', '')
        if name:
            fields[name] = value
    return fields
```

### Function 12: Get All Text Nodes

```python
def get_all_text(html):
    soup = BeautifulSoup(html, 'html.parser')
    return [str(text).strip() for text in soup.find_all(string=True) if str(text).strip()]
```

### Function 13: Find Specific Attribute Values

```python
def find_by_attr_value(html, attr, value):
    soup = BeautifulSoup(html, 'html.parser')
    results = []
    for tag in soup.find_all(True):  # All tags
        if tag.get(attr) == value:
            results.append(tag)
    return results
```

### Function 14: Extract Nested Data

```python
def get_nested_data(html, parent_tag, child_tag):
    soup = BeautifulSoup(html, 'html.parser')
    data = []
    for parent in soup.find_all(parent_tag):
        children = [child.get_text(strip=True) for child in parent.find_all(child_tag)]
        data.append(children)
    return data
```

### Function 15: Sanitize HTML

```python
def sanitize_html(html, allowed_tags=['p', 'a', 'b', 'i']):
    soup = BeautifulSoup(html, 'html.parser')
    for tag in soup.find_all(True):
        if tag.name not in allowed_tags:
            tag.unwrap()
    return str(soup)
```

### Function 16: Extract Structured Data

```python
def extract_articles(html):
    soup = BeautifulSoup(html, 'html.parser')
    articles = []
    for article in soup.find_all('article'):
        articles.append({
            'title': article.find('h2').get_text(strip=True) if article.find('h2') else '',
            'content': article.find('p').get_text(strip=True) if article.find('p') else '',
            'author': article.find('span', class_='author').get_text(strip=True) if article.find('span', class_='author') else ''
        })
    return articles
```

### Function 17: Convert to Dictionary

```python
def tag_to_dict(tag):
    return {
        'name': tag.name,
        'attrs': tag.attrs,
        'text': tag.get_text(strip=True),
        'children_count': len(list(tag.children))
    }
```

### Function 18: Find Ancestor

```python
def find_ancestor_with_class(tag, class_name):
    for parent in tag.parents:
        if parent.has_attr('class') and class_name in parent.get('class', []):
            return parent
    return None
```

### Function 19: Get Siblings with Filter

```python
def get_siblings_with_tag(tag, tag_name):
    return [sibling for sibling in tag.find_next_siblings(tag_name)]
```

### Function 20: Build Navigation Tree

```python
def build_nav_tree(html):
    soup = BeautifulSoup(html, 'html.parser')
    nav = soup.find('nav')
    tree = {}
    for link in nav.find_all('a'):
        tree[link.get_text()] = link.get('href', '#')
    return tree
```

---

## PART 12: OBJECT TYPES SUMMARY

### BeautifulSoup Object

```python
soup = BeautifulSoup(html, 'html.parser')

soup.name                      # '[document]'
soup.contents                  # List of all top-level children
soup.prettify()                # Format and print
```

### Tag Object

```python
tag = soup.find('p')

tag.name                       # Tag name ('p')
tag.attrs                      # Attributes dict
tag.contents                   # Children
tag.string                     # Single string child
tag.text                       # All text
tag.parent                     # Parent tag
tag.children                   # Children iterator
tag.descendants                # All descendants iterator
```

### NavigableString Object

```python
string = tag.string

str(string)                    # Convert to string
len(string)                    # Length
string.strip()                 # Strip whitespace
string.upper()                 # Uppercase
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Parse HTML | `soup = BeautifulSoup(html, 'html.parser')` |
| Find first | `soup.find('tag')` |
| Find all | `soup.find_all('tag')` |
| CSS select | `soup.select('.class')` |
| Get attribute | `tag['attr']` |
| Get text | `tag.get_text()` |
| Get parent | `tag.parent` |
| Get children | `tag.children` |
| Get siblings | `tag.next_sibling` |
| Append text | `tag.append("text")` |
| Create tag | `soup.new_tag('div')` |
| Remove tag | `tag.decompose()` |
| Format output | `soup.prettify()` |
| Change text | `tag.string = "new"` |
| Change attr | `tag['attr'] = 'value'` |
| Loop find results | `for tag in soup.find_all('a'): ...` |

---

## COMMON PATTERNS

**Get text from all paragraphs:**
```python
for p in soup.find_all('p'):
    print(p.get_text())
```

**Get all links with URLs:**
```python
for link in soup.find_all('a'):
    print(link.get('href'))
```

**Find nested element:**
```python
parent = soup.find('div', class_='container')
child = parent.find('p')
```

**Search by multiple criteria:**
```python
soup.find('div', attrs={'class': 'box', 'id': 'main'})
```

**Remove elements with specific class:**
```python
for tag in soup.find_all(class_='remove-me'):
    tag.decompose()
```

**Loop through table rows:**
```python
for row in soup.find('table').find_all('tr'):
    cells = [cell.get_text() for cell in row.find_all(['td', 'th'])]
```

**Chain methods:**
```python
soup.find('div').find('p').find('a').get('href')
```

---

## THAT'S EVERYTHING!

You now know:
- ✅ Parse HTML/XML from strings, files, URLs
- ✅ Find elements (find, find_all, select)
- ✅ Navigate tree (children, parents, siblings, descendants)
- ✅ Find variants (find_parent, find_next, find_previous, etc)
- ✅ Access/modify attributes and text
- ✅ Modify tree (append, insert, remove, wrap, unwrap)
- ✅ Special string types (comments, CData, etc)
- ✅ Output formatting (prettify, encode, formatters)
- ✅ Advanced features (SoupStrainer, custom functions)
- ✅ 20+ practical functions ready to use

Beautiful Soup mastery achieved!
