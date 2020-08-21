# Chapt.9 魔法方法, 特性和迭代器

- 魔法: Python内部机制

## 1. 下划线与名称

- 名称为单下划线: `_`: 零时变量
- 名称以单下划线开头: `_attribute`: 弱私有属性
    - 能从外部访问: `object._attribute`
        - 名称不会被修改
    - 不会被`from module import *`导入
    - 从对象内部可以访问: `self._attribute`
- 名称以双下划线开头: `__attribute`: (强) 私有属性
    - 不能从外部访问
        - 实现方式: 在类定义中对所有以双下划线打头的名称都进行转换, 在开头加上单下划线和类名: `_Class__attribute`
        - 了解实现方式后, 可以从对象外访问私有方法, 但应避免这样做: `object._Class__attribute`
    - 不会被`from module import *`导入
    - 从对象内部可以访问: `self.__attribute`
    - 类似于其它语言中的标准私有属性
- 名称以按下划线结尾: `variable_`: 合适名称被占用时的替代名称, 如`class_`, `def_`
- 名称以双下划线开头和结尾: `__name__`: 魔法变量: Python保留的特殊变量
    - 与双下划线开头的`__attribute`不同, 以双下划线开头和结尾的名称`__name__`不会被修改, 在变量外部也可以正常访问: `object.__name__`
    - 不会被`from module import *`导入
    - 从对象内部可以访问: `self.__name__`
    - 不应该在程序中自己创建这样的名称
    - 如果对象实现了魔法方法, 它们将在特定情况下被Python调用, 而几乎不需要直接调用
