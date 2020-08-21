# Condition

## 1. `if`

- No need brace around condition

```python
if <Condition>:
    ...
(elif <Condition>:
    ...)*
(else:
    ...)?
```

## 2. `switch`

### 2.1. `if elif else`

```python
if <Condition>:
    ...
(elif <Condition>:
    ...)*
(else:
    ...)?
```

### 2.2. Dictionary

```python
value = {
    option1: value1,
    ...
    optionn: valuen
}[switch_value]

value = {
    option1: value1,
    ...
    optionn: valuen
}.get(switch_value, default_value)

result = {
    option1: lambda x: expression1(x)
    ...
    optionn: lambda x: expressionn(x)
}[switch_value](x)

result = {
    option1: function1
    ...
    optionn: functionn
}[switch_value](args)
```
