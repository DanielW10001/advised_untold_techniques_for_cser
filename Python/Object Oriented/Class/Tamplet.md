# Tamplet

```python
class Person(Creature):

    class_var_name = ""  # Class Var

    def __init__(self, name):  # Constructor
        self.__private_name = name

    @classmethod
    def generate(cls, name):  # Java-like Static Factory Method
        return cls(name)

    def __privately_set_name(self, name):  # Private Method
        self.__private_name = name

    def set_name(self, name):  # Public Method
        self.name = name

    def get_name(self):
        return self.name
```