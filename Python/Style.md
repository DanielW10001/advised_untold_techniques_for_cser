# Style

[TOC]

## 1. Tamplet

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""<ModelAbstract>

<ModelDescribtion>
"""

__author__ = "<Author Name> (<Email>)"  # Module Metadata
__version__ = "<Version>"
__license__ = "<Lincense>"
__docformat__ = 'reStructuredText'

import ...

<MODULE_CONSTANT>: <Type> = <Value>
""":var <MODULE_CONSTANT>: <Describtion>
    ...
"""


class <Class>:
    """<ClassAbstract>

    <ClassDescribtion>
    """

    <Variable>: <Type> = <Value>
    """:var <Variable>: <VariableDescribtion>
        ...
    """

    def <Method>(<Argument>: <Type>(, <Argument>: <Type>)*) -> <Type>:
        """<MethodAbstract>

        <MethodDescribtion>

        :param <Argument>: <ArgumentDescribtion>
        :param <Argument>: <Argument
            Describtion>

        :return: <ReturnObjectDescribtion>

        :raise <Exception>: <ExceptionDescribtion>

        :Example:
            <Example>

        .. Reference:: <Reference>
        .. Warning:: <Warning>
        .. Note:: <Note>
        .. TODO:: (<Name or Email>): <ToDoDescribetion> before/after <Date or Event>
        """

        # <BlockComment>
        # <BlockComment>
        ...  # <InlineComment>

        # TODO(<Name or Email>): <ToDoDescribetion> before/after <Date or Event>


    def <Method>(<Argument>: <Type>(, <Argument>: <Type>)*) -> <Type>:
        """One line docstr"""
        ...


def test() -> None:
    ...


if __name__ == '__main__':
    test()
```

## 2. Identifier

|            Type            |     Identifier     |
| :------------------------: | :----------------: |
|          Modules           |  lower_with_under  |
|          Packages          |  lower_with_under  |
|          Classes           |      CapWords      |
|         Exceptions         |      CapWords      |
|         Functions          | lower_with_under() |
|   Global/Class Constants   |  CAPS_WITH_UNDER   |
|   Global/Class Variables   |  lower_with_under  |
|     Instance Variables     |  lower_with_under  |
|        Method Names        | lower_with_under() |
| Function/Method Parameters |  lower_with_under  |
|      Local Variables       |  lower_with_under  |

## 3. Indentency

- Multiline:

```python
function(<Argument>,
         <Argument>,
         ...)
... [<Element>,
     <Element>,
     ...]
```

- Escape line ends
    - Lines in `()`, `[]`, `{}` will be joined automaticly, no manually escape needed

```python
...\
...
(...
...)
[...
...]
{...
...}
```

## 4. `__doc__`

- 文档字符串
- 包, 模块, 类, 函数**内**的第一条语句, 且是字符串字面量
- 采用reStructuredText格式
    - 插入LaTeX公式: `:math:`<LaTeXCode>``

## 5. Multiline String

```python3
('CONTENT'(
 'CONTENT')+
)
```

## 6. Reference
