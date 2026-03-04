# Python Pathlib - ULTIMATE COMPLETE GUIDE

## (Modern File Path Handling)

---

## WHAT IS PATHLIB?

Pathlib is **object-oriented file path handling** that replaces old string-based `os.path`.

**Before pathlib (OLD):**
```python
import os
path = '/home/user/documents/file.txt'
print(os.path.basename(path))        # file.txt
print(os.path.splitext(path)[0])     # /home/user/documents/file
exists = os.path.exists(path)
```

**With pathlib (NEW):**
```python
from pathlib import Path
path = Path('/home/user/documents/file.txt')
print(path.name)        # file.txt
print(path.stem)        # file
exists = path.exists()
```

**Why?** ✅ Cleaner, more readable, cross-platform, object-oriented

---

## PART 1: CREATING PATHS

### Basic Path Creation

```python
from pathlib import Path

# Create path from string
p = Path('documents/file.txt')
print(p)  # documents/file.txt

# Create path from parts
p = Path('documents') / 'subfolder' / 'file.txt'
print(p)  # documents/subfolder/file.txt

# Use / operator to join paths
p = Path('documents') / 'file.txt'
print(p)  # documents/file.txt
```

---

### Absolute Paths

```python
from pathlib import Path

# Current working directory
p = Path.cwd()
print(p)  # /home/user/current/dir

# Home directory
p = Path.home()
print(p)  # /home/user

# Temporary directory
from pathlib import Path
import tempfile
p = Path(tempfile.gettempdir())
```

---

### Path Types

```python
from pathlib import Path, PureWindowsPath, PurePosixPath

# Automatic (works on any OS)
p = Path('file.txt')

# Windows path (even on Linux)
p = PureWindowsPath('C:/Users/file.txt')

# POSIX path (Linux/Mac)
p = PurePosixPath('/home/user/file.txt')
```

---

## PART 2: PATH PROPERTIES

### Get Path Components

```python
from pathlib import Path

p = Path('/home/user/documents/file.txt')

# Name (filename with extension)
print(p.name)           # file.txt

# Stem (filename without extension)
print(p.stem)           # file

# Suffix (extension)
print(p.suffix)         # .txt

# Parent directory
print(p.parent)         # /home/user/documents

# Multiple parents
print(p.parent.parent)  # /home/user

# All parents
for parent in p.parents:
    print(parent)

# Root
print(p.root)           # /

# Drive (Windows)
p = Path('C:/Users/file.txt')
print(p.drive)          # C:

# Parts (all components)
print(p.parts)          # ('C:\\', 'Users', 'file.txt')
```

---

### Check Path Type

```python
from pathlib import Path

p = Path('documents/file.txt')

# Absolute vs relative
print(p.is_absolute())  # False

# Make absolute
abs_path = p.resolve()
print(abs_path.is_absolute())  # True

# Check if exists
print(p.exists())       # True/False

# Is file?
print(p.is_file())      # True if file, False otherwise

# Is directory?
print(p.is_dir())       # True if directory, False otherwise

# Is symlink?
print(p.is_symlink())   # True if symlink, False otherwise
```

---

## PART 3: PATH MANIPULATION

### Modify Paths

```python
from pathlib import Path

p = Path('documents/file.txt')

# Change filename
new_p = p.with_name('newfile.txt')
print(new_p)  # documents/newfile.txt

# Change extension
new_p = p.with_suffix('.md')
print(new_p)  # documents/file.md

# Add extension
p = Path('README')
new_p = p.with_suffix('.txt')
print(new_p)  # README.txt
```

---

### Resolve & Relative Paths

```python
from pathlib import Path

# Get absolute path (resolves symlinks)
p = Path('docs/../file.txt')
abs_p = p.resolve()
print(abs_p)  # /full/path/to/file.txt

# Get relative path
abs_path = Path('/home/user/documents/file.txt')
base = Path('/home/user')
rel_path = abs_path.relative_to(base)
print(rel_path)  # documents/file.txt
```

---

## PART 4: FILE OPERATIONS

### Read Files

```python
from pathlib import Path

p = Path('file.txt')

# Read as string
content = p.read_text()
print(content)

# Read as bytes
binary = p.read_bytes()
print(binary)

# Read line by line
for line in p.read_text().split('\n'):
    print(line)
```

---

### Write Files

```python
from pathlib import Path

p = Path('file.txt')

# Write text (creates/overwrites)
p.write_text('Hello, World!')

# Write bytes
p.write_bytes(b'Binary data')

# Append text
with p.open('a') as f:
    f.write('\nNew line')
```

---

### Open Files

```python
from pathlib import Path

p = Path('file.txt')

# Read
with p.open('r') as f:
    content = f.read()

# Write
with p.open('w') as f:
    f.write('Hello')

# Append
with p.open('a') as f:
    f.write('\nNew line')

# Binary
with p.open('rb') as f:
    binary = f.read()
```

