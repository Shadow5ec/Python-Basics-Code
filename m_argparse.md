# Argparse - ULTIMATE COMPLETE GUIDE (Everything!)

---

## WHAT IS ARGPARSE?

Argparse parses command-line arguments and automatically generates help messages. It makes CLI tools professional and user-friendly.

**Install:** Built-in, no installation needed!

```bash
python script.py -h              # Show help
python script.py --help          # Same thing
```

---

## PART 1: BASICS

### Create Parser

```python
import argparse

# Create parser
parser = argparse.ArgumentParser()

# Parse arguments
args = parser.parse_args()
```

### Add Positional Argument (Required)

```python
parser = argparse.ArgumentParser()

# Required positional argument
parser.add_argument('name', help='Your name')

args = parser.parse_args()
print(args.name)  # Prints: John

# Usage: python script.py John
```

### Add Optional Argument (Flags)

```python
parser = argparse.ArgumentParser()

# Optional argument
parser.add_argument('-v', '--verbose', action='store_true', help='Enable verbose')

args = parser.parse_args()
if args.verbose:
    print("Verbose mode on")

# Usage: python script.py -v
# Usage: python script.py --verbose
```

---

## PART 2: ARGUMENT OPTIONS

### `action` - What to Do with the Argument

```python
# store (default) - Store the value
parser.add_argument('-n', '--name')  # Stores string

# store_true - Store True if present
parser.add_argument('-v', '--verbose', action='store_true')  # args.verbose = True/False

# store_false - Store False if present
parser.add_argument('--no-cache', action='store_false')  # Opposite of store_true

# store_const - Store constant value
parser.add_argument('--flag', action='store_const', const=42)  # args.flag = 42

# append - Collect multiple values in list
parser.add_argument('-n', '--name', action='append')
# Usage: -n Alice -n Bob  →  args.name = ['Alice', 'Bob']

# append_const - Append constant to list
parser.add_argument('--tag', action='append_const', const='important')
# Usage: --tag --tag  →  args.tag = ['important', 'important']

# count - Count how many times flag appears
parser.add_argument('-v', '--verbose', action='count', default=0)
# Usage: -vvv  →  args.verbose = 3

# help - Show help (automatic)
parser.add_argument('-h', '--help', action='help')

# version - Show version
parser.add_argument('--version', action='version', version='1.0')
```

---

### `type` - Convert Argument Type

```python
# String (default)
parser.add_argument('name')  # String

# Integer
parser.add_argument('-n', type=int, help='A number')

# Float
parser.add_argument('-f', type=float, help='A decimal')

# File object
parser.add_argument('input', type=argparse.FileType('r'))  # Open for reading

# Custom type function
def positive_int(value):
    ivalue = int(value)
    if ivalue <= 0:
        raise argparse.ArgumentTypeError(f"{value} is not positive")
    return ivalue

parser.add_argument('count', type=positive_int)
```

---

### `default` - Default Value

```python
parser.add_argument('-n', '--name', default='Guest')

# If no argument given:
args = parser.parse_args([])
print(args.name)  # 'Guest'

# If argument given:
args = parser.parse_args(['-n', 'Alice'])
print(args.name)  # 'Alice'
```

---

### `required` - Make Optional Argument Required

```python
# Optional argument that's required!
parser.add_argument('-i', '--input', required=True, help='Input file')

# Must provide:
# python script.py -i file.txt
```

---

### `choices` - Restrict to Specific Values

```python
parser.add_argument('-m', '--mode', choices=['fast', 'slow', 'normal'], help='Mode')

# Usage:
# python script.py -m fast   ✓
# python script.py -m slow   ✓
# python script.py -m other  ✗ Error!
```

---

### `nargs` - Number of Arguments

```python
# nargs='?' - 0 or 1 argument
parser.add_argument('-o', '--output', nargs='?', const='default.txt')
# -o           →  args.output = 'default.txt'
# -o file.txt  →  args.output = 'file.txt'

# nargs='*' - 0 or more arguments (list)
parser.add_argument('files', nargs='*')
# file1 file2  →  args.files = ['file1', 'file2']
# (none)        →  args.files = []

# nargs='+' - 1 or more arguments (list)
parser.add_argument('files', nargs='+')
# file1 file2  →  args.files = ['file1', 'file2']
# (none)        →  Error!

# nargs=N - Exactly N arguments
parser.add_argument('coords', nargs=3, type=float)  # Exactly 3 floats
# 1.0 2.0 3.0  →  args.coords = [1.0, 2.0, 3.0]

# nargs=argparse.REMAINDER - All remaining arguments
parser.add_argument('command', nargs=argparse.REMAINDER)
```

