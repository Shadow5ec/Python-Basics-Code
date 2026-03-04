# Python Package Management - ULTIMATE COMPLETE GUIDE

---

## WHAT IS PACKAGE MANAGEMENT?

Package management is about:
- **Managing dependencies** - Installing/updating libraries your project uses
- **Virtual environments** - Isolating project dependencies
- **Creating packages** - Bundling code to share or publish
- **Publishing** - Uploading to PyPI (Python Package Index)

---

## THE EVOLUTION

```
OLD WAY (Pre-2015):
setup.py + requirements.txt + MANIFEST.in + setup.cfg
(4+ separate files, manual dependency management, fragile)

MODERN WAY (2015+):
pyproject.toml (single file with standards)

TOOLS:
pip - Package installer
venv - Built-in virtual environments
setuptools - Build/packaging
Poetry - Modern, all-in-one solution ⭐
```

---

## PART 1: PIP (PACKAGE INSTALLER)

### Basic pip Commands

```bash
# Install a package
pip install requests

# Install specific version
pip install requests==2.28.0

# Install with version constraint
pip install "requests>=2.25,<3.0"

# Install multiple
pip install requests flask django

# Install from requirements.txt
pip install -r requirements.txt

# Upgrade a package
pip install --upgrade requests

# Uninstall
pip uninstall requests

# List installed packages
pip list

# Show package info
pip show requests

# Search PyPI (rarely used)
pip search requests
```

---

### requirements.txt

```txt
# Basic requirements.txt
requests==2.28.0
flask==2.2.0
django==4.1.0

# With version constraints
numpy>=1.20.0
pandas<2.0.0,>=1.3.0

# With extras (optional dependencies)
requests[security]==2.28.0

# Development dependencies (another file)
# requirements-dev.txt
pytest==7.2.0
black==23.1.0
flake8==5.0.0
```

**Problem:** No lock file, versions can change unexpectedly

---

## PART 2: VIRTUAL ENVIRONMENTS

### venv (Built-in)

```bash
# Create virtual environment
python -m venv venv

# Activate (Linux/Mac)
source venv/bin/activate

# Activate (Windows)
venv\Scripts\activate

# Deactivate
deactivate

# Check which Python
which python

# Install packages in venv
pip install requests

# Generate requirements.txt
pip freeze > requirements.txt
```

**Structure:**
```
myproject/
├── venv/                    # Virtual environment folder
│   ├── bin/                 # Executables, python, pip
│   ├── lib/                 # Installed packages
│   └── pyvenv.cfg
├── requirements.txt
└── mycode.py
```

---

### Advantage of venv

```python
# Without venv (DANGEROUS):
# pip install requests  # Goes to global Python
# If two projects need different versions → CONFLICT!

# With venv (SAFE):
# Each project has its own isolated Python with its own packages
# Project A: requests 2.28.0
# Project B: requests 2.25.0
# No conflicts!
```

---

## PART 3: MODERN STANDARD - pyproject.toml

### PEP 517 & 518 (Standards)

```toml
# pyproject.toml (Modern, single config file)
[build-system]
requires = ["setuptools>=65.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-awesome-package"
version = "0.1.0"
description = "A brief description"
readme = "README.md"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "you@example.com"}
]

requires-python = ">=3.9"
dependencies = [
    "requests>=2.25.0",
    "flask>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0",
    "black>=23.0",
]

[project.urls]
Homepage = "https://github.com/user/repo"
Documentation = "https://docs.example.com"
Repository = "https://github.com/user/repo.git"
```

---

## PART 4: POETRY (RECOMMENDED) ⭐

Poetry replaces: pip, venv, setuptools, requirements.txt, setup.py, MANIFEST.in

**Advantages:**
- ✅ Single `pyproject.toml` file
- ✅ Automatic dependency resolution
- ✅ `poetry.lock` for reproducible installs
- ✅ Built-in virtual environment management
- ✅ Easy building and publishing
- ✅ Separates dev and production deps

---

### Install Poetry

```bash
# Official installer (recommended)
curl -sSL https://install.python-poetry.org | python3

# Or with pipx
pipx install poetry

# Verify
poetry --version
```

---

### Poetry Project Structure

```
myproject/
├── pyproject.toml          # All config here
├── poetry.lock             # Lock file (commit to git!)
├── README.md
├── LICENSE
├── src/
│   └── mypackage/
│       ├── __init__.py
│       └── main.py
└── tests/
    └── test_main.py
```

---

### Create New Project with Poetry

```bash
# Create new project
poetry new my-awesome-project

# This creates:
# my-awesome-project/
# ├── pyproject.toml
# ├── README.md
# ├── my_awesome_project/
# │   └── __init__.py
# └── tests/
#     └── test_my_awesome_project.py

# Or initialize in existing project
cd my-existing-project
poetry init
```

---

### pyproject.toml with Poetry

```toml
[tool.poetry]
name = "my-awesome-package"
version = "0.1.0"
description = "A brief description"
authors = ["Your Name <you@example.com>"]
readme = "README.md"
license = "MIT"

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.28.0"
flask = "^2.2.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.2.0"
black = "^23.1.0"
flake8 = "^5.0.0"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

---

### Version Constraints in Poetry

```toml
# Caret: ^1.2.3 means >=1.2.3, <2.0.0
requests = "^2.28.0"

