# Method

## 1. Self

- `self`
- Allow method to access object indicated by implicit argument of invoke
- `self` is implicit argument
    - For`def bar(self)`, `foo.bar()` == `Foo.bar(foo)`
    - Invoke: `foo.bar()`
    - Access: `def bar(): self.dummy = None  # self == foo, self.dummy == foo.dummy`
- Do not need to indicate in explicit argument list when invoke
- Any object method need `self` argument

```python
class <ClassName>:
    def __init__(self(, <ExplicitArgument>)*):
        self.<Attribute> = <ExplicitArgument>
    def <MethodName>(self(, <ExplicitArgument>)*):
        self.<Attribute>...

def test():
    object = <ClassName>(<ExplicitArgument>)
    object.(<ExplicitArgument>)  # Implicit argument: object
```

## 2. Default Argument

- All instance's method will use the **SAME** default object
    - Lead to Shared Object Problem
    - Solution: use `<DefaultValue>.copy()`

```python
class <ClassName>:
    def <MethodName>(<Argument>=<DefaultValue>):
        <Usage>(<DefaultValue>.copy())  # Not <DefaultValue>
```

## 3. Class Method

- Use `@classmethod` to define
- Can be invoked on the class or an instance
- `cls`
    - Implicit argument
    - Reference the Class object
    - For`@classmethod def bar(cls)`, `Foo.bar()` == `bar(Foo)`
    - Invoke: `Foo.bar()`
    - Acess: `@classmethod def bar(cls): cls()  # cls == Foo, cls() == Foo()`
- Recommended Implementation of Java-like static factory method
    - Subclass inherit class method and receive subclass rather than supclass as implicit argument: `class SubDate(Date)`, `SubDate.from_string('11-09-2019')  # cls == SubDate != Date`

```python
class Date:
    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, date_as_string):  # Java-like static factory method. cls == <CalledClass>
        day, month, year = map(int, date_as_string.split('-'))
        date = cls(day, month, year)  # == <CalledClass>(...)
        return date

data = Date.from_string('11-09-2019')
```

- Invoke class method from bound method: `type(self).class_method(argument_list)`

## 4. Static Method

- Use `@staticmethod` to define
- Can be invoked on the class or an instance
- Do NOT receive the class as implicit first argument `cls`
    - Cannot access the class
- Can, but do NOT implement Java-like static factory method with static method. Use class method instead
    - Class is hard-coded, Inherited static method in subclass will create supclass instance rather than subclass one: `class SubDate(Date)`, `SubDate.from_string('11-09-2019')  # Return a Date instance, rather than SubDate one`

```python
class Date:
    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    @staticmethod
    def is_day_valid(date_as_string):
        day, month, year = map(int, date_as_string.plic('-'))
        return day <= 31 and month <= 12 and year <= 3999

    # WRONG WAY
    @staticmethod
    def from_string(date_as_string):  # WRONG Java-like static factory
        day, month, year = map(int, date_as_string.split('-'))
        date = Date(day, month, year)  # Date is hard coded, inherited method will create Date instance rather than SubDate one
        return date
```

- Invoke static method from bound method: `type(self).static_method(argument_list)`