---

### `dest` - Change Attribute Name

```python
parser.add_argument('-v', '--verbose', dest='verbosity', action='store_true')

args = parser.parse_args(['-v'])
print(args.verbosity)  # True (not args.verbose)
```

---

### `metavar` - Name in Help Message

```python
parser.add_argument('name', metavar='USERNAME')
parser.add_argument('-n', metavar='NUM', type=int)

# Help shows:
# positional arguments:
#   USERNAME     
# optional arguments:
#   -n NUM
```

---

### `help` - Help Text

```python
parser.add_argument('input', help='Input file path')
parser.add_argument('-v', '--verbose', help='Enable verbose mode')

# Shows in help:
# positional arguments:
#   input          Input file path
# optional arguments:
#   -v, --verbose  Enable verbose mode
```

---

## PART 3: PARSER CONFIGURATION

### Description & Epilog

```python
parser = argparse.ArgumentParser(
    description='Process files and convert them',
    epilog='Examples: script.py input.txt -o output.txt'
)

# Help shows:
# usage: ...
# Process files and convert them
# ...
# Examples: script.py input.txt -o output.txt
```

---

### prog - Program Name

```python
parser = argparse.ArgumentParser(prog='myapp')

# Help shows:
# usage: myapp [-h] ...
```

---

### prefix_chars - Change Prefix from `-`

```python
parser = argparse.ArgumentParser(prefix_chars='-+')
parser.add_argument('+verbose', action='store_true')

# Usage: python script.py +verbose
```

---

### add_help - Auto Help (Default: True)

```python
parser = argparse.ArgumentParser(add_help=False)
# No automatic -h/--help added
```

---

### allow_abbrev - Allow Abbreviated Options

```python
# Default: True
parser.add_argument('--verbose')
# python script.py --verb  ✓ Works (if unambiguous)

# Disable:
parser = argparse.ArgumentParser(allow_abbrev=False)
# python script.py --verb  ✗ Error
```

---

### formatter_class - Format Help Output

```python
# Default (ArgumentsDefaultsHelpFormatter added in 3.10+)
parser = argparse.ArgumentParser()

# Show defaults in help
parser = argparse.ArgumentParser(formatter_class=argparse.ArgumentDefaultsHelpFormatter)

# More compact
parser = argparse.ArgumentParser(formatter_class=argparse.RawDescriptionHelpFormatter)

# Raw text (no wrapping)
parser = argparse.ArgumentParser(formatter_class=argparse.RawTextHelpFormatter)

# Shorter help
parser = argparse.ArgumentParser(formatter_class=argparse.CompactHelpFormatter)
```

---

## PART 4: ADVANCED FEATURES

### Argument Groups (Organize Arguments)

```python
parser = argparse.ArgumentParser()

# Create groups
input_group = parser.add_argument_group('input options')
output_group = parser.add_argument_group('output options')

# Add to groups
input_group.add_argument('-i', '--input', help='Input file')
output_group.add_argument('-o', '--output', help='Output file')

# Help shows organized by group:
# input options:
#   -i, --input   Input file
# output options:
#   -o, --output  Output file
```

---

### Mutually Exclusive Arguments

```python
parser = argparse.ArgumentParser()

# Only one can be used
group = parser.add_mutually_exclusive_group()
group.add_argument('-a', action='store_true')
group.add_argument('-b', action='store_true')

# Usage:
# python script.py -a     ✓
# python script.py -b     ✓
# python script.py -a -b  ✗ Error!
```

---

### Subcommands (Subparsers)

```python
parser = argparse.ArgumentParser(description='Tool with subcommands')

# Create subparsers
subparsers = parser.add_subparsers(dest='command')

# Add subcommand: add
add_parser = subparsers.add_parser('add', help='Add items')
add_parser.add_argument('x', type=int)
add_parser.add_argument('y', type=int)
add_parser.set_defaults(func=lambda args: print(args.x + args.y))

# Add subcommand: multiply
mult_parser = subparsers.add_parser('multiply', help='Multiply items')
mult_parser.add_argument('x', type=int)
mult_parser.add_argument('y', type=int)
mult_parser.set_defaults(func=lambda args: print(args.x * args.y))

# Parse and execute
args = parser.parse_args()
if hasattr(args, 'func'):
    args.func(args)

# Usage:
# python script.py add 2 3        → 5
# python script.py multiply 2 3   → 6
```