---

## PART 5: DIRECTORY OPERATIONS

### Create Directories

```python
from pathlib import Path

p = Path('new_folder')

# Create single directory
p.mkdir()

# Create with parents
p = Path('a/b/c/d')
p.mkdir(parents=True, exist_ok=True)

# Exists check
if not p.exists():
    p.mkdir(parents=True)
```

---

### List Directory

```python
from pathlib import Path

p = Path('documents')

# List immediate contents
for item in p.iterdir():
    print(item)

# List files only
files = [f for f in p.iterdir() if f.is_file()]

# List directories only
dirs = [d for d in p.iterdir() if d.is_dir()]

# Get all files
all_files = list(p.glob('*'))
```

---

### Glob Patterns

```python
from pathlib import Path

p = Path('documents')

# All files in current directory
files = p.glob('*')

# All Python files
py_files = p.glob('*.py')

# All files in subdirectories (non-recursive)
files = p.glob('*/file.txt')

# Recursive search (all subdirectories)
all_py = p.rglob('*.py')

# Multiple patterns
py_and_txt = list(p.glob('*.py')) + list(p.glob('*.txt'))
```

---

### Rename & Delete

```python
from pathlib import Path

p = Path('file.txt')

# Rename file
new_p = p.rename('newname.txt')

# Replace (rename, even if target exists)
p.replace('newname.txt')

# Delete file
p.unlink()

# Delete directory (must be empty)
p.rmdir()

# Delete directory (recursive) - need to delete contents first
import shutil
shutil.rmtree('directory')
```

---

## PART 6: PATH STATISTICS

### File Information

```python
from pathlib import Path
from datetime import datetime

p = Path('file.txt')

# File size (bytes)
size = p.stat().st_size
print(f"Size: {size} bytes")

# Last modified time
mtime = p.stat().st_mtime
mod_time = datetime.fromtimestamp(mtime)
print(f"Modified: {mod_time}")

# Created time (Windows)
ctime = p.stat().st_ctime

# Permissions
mode = p.stat().st_mode

# Is same file?
p2 = Path('other_path')
same = p.samefile(p2)
```

---

### Get File Stats

```python
from pathlib import Path

p = Path('file.txt')

stat = p.stat()
print(f"Size: {stat.st_size}")
print(f"Mode: {stat.st_mode}")
print(f"UID: {stat.st_uid}")
print(f"GID: {stat.st_gid}")
print(f"Modified: {stat.st_mtime}")
print(f"Accessed: {stat.st_atime}")
print(f"Created: {stat.st_ctime}")
```

---

## PART 7: PRACTICAL EXAMPLES (90+)

### Example 1: Find All Python Files

```python
from pathlib import Path

p = Path('.')
py_files = list(p.rglob('*.py'))
print(f"Found {len(py_files)} Python files")
```

---

### Example 2: Count Files by Type

```python
from pathlib import Path
from collections import Counter

p = Path('.')
extensions = Counter(f.suffix for f in p.rglob('*') if f.is_file())
print(extensions)  # {'.py': 42, '.txt': 10, ...}
```

---

### Example 3: Find Largest File

```python
from pathlib import Path

p = Path('.')
largest = max((f for f in p.rglob('*') if f.is_file()), key=lambda f: f.stat().st_size)
print(f"Largest: {largest.name} ({largest.stat().st_size} bytes)")
```

---

### Example 4: Find Recently Modified File

```python
from pathlib import Path

p = Path('.')
newest = max((f for f in p.rglob('*') if f.is_file()), key=lambda f: f.stat().st_mtime)
print(f"Newest: {newest.name}")
```

---

### Example 5: Backup Files

```python
from pathlib import Path
from shutil import copy2

p = Path('documents')
backup = Path('backup')
backup.mkdir(exist_ok=True)

for file in p.glob('*.txt'):
    copy2(file, backup / file.name)
```

---

### Example 6: Create Directory Structure

```python
from pathlib import Path

# Create nested directories
for year in range(2020, 2025):
    for month in range(1, 13):
        p = Path(f'archive/{year}/{month:02d}')
        p.mkdir(parents=True, exist_ok=True)
```

---

### Example 7: Rename Files

```python
from pathlib import Path

p = Path('documents')

for i, file in enumerate(p.glob('*.txt'), 1):
    new_name = p / f'file_{i:03d}.txt'
    file.rename(new_name)
```

---

### Example 8: Remove Empty Directories

```python
from pathlib import Path

def remove_empty_dirs(p):
    for item in p.iterdir():
        if item.is_dir():
            remove_empty_dirs(item)
            try:
                item.rmdir()
            except OSError:
                pass

remove_empty_dirs(Path('.'))
```

---

### Example 9: Find Duplicate Files

