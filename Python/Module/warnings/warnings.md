# warnings

```python3
import warnings
warnings.warn(message(, category)?)  # Warning

warnings.filterwarnings('default|error|ignore'[, message][, category][, module][, lineno][, append])  # Filter Warning
warnings.resetwarnings()  # Restore Filter List
class CustomWarning(ExistWarning): pass  # Define new warning category
```

- 模块`warnings`提供了警告支持
- 警告通常用于提醒用户关于程序的值得注意但不必引发异常, 终止程序的特殊情况
- 通过`warn()`函数提出警告
- 警告信息的处理方式包括
    - 忽视
    - 通常写入`sys.stderr`
    - 转换为并引发异常
- 警告信息处理方式的决定取决于警告类型, 警告内容, 警告发出的源代码位置
- 默认情况下, 相同源代码位置处发出的警告信息仅打印一遍
- 警告控制包括两阶段
    - 首先, 每当一个警告提出时, 警告过滤器决定是否提出警告信息
    - 然后, 若需要, 通过可自定义的方式格式化并打印警告信息
- 警告过滤器: 匹配规则与处置规则所组成的的序列
    - 用于决定是否提出警告信息
- 通过`filterwarnings()`函数向警告过滤器添加规则
- 通过`resetwarnings()`将警告过滤器重置为默认状态
- 格式化与打印警告信息是通过调用`formatwarning()`与`showwarning()`函数完成的
    - 可以通过重写`formatwarning()`与`showwarning()`函数自定义格式化与打印过程
- `logging.captureWarnings(capture: bool) -> None`: 启用或禁用将警告捕获进日志
    - 警告将以`WARNING`等级记入`py.warnings`logger

## 1. 警告类型

有一些代表警告类型的异常类, 可用于设置警告的过滤规则.

警告类型必须是`Warning`类的子类

可以通过派生内置的警告类型来创建自定义的警告类型.

Warning categories that are primarily for Python developersrather than end users are ignored by default.

|           类型            |                                描述                                |
| :-----------------------: | :----------------------------------------------------------------: |
|          Warning          |                  Exception类的子类, 警告类型的基类                   |
|        UserWarning        |            用户提出的警告, `warn()`函数提出的警告的默认类型            |
|    DeprecationWarning     | 面向开发者的关于废弃特性的警告的基类. 除非在`__main__`中引发, 否则被忽视 |
|       SyntaxWarning       |                          可疑语法警告的基类                          |
|      RuntimeWarning       |                         可疑运行时警告的基类                         |
|       FutureWarning       |                  面向用户的关于废弃特性的警告的基类                   |
| PendingDeprecationWarning |                  将废弃的特性警告的基类, 默认被忽视                   |
|       ImportWarning       |                 导入模块时引发的警告的基类, 默认被忽视                 |
|      UnicodeWarning       |                        Unicode相关警告的基类                        |
|       BytesWarning        |                 `bytes`或`bytearray`相关警告的基类                  |
|      ResourceWarning      |                          资源相关警告的基类                          |

## 2. 警告过滤器

- 警告过滤器控制警告的处置方式, 包括: 忽视, 显示, 转换为并引发异常
- 警告过滤器维护一个过滤规则的有序列表
- 过滤规则是形如`(action, message, category, module, lineno)`的元组
- 任何警告都依序与过滤器中的过滤规则尝试匹配, 直到与某个过滤规则匹配
- If a warning is issued and does not mtch any registered filter, the default action is applied.
- 警告所匹配的过滤规则决定了对该警告的处置
- `action`: 下列字符串之一
    - `'default'`: 打印该源代码位置提出的第一次匹配警告
    - `'error'`: 转换为并引发异常: `raise WarningClass(message)`
    - `'ignore'`: 忽视
    - `'always'`: 打印
    - `'module'`: 打印该模块提出的第一次匹配警告
    - `'once'`: 打印第一次匹配警告
- `message`: A **string** containing a **regular expression** that the **start** of the warning message must match. The regular expression is compiled to always be **case-insensitive**.
- `category`: A Warning Class of which the warning must be instance in order to match
- `module`: A string containing a regular expression that the module name that issued warning must match. The expression is compiled to be case sensitive.
- `lineno`: An integer that the line number where the warning occurred must match, or `0` to match all line numbers.

### 2.1. Describing Warning Filters

