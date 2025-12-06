---
title: Sample Guide
layout: default
parent: Guides
nav_order: 1
---

# Sample Guide: Creating a Simple Script

This is a sample guide to demonstrate the structure and formatting of guides in this knowledge base.

## Introduction

This guide will walk you through creating a simple Python script that demonstrates basic file operations. It's designed to show best practices for writing guides.

## Prerequisites

Before starting, make sure you have:

- Python 3.7 or higher installed
- Basic knowledge of Python syntax
- A text editor or IDE

## Step 1: Create the Script File

Create a new file called `file_operations.py`:

```python
#!/usr/bin/env python3
"""
Simple script to demonstrate file operations.
"""

import os
from pathlib import Path
```

## Step 2: Add the Main Function

Add a main function to handle file operations:

```python
def main():
    """Main function to demonstrate file operations."""
    # Create a sample file
    filename = "sample.txt"
    
    # Write to file
    with open(filename, 'w') as f:
        f.write("Hello, World!\n")
        f.write("This is a sample file.\n")
    
    print(f"Created {filename}")
    
    # Read from file
    with open(filename, 'r') as f:
        content = f.read()
        print(f"\nFile contents:\n{content}")
    
    # Get file info
    file_path = Path(filename)
    print(f"\nFile size: {file_path.stat().st_size} bytes")
    print(f"File exists: {file_path.exists()}")
```

## Step 3: Add the Entry Point

Add the standard Python entry point:

```python
if __name__ == "__main__":
    main()
```

## Step 4: Run the Script

Execute the script:

```bash
python file_operations.py
```

Expected output:

```
Created sample.txt

File contents:
Hello, World!
This is a sample file.

File size: 42 bytes
File exists: True
```

## Complete Code

Here's the complete script:

```python
#!/usr/bin/env python3
"""
Simple script to demonstrate file operations.
"""

import os
from pathlib import Path

def main():
    """Main function to demonstrate file operations."""
    # Create a sample file
    filename = "sample.txt"
    
    # Write to file
    with open(filename, 'w') as f:
        f.write("Hello, World!\n")
        f.write("This is a sample file.\n")
    
    print(f"Created {filename}")
    
    # Read from file
    with open(filename, 'r') as f:
        content = f.read()
        print(f"\nFile contents:\n{content}")
    
    # Get file info
    file_path = Path(filename)
    print(f"\nFile size: {file_path.stat().st_size} bytes")
    print(f"File exists: {file_path.exists()}")

if __name__ == "__main__":
    main()
```

## Troubleshooting

### Permission Denied Error

If you get a permission error:

```bash
chmod +x file_operations.py
```

### Module Not Found

Make sure you're using Python 3:

```bash
python3 file_operations.py
```

## Next Steps

Now that you've created a simple script, you can:

- Add error handling
- Work with different file formats (JSON, CSV, etc.)
- Implement command-line arguments
- Add logging functionality

## Related Topics

- [Reference: Python Snippets]({% link docs/reference/python-snippets.md %})