---

### Parent Parsers (Shared Arguments)

```python
# Create parent parser with shared args
parent = argparse.ArgumentParser(add_help=False)
parent.add_argument('-v', '--verbose', action='store_true')
parent.add_argument('-q', '--quiet', action='store_true')

# Child parsers inherit from parent
parser1 = argparse.ArgumentParser(parents=[parent])
parser1.add_argument('input')

parser2 = argparse.ArgumentParser(parents=[parent])
parser2.add_argument('output')

# Both parser1 and parser2 have -v, -q, plus their own args
```

---

### SUPPRESS - Hide from Help

```python
parser = argparse.ArgumentParser()

# This argument won't appear in help
parser.add_argument('--secret', help=argparse.SUPPRESS)

# Or hide default:
parser.add_argument('-n', default=10, help=argparse.SUPPRESS)
```

---

## PART 5: PARSING METHODS

### parse_args() - Standard Parse

```python
parser = argparse.ArgumentParser()
parser.add_argument('name')
parser.add_argument('-v', '--verbose', action='store_true')

# From command line (automatic)
args = parser.parse_args()

# From custom list
args = parser.parse_args(['John', '-v'])
print(args.name)      # 'John'
print(args.verbose)   # True
```

---

### parse_known_args() - Partial Parsing (Return Remaining)

Parse only known arguments, return unknown as list. Useful for chaining parsers.

```python
parser = argparse.ArgumentParser()
parser.add_argument('-v', '--verbose', action='store_true')
parser.add_argument('file')

# Parse, leaving unknown args
args, remaining = parser.parse_known_args(['-v', '--unknown', 'test.txt', 'extra'])
print(args.verbose)   # True
print(args.file)      # 'test.txt'
print(remaining)      # ['--unknown', 'extra']
```

**Use case: Pass remaining args to another program**
```python
args, unknown = parser.parse_known_args()
subprocess.run(['other_program'] + unknown)
```

---

### parse_intermixed_args() - Intermixed Parsing (No Remaining)

Allow options and positional args to be intermixed (like Unix commands).

```python
parser = argparse.ArgumentParser()
parser.add_argument('--foo')
parser.add_argument('cmd')
parser.add_argument('rest', nargs='*', type=int)

# Normal parse_known_args: stops at first positional
result = parser.parse_known_args('doit 1 --foo bar 2 3'.split())
# (Namespace(cmd='doit', foo='bar', rest=[1]), ['2', '3'])

# Intermixed: collects all matching args
result = parser.parse_intermixed_args('doit 1 --foo bar 2 3'.split())
# Namespace(cmd='doit', foo='bar', rest=[1, 2, 3])
```

**Difference:**
- `parse_known_args()` stops when it hits first positional argument
- `parse_intermixed_args()` allows options after positional arguments

---

### parse_known_intermixed_args() - Intermixed + Partial

Like `parse_intermixed_args()` but returns remaining args.

```python
parser = argparse.ArgumentParser()
parser.add_argument('-v', '--verbose', action='store_true')
parser.add_argument('cmd')

args, remaining = parser.parse_known_intermixed_args(
    ['test', '-v', '--unknown']
)
print(args.verbose)   # True
print(args.cmd)       # 'test'
print(remaining)      # ['--unknown']
```

---

### Access Parsed Arguments

```python
parser = argparse.ArgumentParser()
parser.add_argument('name')
parser.add_argument('-a', '--age', type=int)

args = parser.parse_args(['Alice', '-a', '25'])

# Access as attributes
print(args.name)      # 'Alice'
print(args.age)       # 25
print(args)           # Namespace(name='Alice', age=25)

# Convert to dict
print(vars(args))     # {'name': 'Alice', 'age': 25}

# Check if attribute exists
if hasattr(args, 'value'):
    print(args.value)
```

---

### Namespace - Container Object

```python
# Create manually
args = argparse.Namespace(name='John', age=30)
print(args.name)      # 'John'

# Modify
args.city = 'NYC'
print(args.city)      # 'NYC'

# Convert to dict
d = vars(args)        # {'name': 'John', 'age': 30, 'city': 'NYC'}
```