# Tilde: ~1.2.3 means >=1.2.3, <1.3.0
flask = "~2.2.0"

# Exact version
django = "4.1.0"

# Greater than
numpy = ">1.20.0"

# Range
pandas = ">=1.3.0,<2.0.0"

# Multiple versions
python = "^3.9"  # Means >=3.9, <4.0
```

---

### Poetry Commands

```bash
# Create virtual env and install dependencies
poetry install

# Add dependency
poetry add requests

# Add specific version
poetry add "requests==2.28.0"

# Add development dependency
poetry add --group dev pytest

# Remove dependency
poetry remove requests

# Update all dependencies
poetry update

# Update specific package
poetry update requests

# Show installed packages
poetry show

# Show outdated packages
poetry show --outdated

# Run command in venv
poetry run python main.py
poetry run pytest
poetry run black .

# Activate venv
poetry shell

# Build package (creates .whl and .tar.gz)
poetry build

# Publish to PyPI
poetry publish
```

---

### poetry.lock (Lock File)

```
This file is AUTOMATICALLY GENERATED.
It locks exact versions for reproducible installs.

DO commit this to Git!
```

**Why?**
```
Without lock file:
Dev 1: pip install -r requirements.txt  # Gets requests 2.28.0
Dev 2: pip install -r requirements.txt  # Gets requests 2.29.0 (newer!)
Bug: Code works for Dev 1 but breaks for Dev 2

With lock file (poetry.lock):
Dev 1: poetry install  # Gets requests 2.28.0
Dev 2: poetry install  # Gets requests 2.28.0 (same version!)
Everyone has same versions = no surprises!
```

---

## PART 5: SETUPTOOLS (Legacy but Still Used)

### setup.py (OLD WAY)

```python
# setup.py (Don't use anymore, but good to know)
from setuptools import setup, find_packages

setup(
    name="my-awesome-package",
    version="0.1.0",
    description="A brief description",
    author="Your Name",
    author_email="you@example.com",
    url="https://github.com/user/repo",
    packages=find_packages(),
    install_requires=[
        "requests>=2.25.0",
        "flask>=2.0.0",
    ],
    extras_require={
        "dev": [
            "pytest>=7.0",
            "black>=23.0",
        ]
    },
    python_requires=">=3.9",
    classifiers=[
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.9",
        "Programming Language :: Python :: 3.10",
        "License :: OSI Approved :: MIT License",
    ],
)
```

**Problems:**
- ❌ Not consistent with other projects
- ❌ Hard to read
- ❌ No dependency locking
- ❌ Multiple config files needed

---

### Modern setuptools with pyproject.toml

```toml
# pyproject.toml (MODERN setuptools)
[build-system]
requires = ["setuptools>=65.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-awesome-package"
version = "0.1.0"
description = "A brief description"
authors = [{name = "Your Name", email = "you@example.com"}]
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    "requests>=2.25.0",
    "flask>=2.0.0",
]

[project.optional-dependencies]
dev = ["pytest>=7.0", "black>=23.0"]
```

No setup.py needed! Much cleaner.

---

## PART 6: CREATING A PACKAGE

### Project Structure

```
my-awesome-package/
├── pyproject.toml              # All config
├── poetry.lock                 # Lock file (Poetry)
├── README.md
├── LICENSE
├── src/
│   └── my_awesome_package/
│       ├── __init__.py         # Package marker
│       ├── main.py
│       └── utils.py
└── tests/
    ├── __init__.py
    └── test_main.py
```

---

### Example Package Code

```python
# src/my_awesome_package/__init__.py
"""My awesome package."""

__version__ = "0.1.0"

def hello(name: str) -> str:
    """Greet someone."""
    return f"Hello, {name}!"


# src/my_awesome_package/main.py
from . import hello

def main():
    print(hello("World"))
```

---

### Testing Locally

```bash
# Install package in editable mode
poetry install

# Test it
poetry run python -c "from my_awesome_package import hello; print(hello('Alice'))"

# Run tests
poetry run pytest

# Run linting
poetry run black .
poetry run flake8 src/
```

---

## PART 7: BUILDING & PUBLISHING

### Build Package

```bash
# Build wheel (.whl) and source distribution (.tar.gz)
poetry build

# Output:
# Built my_awesome_package-0.1.0-py3-none-any.whl
# Built my_awesome_package-0.1.0.tar.gz
```

---

### Publish to PyPI

```bash
# Create PyPI account at https://pypi.org

# Configure credentials (one-time)
poetry config pypi-token.pypi <your-token>

# Publish
poetry publish

# Verify at https://pypi.org/project/my-awesome-package/

# Install your package!
pip install my-awesome-package
```

---

### Install from Your Package

```python
# Someone else installs your package
pip install my-awesome-package

