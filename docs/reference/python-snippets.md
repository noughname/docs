---
title: Python Snippets
layout: default
parent: Reference
nav_order: 1
---

# Python Code Snippets

Useful Python code snippets and examples.

## File Operations

### Read File Contents

```python
# Read entire file
with open('file.txt', 'r') as f:
    content = f.read()

# Read line by line
with open('file.txt', 'r') as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open('file.txt', 'r') as f:
    lines = f.readlines()
```

### Write to File

```python
# Write string to file
with open('file.txt', 'w') as f:
    f.write('Hello, World!\n')

# Append to file
with open('file.txt', 'a') as f:
    f.write('New line\n')

# Write multiple lines
lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']
with open('file.txt', 'w') as f:
    f.writelines(lines)
```

## JSON Operations

### Read JSON File

```python
import json

with open('data.json', 'r') as f:
    data = json.load(f)
```

### Write JSON File

```python
import json

data = {
    'name': 'John Doe',
    'age': 30,
    'city': 'New York'
}

with open('data.json', 'w') as f:
    json.dump(data, f, indent=2)
```

### Parse JSON String

```python
import json

json_string = '{"name": "John", "age": 30}'
data = json.loads(json_string)
```

## List Comprehensions

### Basic List Comprehension

```python
# Square all numbers
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]
# Result: [1, 4, 9, 16, 25]
```

### With Condition

```python
# Get even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = [x for x in numbers if x % 2 == 0]
# Result: [2, 4, 6, 8, 10]
```

### Nested Comprehension

```python
# Flatten a nested list
nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [item for sublist in nested for item in sublist]
# Result: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## Dictionary Operations

### Dictionary Comprehension

```python
# Create dictionary from lists
keys = ['a', 'b', 'c']
values = [1, 2, 3]
dictionary = {k: v for k, v in zip(keys, values)}
# Result: {'a': 1, 'b': 2, 'c': 3}
```

### Merge Dictionaries

```python
# Python 3.9+
dict1 = {'a': 1, 'b': 2}
dict2 = {'c': 3, 'd': 4}
merged = dict1 | dict2

# Python 3.5+
merged = {**dict1, **dict2}
```

### Get with Default

```python
data = {'name': 'John', 'age': 30}

# Get value with default
city = data.get('city', 'Unknown')
# Result: 'Unknown'
```

## String Operations

### String Formatting

```python
# f-strings (Python 3.6+)
name = "John"
age = 30
message = f"My name is {name} and I'm {age} years old"

# format() method
message = "My name is {} and I'm {} years old".format(name, age)

# Named placeholders
message = "My name is {name} and I'm {age} years old".format(name=name, age=age)
```

### String Methods

```python
text = "Hello, World!"

# Case conversion
text.upper()        # "HELLO, WORLD!"
text.lower()        # "hello, world!"
text.capitalize()   # "Hello, world!"
text.title()        # "Hello, World!"

# Checking
text.startswith('Hello')  # True
text.endswith('!')        # True
'World' in text           # True

# Splitting and joining
words = text.split(', ')  # ['Hello', 'World!']
joined = '-'.join(words)  # 'Hello-World!'
```

## Error Handling

### Try-Except Block

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
else:
    print("No errors occurred")
finally:
    print("This always executes")
```

### Context Managers

```python
# File handling with automatic cleanup
with open('file.txt', 'r') as f:
    content = f.read()
# File is automatically closed

# Custom context manager
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    yield
    end = time.time()
    print(f"Elapsed time: {end - start:.2f}s")

with timer():
    # Your code here
    pass
```

## Working with Paths

### Using pathlib

```python
from pathlib import Path

# Create path object
path = Path('folder/subfolder/file.txt')

# Check existence
if path.exists():
    print("File exists")

# Get parent directory
parent = path.parent

# Get filename
filename = path.name

# Get extension
extension = path.suffix

# Create directory
Path('new_folder').mkdir(parents=True, exist_ok=True)

# List directory contents
for item in Path('.').iterdir():
    print(item)

# Find files by pattern
for file in Path('.').glob('*.txt'):
    print(file)
```

## Date and Time

### Working with datetime

```python
from datetime import datetime, timedelta

# Current date and time
now = datetime.now()
today = datetime.today()

# Format datetime
formatted = now.strftime('%Y-%m-%d %H:%M:%S')

# Parse string to datetime
date = datetime.strptime('2024-01-15', '%Y-%m-%d')

# Add/subtract time
tomorrow = now + timedelta(days=1)
last_week = now - timedelta(weeks=1)

# Get components
year = now.year
month = now.month
day = now.day
hour = now.hour
```

## Related Topics

- [Sample Guide: Creating a Simple Script]({% link docs/guides/sample-guide.md %})
- [Configuration Examples]({% link docs/reference/config-examples.md %})