---

## PART 6: ADVANCED PARSER METHODS

### print_help() - Show Help

```python
parser = argparse.ArgumentParser(description='My tool')
parser.add_argument('input', help='Input file')

# Print help
parser.print_help()

# Output:
# usage: prog.py [-h] input
# 
# My tool
# 
# positional arguments:
#   input       Input file
#
# optional arguments:
#   -h, --help  show this help message and exit
```

---

### print_usage() - Show Usage Only

```python
parser.print_usage()

# Output:
# usage: prog.py [-h] input
```

---

### format_help() - Get Help as String

```python
help_text = parser.format_help()
print(help_text)  # Same as print_help() but returns string
```

---

### format_usage() - Get Usage as String

```python
usage = parser.format_usage()
print(usage)
```

---

### error() - Raise Error with Message

```python
parser.add_argument('-n', type=int)

args = parser.parse_args(['-n', 'abc'])
if args.n < 0:
    parser.error('Argument must be positive!')

# Output:
# usage: prog.py [-h] [-n N]
# prog.py: error: Argument must be positive!
```

---

### exit() - Exit with Message

```python
parser.exit(status=1, message='Custom exit message')
# Exits with status code 1
```

---

### set_defaults() - Set Multiple Defaults

```python
parser = argparse.ArgumentParser()
parser.add_argument('--foo')
parser.add_argument('--bar')

# Set defaults after creation
parser.set_defaults(foo='default1', bar='default2')

args = parser.parse_args([])
print(args.foo)  # 'default1'
```

---

### convert_arg_line_to_args() - Custom File Parsing

Override to parse file arguments differently.

```python
class MyArgumentParser(argparse.ArgumentParser):
    def convert_arg_line_to_args(self, arg_line):
        # Default: one argument per line
        # Override: space-separated words
        return arg_line.split()

# File with: "-v -f output.txt"
# Reads as: ['-v', '-f', 'output.txt']
```

---

### fromfile_prefix_chars - Read Args from File

Read arguments from file when @ prefix is used.

```python
parser = argparse.ArgumentParser(fromfile_prefix_chars='@')
parser.add_argument('-v', '--verbose', action='store_true')
parser.add_argument('-f', '--file')

# File: args.txt contains:
# -v
# -f input.txt

# Usage:
args = parser.parse_args(['@args.txt'])
print(args.verbose)  # True
print(args.file)     # 'input.txt'
```

---

### argument_default - Global Default

```python
parser = argparse.ArgumentParser(argument_default=argparse.SUPPRESS)
parser.add_argument('--foo')
parser.add_argument('bar', nargs='?')

# SUPPRESS means attribute not added if not provided
args = parser.parse_args([])
print(vars(args))  # {} - foo not in namespace

args = parser.parse_args(['--foo', '1', 'BAR'])
print(vars(args))  # {'bar': 'BAR', 'foo': '1'}
```

---

### exit_on_error - Catch Errors Instead of Exiting

```python
parser = argparse.ArgumentParser(exit_on_error=False)
parser.add_argument('--count', type=int)

try:
    args = parser.parse_args(['--count', 'invalid'])
except argparse.ArgumentError as e:
    print(f'Error: {e}')
```

---

### conflict_handler - Handle Conflicting Arguments

```python
# Default: 'error' - raise error on conflict
parser = argparse.ArgumentParser(conflict_handler='error')

# Replace: replace conflicting action
parser = argparse.ArgumentParser(conflict_handler='resolve')
parser.add_argument('-f', '--foo', default=1)
parser.add_argument('-f', '--foo', default=2)  # Replaces first one

args = parser.parse_args([])
print(args.foo)  # 2
```

---

### suggest_on_error - Suggest Closest Match

```python
parser = argparse.ArgumentParser(suggest_on_error=True)
parser.add_argument('--action', choices=['debug', 'dryrun'])

# User types: --action debugg
# Error: "invalid choice: 'debugg', maybe you meant 'debug'?"
```

---

### Custom Action Class

Create custom behavior for arguments.