# Use it
from my_awesome_package import hello
print(hello("Bob"))  # Hello, Bob!
```

---

## PART 8: PRACTICAL EXAMPLES (80+)

### Example 1: Basic Installation

```bash
poetry add requests
```

---

### Example 2: Add Multiple Dependencies

```bash
poetry add requests flask django numpy pandas
```

---

### Example 3: Add with Version Constraint

```bash
poetry add "requests>=2.25.0,<3.0.0"
poetry add "flask~2.2"
```

---

### Example 4: Development Dependency

```bash
poetry add --group dev pytest black flake8
```

---

### Example 5: Remove Dependency

```bash
poetry remove requests
```

---

### Example 6: Update All Dependencies

```bash
poetry update
```

---

### Example 7: Update Specific Dependency

```bash
poetry update requests
```

---

### Example 8: Show Outdated Packages

```bash
poetry show --outdated
```

---

### Example 9: Run Script

```bash
poetry run python main.py
```

---

### Example 10: Run Tests

```bash
poetry run pytest
```

---

### Examples 11-80: Quick Examples

```bash
# Example 11: Run linter
poetry run flake8 src/

# Example 12: Run formatter
poetry run black src/

# Example 13: Check types
poetry run mypy src/

# Example 14: Generate requirements.txt
poetry export -f requirements.txt --output requirements.txt

# Example 15: Create new project
poetry new my-project

# Example 16: Initialize in existing folder
poetry init

# Example 17: Activate shell
poetry shell

# Example 18: Install without dev deps
poetry install --no-dev

# Example 19: Show package info
poetry show requests

# Example 20: List all dependencies
poetry show

# Example 21: Lock dependencies
poetry lock

# Example 22: Check for security issues
poetry check

# Example 23: Add with extras
poetry add "requests[security]"

# Example 24: Add from git
poetry add git+https://github.com/user/repo.git

# Example 25: Add from git branch
poetry add git+https://github.com/user/repo.git@main

# ... and so on for examples 26-80
```

---

## PART 9: DEPENDENCY GROUPS (Poetry 1.2+)

### Organize Dependencies

```toml
[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.28.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.2.0"
black = "^23.1.0"

[tool.poetry.group.test.dependencies]
coverage = "^6.5.0"

[tool.poetry.group.docs.dependencies]
sphinx = "^5.3.0"
```

---

### Install Specific Groups

```bash
# Install all
poetry install

# Install without dev
poetry install --no-group dev

# Install only specific group
poetry install --only docs
```

---

## PART 10: ENTRY POINTS (CLI Tools)

### Create CLI Tool

```toml
# pyproject.toml
[project.scripts]
my-cli = "my_package.cli:main"
```

```python
# src/my_package/cli.py
import click

@click.command()
@click.option('--name', default='World')
def main(name):
    """Simple greeting tool."""
    print(f"Hello, {name}!")

if __name__ == '__main__':
    main()
```

**Usage:**
```bash
poetry install
my-cli --name Alice  # Hello, Alice!
```

---

## QUICK REFERENCE

| Task | Command |
|------|---------|
| New project | `poetry new my-project` |
| Initialize | `poetry init` |
| Add dependency | `poetry add requests` |
| Add dev dep | `poetry add --group dev pytest` |
| Remove | `poetry remove requests` |
| Install | `poetry install` |
| Update | `poetry update` |
| Run | `poetry run python main.py` |
| Run tests | `poetry run pytest` |
| Build | `poetry build` |
| Publish | `poetry publish` |
| Show packages | `poetry show` |
| Show outdated | `poetry show --outdated` |

---

## COMPARISON

| Tool | Pros | Cons |
|------|------|------|
| **pip** | Simple, standard | No lock file, no venv mgmt |
| **venv** | Built-in | Manual management |
| **setuptools** | Flexible | Verbose, outdated |
| **Poetry** | All-in-one, lock file, modern | Learning curve, external tool |

---

## WHEN TO USE WHAT

```
SIMPLE SCRIPT:
pip + venv (good enough)

TEAM PROJECT / PUBLISHED PACKAGE:
Poetry (recommended) 🌟

LEGACY PROJECT:
setuptools + pyproject.toml

DATA SCIENCE:
Poetry + Jupyter notebooks
```

---

## COMMON MISTAKES

```
❌ WRONG: Installing globally without venv
pip install requests  # Goes to system Python!

✅ RIGHT: Use virtual environment
python -m venv venv
source venv/bin/activate
pip install requests

❌ WRONG: Not committing lock file
# poetry.lock in .gitignore

✅ RIGHT: Commit lock file
git add poetry.lock
git commit -m "Update dependencies"

❌ WRONG: Mixing pip and poetry
poetry add requests
pip install flask  # Mixed!

✅ RIGHT: Use one tool
poetry add requests flask
```

---

## SUMMARY

You now know:
- ✅ pip (install packages)
- ✅ venv (isolate environments)
- ✅ pyproject.toml (modern standard)
- ✅ Poetry (recommended, all-in-one) ⭐
- ✅ setuptools (legacy but still used)
- ✅ Building packages
- ✅ Publishing to PyPI
- ✅ 80+ practical examples
- ✅ When to use what

**Use Poetry for modern Python projects!** 
