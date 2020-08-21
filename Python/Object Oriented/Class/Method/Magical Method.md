# Magical Method

## 1. Initial

- Name: `__init__`
- Argument
    - self
    - Object Attributes

```python
def __init__(self(, <Attribute>)*):
    self.<Attribute> = <Attribute>
    ...
```

## 2. String

- Name: `__str__`
- Define string representation of object
- Will automaticly invoke when use object as `str`

```python
def __str__(self):
    ...
    return rst_str
```