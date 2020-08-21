# 静态类型标记 Type Hint

## 1. 类型与类型继承

- 类型 Type: 数据与访问数据的函数的集合
- 注意区分类与类型
    - 类 Class: Python运行时概念, 由`class`语句定义
    - 类型 Type: Language Server概念, 由类和类型别名等定义
- 子类型 Subtype: 若对类型T与类型U以下两条都成立, 则称T为U的子类型, U为T的超类型, 称T继承U
    - T的每个实例都是或可以转换为U的实例
    - U的每个函数都是T的函数
- 子类型实例可以被用作超类型的实例
- 子类一定是超类的子类型, 子类型不一定是超类型的子类
- 复合类型与类型继承关系
    - 协变式: 复合类型与所使用类型的继承关系相同: 若T继承U, 则`Tuple[T]`继承`Tuple[U]`
    - 无关式: 复合类型与所使用类型的继承关系无关: 若T继承U, 则`List[T]`与`List[U]`无关
    - 逆变式: 复合类型与所使用类型的继承关系相反: 若T继承U, 则`Callable[[U], ...]`继承`Callable[[T], ...]`

## 2. 使用类型标记与类型别名

- 为代码补全, IDE与类型检查提供对象应是的类型信息

```python3
variable(: Type)? = value  # 给变量作标记

def fuction((argument(: Type)?( = default_value)?)*) -> ReturnType:  # 给参数与返回值做标记
    # ...
```

- 通过方括号指定类型所使用的类型, 如容器的元素类型, 可调用类型的参数和返回值类型等
- 对内置容器类型
    - 容器类型本身`list`, `dict`, `tuple`不能指定元素类型
        - 例: 不合法: `list[int]`
    - 使用`typing`模块中的`List`, `Dict`, `Tuple`代替, 可以通过方括号指定元素类型
        - 例: 合法: `List[int]`
- 为单星号或双星号参数提供类型标记时, 提供其元素或值的类型而非参数本身的类型
    - 如`*args: str`, `**kwargs: int`而非`*args: Tuple[str]`, `**kwargs: Dict[str, int]`
- 当`Type`尚未定义/导入时, 可以使用`'Type'`

实例:

```python3
class Type:
    def get_value(self) -> Type:  # Error: Type not defined (yet)
    def get_value(self) -> 'Type':  # Error: Type not defined (yet)
```

- 为避免循环导入, 可将为类型标记而做的导入放入`typing.TYPE_CHECKING`条件中, 同时使用字符串类型标记`'Type'` :

```python3
if TYPE_CHECKING:
    from module import Type

def function(arguemnt: 'Type') -> None:
    # ...
```

- 通过赋值创建类型别名: `Alias = Type`
    - 类型别名也是类型, 也可用于类型标记中
    - 可用于简化复杂类型的标记

## 3. 类型变量与泛型

- 类型变量: 用于在泛型类中接收方括号类型参数所指定的类型的变量, 或用作随变量实际类型而变的泛型类型
- 通过`typing.TypeVar`函数创建类型变量: `Type = TypeVar('Type')`
- 使用类型变量与其他泛型类型 (如`typing.Generic`) 来允许泛型类接收类型标记中的类型参数

```python3
from typing import TypeVar, Generic

Type = TypeVar('Type')

class Class(Generic[Type(, Type)*]):
    # Define Class with `Type` as a type
```

- 对于允许接收类型标记中类型参数的泛型类型, 可在类型标记中通过方括号指定泛型类型所使用的类型: `Class[Type(, Type)*]`

实例:

```python3
from typing import TypeVar, Generic

T = TypeVar('T')

class Wrapper(Generic[T]):
    def __init__(self, value: T) -> None:
        self.value: T = value
    def get(self) -> T:
        return self.value

def function(wrapper_of_str: Wrapper[str]) -> None:
    print(wrapper_of_str.get())

function(Wrapper("Hello!"))
```

- 使用类型变量作为随变量实际类型而变的泛型类型:

实例:

```python3
from typing import TypeVar

T = TypeVar('T')

def echo(var: T) -> T:
    return var

echo('Hello!')
```

注意:

- 作为泛型类型的类型变量随变量的实际类型而变
    - 当参数`var`接收到的值的实际类型为`str`时, 类型变量`T`就为`str`
    - 因此, `echo('Hello')`的返回值为`T`, 也就是`str`
- 相比于使用`Any`作为泛型类型标记, 使用类型变量保留了更多信息: 如`echo`的返回值与参数值类型相同这一信息

## 4. 标记类型

- `typing`模块提供了用于类型标记的类型
- `Any`: 任何类型
- `Type[TYPE]`: `TYPE`类型对象本身, 即类对象`TYPE`
    - 注意: 标记`TYPE`接收`TYPE`类型的实例`type`, 标记`Type[TYPE]`接收类对象`TYPE`, 即`type.__class__`
- `Callable[[(ArgumentType(, ArgumentType)*)?], (ReturnType)?]`: 参数为`(ArgumentType(, ArgumentType)*)?`, 返回值为`(ReturnType)?`的可调用对象
- `Callable[..., (ReturnType)?]`: 返回值为`(ReturnType)?`的可调用对象
- `Iterable[ElementType]`: 元素为`ElementType`的可迭代对象
- `AsyncIterable[ElementType]`: 元素为`ElementType`的异步可迭代对象
- `Iterator[ElementType]`: 元素为`ElementType`的迭代器
- `AsyncIterator[ElementType]`: 元素为`ElementType`的异步迭代器
- `SupportsInt`: 实现了`__int__`方法的类`int`对象
- `SupportsFloat`: 实现了`__float__`方法的类`float`对象
- `SupportsComplex`: 实现了`__complex__`方法的类复数对象
- `SupportsBytes`: 实现了`__bytes__`方法的类`bytes`对象
- `SupportsIndex`实现了`__index__`方法的对象
- `Container[ElementType]`: 元素为`ElementType`的容器
- `Collection[ElementType]`: 元素为`ElementType`的集合
- `AbstractSet[ElementType]`: 元素为`ElementType`的抽象集合
- `MutableSet[ElementType]`: 元素为`ElementType`的可变集合
- `Mapping[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的映射
- `MutableMapping[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的可变映射
- `Sequence[ElementType]`: 元素为`ElementType`的序列
- `MutableSequence[ElementType]`: 元素为`ElementType`的可变序列
- `Deque[ElementType]`: 元素为`ElementType`的`deque`
- `List[ElementType]`: 元素为`ElementType`的列表
- `Set[ElementType]`: 元素为`ElementType`的集合
- `FrozenSet[ElementType]`: 元素为`ElementType`的冻结集合
- `KeysView[KeyType]`: 键为`KeyType`的键视图
- `ItemsView[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的项视图
- `ValuesView[ValueType]`: 值为`ValueType`的值视图
- `ContextManager[ResourceType]`: 资源为`ResourceType`的上下文管理器
- `AsyncContextManager[ResourceType]`: 资源为`ResourceType`的异步上下文管理器
- `Dict[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的字典
- `DefaultDict[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的默认字典
- `OrderedDict[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的有序字典
- `Counter[ElementType]`: 元素为`ElementType`的`Counter`
- `ChainMap[KeyType, ValueType]`: 键为`KeyType`, 值为`ValueType`的链接映射
- `Generator[YieldType, SendType, ReturnType]`: `yield`值为`YieldType`, `send`值为`SendType`, 返回值为`ReturnType`的生成器
- `AsyncGenerator[YieldType, SendType, ReturnType]`: `yield`值为`YieldType`, `send`值为`SendType`, 返回值为`ReturnType`的异步生成器
- `NoReturn`: 标记函数永不正常退出
- `Union[Type(, Type)*]`: `Type(, Type)*`中的任意一种类型
- `Optional[Type]`: `Type`类型或`None`类型
- `Tuple[Type(, Type)*]`: 第1, 第2, ..., 第n个元素分别为`Type`, `Type`, ..., `Type`类型的元组
- `Tuple[Type, ...]`: 元素为`Type`类型的元组
- `Literal[value(, value)*]`: 值为`value(, value)*`中任意一个的字面量
- `Final`: 不能被赋值或屏蔽的变量
- `Pattern[(str|bytes)]`: `re.Pattern`对象
- `Match[(str|bytes)]`: `re.Match`对象
- `AnyStr`: 类`str`对象