```python
class CustomAction(argparse.Action):
    def __call__(self, parser, namespace, values, option_string=None):
        # Custom logic
        print(f'Called with: {values}')
        setattr(namespace, self.dest, values.upper())

parser = argparse.ArgumentParser()
parser.add_argument('--text', action=CustomAction)

args = parser.parse_args(['--text', 'hello'])
# Output: Called with: hello
print(args.text)  # HELLO
```

---

### extend Action - Extend List (Python 3.8+)

```python
parser = argparse.ArgumentParser()
parser.add_argument('--foo', action='extend', nargs='+', type=str)

args = parser.parse_args(['--foo', 'f1', '--foo', 'f2', 'f3'])
print(args.foo)  # ['f1', 'f2', 'f3']
```

---

## PART 7: PRACTICAL EXAMPLES (25+)

### Example 1: Simple Script

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description='Greet someone')
    parser.add_argument('name', help='Person to greet')
    parser.add_argument('-l', '--loud', action='store_true', help='Shout')
    
    args = parser.parse_args()
    greeting = f'Hello, {args.name}!'
    
    if args.loud:
        greeting = greeting.upper()
    
    print(greeting)

if __name__ == '__main__':
    main()

# Usage:
# python script.py John
# python script.py John -l
```

---

### Example 2: File Processing

```python
parser = argparse.ArgumentParser()
parser.add_argument('input', type=argparse.FileType('r'), help='Input file')
parser.add_argument('-o', '--output', type=argparse.FileType('w'), default='-')
parser.add_argument('-u', '--upper', action='store_true', help='Convert to uppercase')

args = parser.parse_args()

for line in args.input:
    if args.upper:
        line = line.upper()
    args.output.write(line)

# Usage:
# python script.py input.txt -o output.txt
# python script.py input.txt --upper
```

---

### Example 3: Multiple Values

```python
parser = argparse.ArgumentParser()
parser.add_argument('files', nargs='+', help='Files to process')
parser.add_argument('-t', '--tags', nargs='*', help='Tags')

args = parser.parse_args(['f1.txt', 'f2.txt', '-t', 'tag1', 'tag2'])

print(args.files)  # ['f1.txt', 'f2.txt']
print(args.tags)   # ['tag1', 'tag2']
```

---

### Example 4: Type Checking

```python
def positive_int(value):
    ivalue = int(value)
    if ivalue <= 0:
        raise argparse.ArgumentTypeError(f'{value} must be positive')
    return ivalue

parser = argparse.ArgumentParser()
parser.add_argument('count', type=positive_int, help='Positive number')

# python script.py 5    ✓
# python script.py -3   ✗ Error
```

---

### Example 5: Choices

```python
parser = argparse.ArgumentParser()
parser.add_argument('-f', '--format', choices=['json', 'csv', 'xml'], default='json')
parser.add_argument('-l', '--level', choices=['debug', 'info', 'warning', 'error'])

args = parser.parse_args(['-f', 'csv'])
print(args.format)  # 'csv'
```

---

### Example 6: Defaults & Help

```python
parser = argparse.ArgumentParser(formatter_class=argparse.ArgumentDefaultsHelpFormatter)
parser.add_argument('-t', '--timeout', type=int, default=30, help='Timeout in seconds')
parser.add_argument('-r', '--retries', type=int, default=3, help='Number of retries')

# Help shows default values automatically
```

---

### Example 7: Verbose Counting

```python
parser = argparse.ArgumentParser()
parser.add_argument('-v', '--verbose', action='count', default=0, help='Increase verbosity')

args = parser.parse_args(['-vvv'])
print(args.verbose)  # 3

# Usage:
# script.py -v    → verbose=1
# script.py -vv   → verbose=2
# script.py -vvv  → verbose=3
```

---

### Example 8: Mutually Exclusive

```python
parser = argparse.ArgumentParser()
group = parser.add_mutually_exclusive_group(required=True)
group.add_argument('-u', '--username', help='Username')
group.add_argument('--api-key', help='API key')

# Must provide one or the other
```

---

### Example 9: Calculator with Subcommands

```python
def add_cmd(args):
    return args.x + args.y

def mul_cmd(args):
    return args.x * args.y

parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest='cmd', required=True)

add = subparsers.add_parser('add')
add.add_argument('x', type=float)
add.add_argument('y', type=float)
add.set_defaults(func=add_cmd)

mul = subparsers.add_parser('mul')
mul.add_argument('x', type=float)
mul.add_argument('y', type=float)
mul.set_defaults(func=mul_cmd)

