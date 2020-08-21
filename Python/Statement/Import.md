# Import

## 1. Syntax

- Import module or attribute: `import <Package>.<Module>(.<Attribute>)?`
    - Usage: `<Package>.<Module>(.<Attribute>)?`
- Import all (public)
    - `import <Package>.*`
    - `import <Package>.<Module>.*`
        - `_<Name>` and `__<Name>` are not imported
- Import with simplified name
    - `from <Package> import <Module>`
    - `from <Package>.<Module> import <Attribute>`
    - Usage: `<ModuleOrAttribute>`
- Import all (public) with simplified name
    - `from <Package> import *`
    - `form <Package>.<Module> import *`
        - `_<Name>` and `__<Name>` are not imported
- Import with customized name: `<Import> as <CustomizedName>`
    - Usage: `<CustomizedName>`

## 2. Machenism

- All code in module will be excuted when it imported
- `def` statement of attribute will be excuted when it imported
- This excution is not designed for excuting code, but designed for define attribute
    - Do not using import excution as code excution
- If module / attribute has been imported, imported it again will not excute any code
    - This prevents loop importing when two module using each other

## 3. Reimport

- If module is changed after imported, it may be necessary to reimport it: `<Module> = importlib.reload(<Module>)`
    - `importlib.reload(<Module>)` return module reloaded
- Reimport will not change objects created with old version of imported module's class
    - Recreate to update