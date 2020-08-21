# Module

- Module: Python Source Code File
    - Making code reusable
- Module Name: File Name
    - Module Name of `<FileName>.py`: `<FileName>`
        - Tailing name `.py` not included

## 1. Mark

### 1.1. ShaBang

- Must be the first line of module
- Indicate Interpreter when be excuted as a script in POSIX

```python
#!/usr/bin/env python3
# ...
```

### 1.2. Coding

- Must be the first line or right after ShaBang line
- Indicate Module Coding

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

## 2. Attribute

- All class, function, variable are attribute of module
- Module Attribute's scope is within module itself

## 3. Variable

### 3.1. Name

- `__name__`
- Value
    - Module Name when run because be imported
    - `'__main__'` when run as a script (or as intepreter)

### 3.2. File

- `__file__`: Module file name

## 4. Module Path

- A path from top super package to module itself: `<Package>(.<Package>)*.<Module>`
    - No tailing name `.py` included
- Module can be placed directly in python path
    - Do not included by any package
    - Do not have any super package
    - Module Path: `<Module>`

## 5. Import

- Import Objects from other module

```python
import <Module>
<Module>.<Attribute>
```

- Simplified Syntax

```python
from <Module> import <Attribute>
<Attribute>
```

- Import all public attribute: `from <Module> import *`
    - `_<Name>` and `__<Name>` attributes will not be imported

### 5.1. Importable

- Any module can be imported
    - Module `<ModuleName>.py` can be imported as `<ModuleName>`
        - No tailing name `.py` needed
- All code in module will be excuted when be imported
- To make module both importable and runnable

```python
# ...

def test():
    # ...

if __name__ == "__main__":
    test()
```

- Test can also be invoked manually after imported: `<Module>.test()`

### 5.2. Usage

- Use module's attribute after module imported: `<Module>.<Attribute>`

## 6. `__pycache__`

- When module be imported, python inteperter may generate a dir `__pycache__` at where module is placed
- `__pycache__` contains processed module data, which allows intepreter process module more efficiently
- Intepreter will use `__pycache__` if module has not been changed after `__pycache__`'s generating.
- Older python intepreter using `<Module>.pyc` instead of `__pycache__`
- It is safe to delete `__pycache__`, it will be generated again if necessary
