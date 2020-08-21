# Variable

- Object Reference
    - Value has type, Variable do not has type

## 1. Define

- No Declaration Needed
- Define by assigning: `<Var> = <Value>`
- Life Cycle: Exist when needed, disapear when not needed

## 2. Scope

- Inside where defined: Module, Function, Class, Object
- Access
    - Access Global Variable in Local Scope:

```python
global_variable = dummy_1

def fuction_1():
    global global_variable
    # Do something with global_variable

def fuction_2():
    global_variable = dummy_2
    # Wrong way: override global variable with same-name local variable
```

## 3. Copy

- Return a copy of current object: `object.copy()`

## 4. Annotation

- Annotation: A expression associated with the variable
    - Expression is evaluated when variable defined

```python
<Variable>[: <Annotation>][ = <Value>]
```

### 4.1. Type Annotation

- Used for type hint and check

#### 4.1.1. Generic Type

- Built-in Container Types cannot be used in Type Annotations to indicate its element's type

```python
var: list[int]  # INVALID
```

- Use abstract base classes in `typing` module to support indicating element type of container type
    - Sequence: `<ContainerType>[<ElementType>]`
    - Dictionary: `<Variable>: Dict[<KeyType>, <ValueType>]`
    - Generic Container Type is defined with capitalization
        - E.g.: `list (built-in) -> typing.List (Capitalized)`

```python
from typing import <ContainerType>

var: <ContainerType>[<ElementType>]  # VALID
```
