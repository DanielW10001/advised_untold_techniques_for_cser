# Class

## 1. Attribute

- Not defined in class, but define in `__init__`

## 2. Object

```python
<Object> = <ClassName>(<Arguments>)
<Object>.<Attribute>
<Object>.<Method>(<Arguments>)
```

## 3. Access Control

- Pseudo Private: `__<Name>`
    - Access in class: `__<Name>`
    - (Not Recommended) Access out class: `_<ClassName>__<Name>`
    - Not imported with `from Class import *`
- Week Pseudo Private: `_<Name>`
    - Access in class: `_<Name>`
    - (Not Recommended) Access out class: `_<Name>`
    - Not imported with `from Class import *`