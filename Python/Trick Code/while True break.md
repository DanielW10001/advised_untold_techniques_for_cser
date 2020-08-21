# while True/break

- 通过while True/break避免虚值或重复代码

```python3
# Dummy Value Approach
value = 'Dummy'  # Dummy Value
while value:  # Loop while value is not empty
    value = input()
    do_something(value)

# Duplicated Code Approach
value = input()
while value:  # Loop while value is not empty
    value = input()  # Duplicated Code
    do_something(value)

# while True/break Approach
while True:
    value = input()
    if not value: break  # Break when value is empty. Loop while value is not empty
    do_something(value)
```