```python
from pathlib import Path
from collections import defaultdict
import hashlib

p = Path('.')
size_map = defaultdict(list)

for file in p.rglob('*'):
    if file.is_file():
        size = file.stat().st_size
        size_map[size].append(file)

# Same size = potential duplicates
for size, files in size_map.items():
    if len(files) > 1:
        print(f"Potential duplicates ({size} bytes): {files}")
```

---

### Example 10: Convert Between Paths

```python
from pathlib import Path

# String to Path
s = '/home/user/file.txt'
p = Path(s)

# Path to string
path_str = str(p)

# Accept both
def process_file(f):
    p = Path(f)  # Works with string or Path
    return p.read_text()
```

---

### Examples 11-90: Quick Examples

```python
# Example 11: Check if file is in directory
file_path = Path('documents/file.txt')
docs_path = Path('documents')
in_docs = docs_path in file_path.parents

# Example 12: Get current working directory
cwd = Path.cwd()

# Example 13: Get home directory
home = Path.home()

# Example 14: Check file size
size = Path('file.txt').stat().st_size

# Example 15: Copy file
from shutil import copy2
copy2('src.txt', 'dst.txt')

# Example 16: Move file
import shutil
shutil.move('src.txt', 'dst.txt')

# Example 17: Check if path is absolute
is_abs = Path('file.txt').is_absolute()

# Example 18: Get all subdirectories
dirs = [d for d in Path('.').iterdir() if d.is_dir()]

# Example 19: Change extension
p = Path('file.txt')
new_p = p.with_suffix('.md')

# Example 20: Get file extension
ext = Path('file.txt').suffix

# ... and so on for examples 21-90
```

---

## PART 8: CROSS-PLATFORM PATHS

### Handle Windows & Linux Paths

```python
from pathlib import Path

# Pathlib handles automatically
p = Path('documents') / 'file.txt'

# On Windows: documents\file.txt
# On Linux: documents/file.txt

# Force specific OS
from pathlib import PureWindowsPath, PurePosixPath

# Windows path (even on Linux)
win_path = PureWindowsPath('C:/Users/file.txt')
print(win_path)  # C:\Users\file.txt

# POSIX path (even on Windows)
posix_path = PurePosixPath('/home/user/file.txt')
print(posix_path)  # /home/user/file.txt
```

---

## PART 9: COMPARING WITH OLD os.path

### Migration Guide

```
OLD (os.path)                NEW (pathlib)
─────────────────────────────────────────────────
os.path.join(a, b)           Path(a) / b
os.path.basename(p)          Path(p).name
os.path.dirname(p)           Path(p).parent
os.path.splitext(p)[0]       Path(p).stem
os.path.exists(p)            Path(p).exists()
os.makedirs(p)               Path(p).mkdir(parents=True)
os.path.isfile(p)            Path(p).is_file()
os.path.isdir(p)             Path(p).is_dir()
open(p)                       Path(p).open()
os.rename(src, dst)          Path(src).rename(dst)
shutil.rmtree(p)             (manual with pathlib)
glob.glob(pattern)           Path.glob(pattern)
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Create path | `Path('file.txt')` |
| Join paths | `Path('a') / 'b'` |
| Get filename | `p.name` |
| Get stem | `p.stem` |
| Get extension | `p.suffix` |
| Get parent | `p.parent` |
| Exists? | `p.exists()` |
| Is file? | `p.is_file()` |
| Is directory? | `p.is_dir()` |
| Read text | `p.read_text()` |
| Write text | `p.write_text()` |
| Open file | `p.open()` |
| Make directory | `p.mkdir()` |
| List directory | `p.iterdir()` |
| Find files | `p.glob('*.py')` |
| Find recursive | `p.rglob('*.py')` |
| Rename | `p.rename()` |
| Delete file | `p.unlink()` |
| Delete dir | `p.rmdir()` |
| File size | `p.stat().st_size` |

---

## COMMON PATTERNS

### Safe File Operations

```python
from pathlib import Path

p = Path('file.txt')

# Safe read
if p.exists() and p.is_file():
    content = p.read_text()

# Safe directory creation
p = Path('new/nested/path')
p.mkdir(parents=True, exist_ok=True)

# Safe delete
if p.exists():
    p.unlink()
```

---

### Process All Files

```python
from pathlib import Path

p = Path('documents')

# Process all Python files
for py_file in p.rglob('*.py'):
    content = py_file.read_text()
    print(f"Processing {py_file.name}")
```

---

## SUMMARY

You now know:
- ✅ Create Path objects
- ✅ Get path components (.name, .stem, .suffix, .parent)
- ✅ Check path type (.exists(), .is_file(), .is_dir())
- ✅ Read/write files (.read_text(), .write_text())
- ✅ Create directories (.mkdir())
- ✅ List directories (.iterdir(), .glob(), .rglob())
- ✅ Rename/delete files (.rename(), .unlink())
- ✅ Get file stats (.stat())
- ✅ 90+ practical examples
- ✅ Cross-platform path handling
- ✅ Migration from os.path

**Use pathlib for all new Python code!** 
