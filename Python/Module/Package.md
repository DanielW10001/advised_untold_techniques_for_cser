# Package

## 1. PythonPath

- Python Intepreter look for module using its package path
    - Default contains standard library and working directory `.`
- Look for based on each path of enviroment variable `PYTHONPATH`

### 1.1. `sys.path`

- `list[str]`
- Each `str` inside is `'<PythonPath>'`
- Python Intepreter will add `PYTHONPATH` into `sys.path` automaticlly
    - Thus eding either `PYTHONPATH` or `sys.path` is equally effective

### 1.2. Configration

- Enable Intepreter to find Module Correctly

#### 1.2.1. Put Package at Python Path

- Put package at one of the python path
- Print Python Path List: `pprint(sys.path)`
- Recommended: `<PythonDir>/lib/site-packages/`

#### 1.2.2. Add Python Path

- By `PYTHONPATH` (Recommended)
    - Editing `.<Shell>rc`: `export PYTHONPATH=${PYTHONPATH}:<NewPythonPath>`
- By `sys.path`: `sys.path.append(r'<PythonPath>`)`
    - Cannot use `~` to represent home directly
    - `sys.path.append(sys.path.expanduser(r'<PythonPath>'))`
- By Path Configration File `.pth`
    - Located in some special dir
    - Containing Python Paths to be added into `sys.path`
    - Refer stdlib module `site` for details

## 2. Package

### 2.1. Definition

- Package is special module used to organize modules
    - Package is special module
    - Package: A Directory contains `__init__.py`
- Package can contains other modules (which can also be packages)
    - To contains other modules (normal or package), Put them in package directory
- File Type
    - Normal Module: `<ModuleName>.py`
    - Package Module: `<ModuleName>/__init__.py`

### 2.2. Usage

#### 2.2.1. Import Package

- `import <Package>`
    - Package is special module
- Import contents of `<Directory>/__init__.py`
    - Import and ONLY import `__init__.py`
    - The submodules are not imported
- Usage: `<Package>.<Attribute>`

#### 2.2.2. Import Module in Package

- Module Path: The module's path is the path from `<PythonPath>` to itself
- `import <Package>(.<Package>)*.<Module>`
- `from <Package>(.<Package>)* import <Module>`
    - Import and ONLY import indicated module
    - The parent package (module, `__init__.py`) is not imported
- Notice
    - To use a submodule of itself, a package module also need to `import` the submodule