args = parser.parse_args()
print(args.func(args))

# Usage:
# python script.py add 2 3     → 5.0
# python script.py mul 2 3     → 6.0
```

---

### Example 10: Configuration File

```python
parser = argparse.ArgumentParser()
parser.add_argument('-c', '--config', type=argparse.FileType('r'), help='Config file')
parser.add_argument('-v', '--verbose', action='store_true')

args = parser.parse_args()

if args.config:
    config = args.config.read()
    print(f'Loaded config:\n{config}')
```

---

### Example 11: Optional Positional

```python
parser = argparse.ArgumentParser()
parser.add_argument('name', nargs='?', default='World', help='Name to greet')

args = parser.parse_args([])
print(f'Hello, {args.name}!')  # Hello, World!

args = parser.parse_args(['Alice'])
print(f'Hello, {args.name}!')  # Hello, Alice!
```

---

### Example 12: Store Constants

```python
parser = argparse.ArgumentParser()
parser.add_argument('--debug', action='store_const', const=True, default=False)
parser.add_argument('--level', action='store_const', const='DEBUG')

args = parser.parse_args(['--debug', '--level'])
print(args.debug)   # True
print(args.level)   # 'DEBUG'
```

---

### Example 13: Append Values

```python
parser = argparse.ArgumentParser()
parser.add_argument('-t', '--tag', action='append', help='Add tag')

args = parser.parse_args(['-t', 'python', '-t', 'tutorial'])
print(args.tag)  # ['python', 'tutorial']
```

---

### Example 14: List with Limit

```python
parser = argparse.ArgumentParser()
parser.add_argument('items', nargs='*', help='Items')

args = parser.parse_args(['a', 'b', 'c'])
print(args.items[:2])  # ['a', 'b']
```

---

### Example 15: Argument with Equals

```python
parser = argparse.ArgumentParser()
parser.add_argument('-o', '--output', help='Output')

# All equivalent:
# python script.py -o file.txt
# python script.py --output file.txt
# python script.py -o=file.txt
# python script.py --output=file.txt
```

---

### Example 16: JSON Type

```python
import json

def json_type(value):
    try:
        return json.loads(value)
    except json.JSONDecodeError as e:
        raise argparse.ArgumentTypeError(f'Invalid JSON: {e}')

parser = argparse.ArgumentParser()
parser.add_argument('data', type=json_type)

args = parser.parse_args(['{"key": "value"}'])
print(args.data)  # {'key': 'value'}
```

---

### Example 17: Range Type

```python
def range_type(value):
    ivalue = int(value)
    if ivalue < 0 or ivalue > 100:
        raise argparse.ArgumentTypeError(f'{value} not in 0-100')
    return ivalue

parser = argparse.ArgumentParser()
parser.add_argument('percentage', type=range_type)

# 50 → OK
# 150 → Error
```

---

### Example 18: Boolean Flag

```python
parser = argparse.ArgumentParser()
parser.add_argument('--enable', action='store_true')
parser.add_argument('--disable', action='store_false', dest='enable')

# python script.py --enable    → enable=True
# python script.py --disable   → enable=False
```

---

### Example 19: List of Choices

```python
parser = argparse.ArgumentParser()
parser.add_argument('-m', '--mode', nargs='+', choices=['fast', 'slow'])

args = parser.parse_args(['-m', 'fast', 'slow'])
print(args.mode)  # ['fast', 'slow']
```

---

### Example 20: Environmental Variables

```python
import os
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--username', default=os.getenv('USER'))
parser.add_argument('-h', '--host', default=os.getenv('HOST', 'localhost'))

args = parser.parse_args()
```

---

### Example 21: Conditional Arguments

```python
parser = argparse.ArgumentParser()
subparsers = parser.add_subparsers(dest='command')

# 'connect' needs host and port
connect = subparsers.add_parser('connect')
connect.add_argument('host')
connect.add_argument('port', type=int)

# 'disconnect' needs nothing extra
disconnect = subparsers.add_parser('disconnect')

args = parser.parse_args()
```

---

### Example 22: Usage Examples in Help

```python
parser = argparse.ArgumentParser(
    description='My tool',
    epilog='''
Examples:
  python script.py input.txt
  python script.py input.txt -o output.txt --verbose
  python script.py *.txt
    '''
)
```

---

### Example 23: Namespace to Config Dict

```python
parser = argparse.ArgumentParser()
parser.add_argument('-n', '--name')
parser.add_argument('-a', '--age', type=int)

