# Function

## 1. Define

```python
def fuction(argument(, argument)*):
    ...
```

### 1.1. Annotate Type

- Annotation is expressions associated with indicated parts of functions
    - Stored as a `dict` from anntated parameter string to annotation expression
        - `<Function>.__annotaions__ == {'<Parameter>': <AnnotationExpression>, ..., 'return': <AnnotationExpression>}`
- Annotate argument's type: `argument: <Type>`
- Annotate return type: `def fuction(<Arguments>) -> <ReturnType>:`
- Function with type annotation:

```python
def <FunctionName>(<Argument>: <Type>(, <Argument>: <Type>)*) -> <ReturnType>:
    # ...
```

- Annotation is prefered over type docstr

## 2. Parameter

- Pass Reference, not Variable

```puml
[Original Variable] -> [Object]
[Parameter Variable] -> [Object]
```

### 2.1. Optional Parameter

- Define: `<Argument>=<DefaultValue>`
- Can provide value optionally when invoke

```python
def function(argument=<DefaultValue>):
    ...

fuction(argument)
fuction()  # Equivalent to fuction(<DefaultValue>)
```

- `<DefaultValue>` is a expression, and evaluated when the function defined

### 2.2. Keyword Parameter 关键字参数

- 在调用时通常按名称指定值: `fuction(argument=value)`

### 2.3. 指示形式参数

- 在定义函数时, 有以下两种特殊的指示形式参数
    - `/`: 之前的参数只能通过位置参数指定值 Positional Only, 不能通过关键字参数指定值
        - 名称不重要且总是以固定顺序指定值的参数可限定为仅限位置参数指定值 Positional Only
    - `*`: 标志可通过位置参数指定值的参数的结束, 之后的参数只能通过关键字参数指定值 Keyword Only, 不能通过位置参数指定值
        - 名称重要且有助于使函数调用更可读的参数可限定为仅限关键字参数指定值 Keyword Only

### 2.4. API Notation

- `[]`: 其中的参数是可选的

## 3. Object

- Function is Function Variable
    - Function is Variable which refer to fuction object
    - E.g.:
        - `def function()`
        - `function` is a variable
            - Refer to a function object
- Function Variable is Function
    - Function Variable can be used as Function: `function_variable()`

```python
def original_function_variable():
    ...
new_function_variable = original_function_variable
new_function_variable()
```

## 4. Built-in Functions

### 4.1. `input()`

- `input(prompt: str) -> str`
- Print `prompt` without additional ending then read from `stdin` and return as `str`

### 4.2. `print()`

- `print(object(, object)*[, sep=sep][, end=''][, file=file][, flush=True])`
    - Print `object` to `file`, separated by `sep` and followed by `end`
    - `object`: If is `str`, print as is; if is not `str`, print `str(object)`
    - `sep`: Default to `' '`
    - `end`: Default to `'\n'`
    - `file`: Default to `sys.stdout`
    - `flush`: Default to `False`

### 4.3. `all()`

- `all(iterable) -> bool`: 内置函数, 当`iterable`中所有元素都为真时返回真, 否则返回假
- `all(expression_list: iterable) -> bool`
- `all(<ListComprehensionWhitoutBracket>) -> bool`
    - E.g.: `all(expression(element) for element in iterable)`
- Return `True` if all element in `expression_list` is `True`, otherwise `False`
    - Short circuiting: Terminate on first `False`
    - `True` if `iterable` is empty

### 4.4. `any()`

- `any(iterable) -> bool`: 内置函数, 当`iterable`中任意元素为真时返回真, 否则返回假
- `any(expression_list: iterable) -> bool`
- `any(<ListComprehensionWhitoutBracket>) -> bool`
    - E.g.: `any(expression(element) for element in iterable)`
- Return `True` if any element in `expression_list` is `True`, otherwise `False`
    - Short circuiting: Terminate on first `True`
    - `False` if `iterable` is empty

### 4.5. `sorted()`

- `sorted(iterable, *, key=None, reverse=False) -> List`
- Argument
    - `key`: `KEY(ELEMENT: object) -> int`
        - Function extract sort key from each element
        - Sort with lambda function: `lambda <Element>: <SortKey>`
        - E.g.:
            - `sorted(['a', 'ab', 'abc'], key=len)`
            - `sorted(['a', 'ab', 'abc'], key=lambda string: len(string))`
    - `reverse`: `bool`: If sort reversely
- Return a NEW (default ascending) sorted list from iterable
- Stable

### 4.6. `max()`

- `max(iterable, *[, default=obj, key=func]) -> value`
- `max(arg1, arg2, *args, *[, key=func]) -> value`
- Return largest item
- Argument
    - `default`: Object to return if provided iterable is empty
    - `key`: `key(element: object) -> int`
        - Function extract sort key from each element
        - Sort with lambda function: `lambda <Element>: <SortKey>`

### 4.7. `callable()`

- `callable(object: object) -> boolean`
- Return if `objcet` is callable

### 4.8. `hasattr()`

- `hasattr(object: object, name: str) -> bool`
- Return `True` if `object` has attr `name`

### 4.9. `getattr()`

- `getattr(object: object, name: str(, default: object)?) -> object`

### 4.10. `setattr()`

- `setattr(object: object, name: str(, value: object)?) -> object`

### 4.11. `isinstance()`

- `isinstance(object: Object, classinfo: Class) -> bool`
- Return `True` if `object` is instance of `classinfo`

### 4.12. `issubclass()`

- `issubclass(class: Class, classinfo: Class) -> bool`
- Return `True` if `class` is subclass of `classinfo`

### 4.13. `chr()`

- `chr(char_unicode_code_point: int) -> str`
- Return `str` character of given character unicode code point

### 4.14. `ord()`

- `ord(char_str: str) -> int`
- Return character unicode code point of given `str` character

### 4.15. `reversed()`

- `reversed(Iterable) -> Iterator`
- 返回一个逆序迭代器

### 4.16. `abs()`

- `abs(number)`: 返回`number`的绝对值

### 4.17. `open()`

- `open(file, mode='[rwxabt+]', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)`: Open `file` and return a corresponding file object
    - `mode`
        - `r`: Read (default)
        - `w`: Write
        - `x`: Exclusive write, failing if file already exists
        - `a`: Appending
        - `b`: Binary mode
        - `t`: Text mode (default)
        - `+`: Read & Write
    - `encoding`: Usually `'utf-8'`
    - Returned object
        - Text mode: `io.TextIOWrapper`
        - Buffered Binary mode: `io.BufferedReader`, `io.BufferedWriter`, `io.BufferedRandom`
        - Unbuffered Binary mode: `io.FileIO`
        - `seek(OFFSET, whence=(SEEK_CUR|SEEK_END))` to move cursor

```python
with open(r'<FileDir>', r'<Mode>') as <FileObjectName>:
    # ...
```

### 4.18. `vars()`

- `vars([object])`: Return `__dict__` attribute for `object` (default to local scope)
