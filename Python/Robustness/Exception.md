# Exception

- Exception (Object) is instance of Exception (or its sub) Class
- Throw exception when something wrong
- If exception is not handled util it is thrown out from top level script, the program will print error info and exit abnormally

## 1. Exception Class

- Every Exception (Object) is instance of Exception Class
- Exception Class must be subclass of `Exception`
- Define Exception Class:

```python
class <Exception>(Exception): pass
```

## 2. try/except/else/finally

- Try to excute code
- When indicated exception thrown, excute `except` code
- When no exception thrown, excute `else` code
- Finilize at any situation with `finally` code

```python
try:
    # ...
except (<ExceptionType>(, <ExceptionType>)*)( as <Exception>)?:
    # <Handler>
# ...
except (<ExceptionType>(, <ExceptionType>)*):
    # <Handler>
else:
    # ...
finally:
    # ...
```

- Catch exception of any type:
    - CAUTION: DANGROUS This may also catch exception of unexpected type, leading to unexpected behavior, such as suppress the intention of exit with `<CTRL-C>`

```python
try:
    # ...
except:
    # <Handler>
```

## 3. raise

- Use `raise` to raise exception
    - Raise empty exception: `raise <ExceptionClass>`
    - Raise exception with message: `raise <ExceptionClass>("<Message>")`
    - reraise exception after catch in `except` block: `raise`

## 4. Context

- When another exception thrown in `except` block, the original exception trigered this `except` block will be stored in the new exception as context
- Indicate context manually: `raise <Exception> from <Exception>`
    - Swith context off manually: `raise <Exception> from None`
- Catch and reThrow
    - Same Type:
```python
try:
    # ...
except <Exception>:
    raise
```
    - Different Type:
```python
try:
    # ...
except <Exception> as exception:
    raise <Exception>(exception) from None
```

## 5. Exception Object

- `str(exception)`: Exception message

## 6. Built-in Exception

- `ZeroDivisionError`