args = parser.parse_args(['-n', 'Alice', '-a', '30'])

config = vars(args)  # {'name': 'Alice', 'age': 30}
```

---

### Example 24: Partial Parse (for Chaining)

```python
parser = argparse.ArgumentParser()
parser.add_argument('--verbose', action='store_true')

args, remaining = parser.parse_known_args(['--verbose', 'other', 'args'])
print(args.verbose)  # True
print(remaining)     # ['other', 'args']
```

---

### Example 25: Custom Help Formatter

```python
class CustomFormatter(argparse.RawDescriptionHelpFormatter):
    pass

parser = argparse.ArgumentParser(
    formatter_class=CustomFormatter,
    description='My Description\n\nWith multiple lines'
)
```

---

## QUICK REFERENCE

| Task | Code |
|------|------|
| Create parser | `parser = argparse.ArgumentParser()` |
| Positional arg | `parser.add_argument('name')` |
| Optional arg | `parser.add_argument('-n', '--name')` |
| Boolean flag | `parser.add_argument('-v', action='store_true')` |
| Default value | `parser.add_argument('-n', default='guest')` |
| Required optional | `parser.add_argument('-i', required=True)` |
| Type checking | `parser.add_argument('-n', type=int)` |
| Choices | `parser.add_argument('-m', choices=['a', 'b'])` |
| Multiple args | `parser.add_argument('files', nargs='+')` |
| Count flag | `parser.add_argument('-v', action='count')` |
| Group args | `group = parser.add_argument_group('name')` |
| Mutually exclusive | `group = parser.add_mutually_exclusive_group()` |
| Subcommands | `subparsers = parser.add_subparsers()` |
| Parse | `args = parser.parse_args()` |
| Parse partial | `args, unknown = parser.parse_known_args()` |
| Parse intermixed | `args = parser.parse_intermixed_args()` |
| Access arg | `args.name` |
| Help text | `parser.add_argument('-n', help='Description')` |
| Show help | `parser.print_help()` |
| Get help string | `help_text = parser.format_help()` |
| Error | `parser.error('Message')` |
| Set defaults | `parser.set_defaults(foo='bar')` |
| File parsing | `fromfile_prefix_chars='@'` |
| Catch errors | `exit_on_error=False` |

---

## COMMON PATTERNS

**Simple CLI:**
```python
parser = argparse.ArgumentParser(description='My tool')
parser.add_argument('input')
parser.add_argument('-o', '--output')
args = parser.parse_args()
```

**With subcommands:**
```python
subparsers = parser.add_subparsers(dest='cmd')
sub = subparsers.add_parser('cmd1')
sub.add_argument('arg1')
```

**Check if argument provided:**
```python
if args.input:
    process(args.input)
```

**Convert to dict:**
```python
config = vars(args)
```

**Custom validation:**
```python
if args.count < 0:
    parser.error('Count must be positive')
```

**Parse partial (chain parsers):**
```python
args, unknown = parser.parse_known_args()
other_parser.parse_args(unknown)
```

**Intermixed args (Unix-style):**
```python
args = parser.parse_intermixed_args()
```

**Read from file:**
```python
parser = argparse.ArgumentParser(fromfile_prefix_chars='@')
# Usage: program @argfile.txt
```

**Catch parsing errors:**
```python
parser = argparse.ArgumentParser(exit_on_error=False)
try:
    args = parser.parse_args(...)
except argparse.ArgumentError as e:
    handle_error(e)
```

---

## THAT'S EVERYTHING!

You now know:
- ✅ Create parsers and add arguments
- ✅ Positional vs optional arguments
- ✅ Actions (store, store_true, append, count, extend, etc)
- ✅ Type checking and conversion
- ✅ Default values and required arguments
- ✅ Choices and nargs
- ✅ Argument groups and mutually exclusive groups
- ✅ Subcommands and parent parsers
- ✅ **PARSING METHODS** (parse_args, parse_known_args, parse_intermixed_args, parse_known_intermixed_args)
- ✅ **ADVANCED METHODS** (print_help, error, set_defaults, fromfile_prefix_chars, exit_on_error, custom actions)
- ✅ Help and error handling
- ✅ 25+ practical examples

Argparse mastery achieved!