- The wrnings filter is initialized by `-W` options passed to the Python interpreter command line and the `PYTHONWARNINGS` environment variable
- Invalid options will cause a message to `sys.stderr` and then be ignored.
- Filter listed later take precedence over those listed before them, as filters are applied left-to-right, and the most recently applied filters take precedence over earlier ones.
- Specify individual warnings filters with a sequence of fields separated by colons: `actionp[:message[:category[:module[:line]]]]`
- Separate multiple filters on a single line with commas

### 2.2. Default Warning Filter

Default warning filter, which can be overridden by `-W`, `PYTHONWARNINGS` or `filterwarnings`, in order of precedence

```plain
default::DeprecationWaring:__main__
ignore::DeprecationWarning
ignore::PendingDeprecationWarning
ignore::ImportWarning
ignore::ResourceWarning
```

### 2.3. Overriding Default Filter

- `sys.warnoptions` contains user passed filters

```python3
# Disable
import sys
if not sys.warnoptions:  # If no user defined filters
    import warnings
    warnings.simplefilter('ignore')

# Enable
import sys
if not sys.warnoptions:  # If no user defined filters
    import os, warnings
    warnings.simplefilter('default')  # Set for current process
    os.environ['PYTHONWARNINGS'] = 'default'  # Set for subprocess

# Enable DeprecationWarning for interactive_module
import warnings
warnings.filterwarnings('default', category=DeprecationWarning, module=interactive_module.get('__name__'))
```

### 2.4. Temporarily Suppressing Warnings

```python3
import warnings
with warnings.catch_warnings():
    warnings.simplefilter('ignore')
    # Critical Code
```

### 2.5. Testing Warnings

```python3
import warnings
with warnings.catch_warnings(record=True) as warning_list:
    warnings.simplefilter('always')
    # Critical Code
    # Test warning
    assert test(warning_list)
```

- Because DeprecationWarning is ignored by default, developers should test code with DeprecationWarning explicitly made visible to get notifications of outdated API

## 3. Fuction

- `warnings.warn(message, category=None, stacklevel=1, source=None)`: Issue a warning
    - `message`: Message string or a `Warning` instance
        - If `message` is a `Warning` instance, message text will be `str(message)`
    - `category`: `UserWarning` if not given
        - If `message` is a `Warning` instance, `category` will be ignored and `message.__class__` will be used
    - This fuction raises an exception if this warning is transtered into an error
    - `stacklevel`: `stacklevel - 1` step up in invoke stack before issue the warning. Used by wrapper function to raise warning.
    - `source`: If supplied, is the destroyed object which emitted a `ResourceWarning`
- `warnings.showwarning(message, category, filename, lineno, file=None, line=None)`: Write a warning to a file
    - The default implementation calls `formatwarning(message, category, filename, lineno, line)` and writes the resulting string to file, which defaults to `sys.stderr`.
    - May replace this fuction by assigning to `warnings.showwarning`
    - `line` is a line of source code to be included in the warning message; if `line` is not supplied, `showwarning()` will try to read the line specified by `filename` and `lineno`.
- `warnings.formatwarning(message, category, filename, lineno, line=None) -> str`: Format a warning.
    - `line` is a line of source code to be included in the warning message; if `line` is not supplied, `formatwarning()` will try to read the line specified by `filename` and `lineno`.
- `warnings.filterwarnings(action, message='', category=Warning, module='', lineno=0, append=False)`: Insert an filter entry into the list of filter list.
    - The entry is inserted as a tuple at the front by default (most precedent); if `append` is true, it is inserted at the end (last precedent).
    - Omitted arguments default to a value that matches everything.
- `warnings.simplefilter(action, category=Warning, lineno=0, append=False)`: Insert a simple filter entry into filter list.
    - Similar to `warnings.filterwarnings()` except omitted `message` and `module`, which default to a value that matches everything.
- `warnings.resetwarnings()`: Reset filter list to default.

## 4. Context Manager

- `class warnings.catch_warnings(*, record=False, module=None)`
    - A context manager that copies and, upon exit, restores the warnings filter and the `showwarning()` function.
    - If the record argument is `False` (the default) the context manager returns `None` on entry. If record is `True`, a list is returned that is progressively populated with objects as seen by a custom `showwarning()` function (which also suppresses output to `sys.stdout`).
    - Each object in the list has attributes with the same names as the arguments to `showwarning()`.
    - Once the context manager exits, the warnings filter and showwarning() function are restored to its state when the context was entered.
    - Note: Multi-threading program's behavior of `catch_warnings` is undefined