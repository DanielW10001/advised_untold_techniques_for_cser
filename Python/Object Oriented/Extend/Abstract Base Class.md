# Abstract Base Class

[TOC]

- 不应实例化
- 定义子类应该实现的方法
- 使用`@abstractmethod`标记抽象方法

```python
from abc import ABC, abstractmethod

class Talker(ABC):
    @abstractmethod
    def talk(self):
        pass

class Knigget(Talker):
    def talk(self):
        print("Ni!")
```
