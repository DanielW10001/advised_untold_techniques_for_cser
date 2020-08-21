# List

- `List[<ElementType>]`
- Implements Sequence Interface

## 1. Literal

- Square Brackets: `[(<Value>)?(, <Value>)*]`
- Notice: Do NOT define list with `[<InitialElement>] * <Length>`, which will lead to every element of the list refer to the same object. Instead, define list with `[<InitialElement> for i in range(<Length>)]`

## 2. Method

### 2.1. Append

- Append element to list: `list.append(element)`
- Change list **ITSELF**

### 2.2. Extend

- Extend by appending: `list.extend(iterable)`

### 2.3. Insert

- Intert before `index`: `list.insert(index: int, element: object)`

### 2.4. Remove

- Remove First Element whose value is equal to `element`: `list.remove(element: object)`

### 2.5. Pop

- Remove and return element at `<Index>`: `list.pop(<Index>)`
- Remove and return last element: `list.pop()`

### 2.6. Clear

- Remove all items: `list.clear()`

### 2.7. Index

- Return the first index of `<Object>`, or raise `ValueError` if not found: `list.index(<Object>[, start: int[, end: int]])`
    - `start`, `end`: Slice Index

### 2.8. Count

- Count appearence: `list.count(element: object)`

### 2.9. Sort

- `sort(*, key=None, reverse=False) -> None`
- Sort the list ITSELF ascendingly
- Argument
    - `key`: `key(element: object) -> int`
        - Function extract sort key from each element
        - Lambda function: `lambda <Element>: <SortKey>`
        - `operator`
            - `itemgetter(index: int) -> object`
                - Return item at `index` in iterable
            - `attrgetter(attr_name: str) -> object`
                - Return attribute named `attr_name` in object being sorted
            - Both function can receive multiple keys for multiple level of sorting
                - `(item|attr)getter(<Key>(, <Key>)*)`
                - Keys are received in descending order of weight.
    - `reverse`: If sorted reversed
- Stable

### 2.10. Reverse

- Reverse list: `list.reverse()`

### 2.11. Copy

- Return a shallow copy: `list.copy()`

## 3. Operation

### 3.1. Multiply

- Splice `<Number>` of `<List>` into one list: `<List> * <Number>`

### 3.2. Complehension

- 列表推导: 从其他可迭代对象创建列表

```python3
[expression(element(, element)*)(for inner_structure in external_structure( if condition(element(, element)*))?)+]
```

- 提取的先后顺序, 变量的作用域, 语句的执行次数, 新列表元素的排列顺序均与等价for语句相同
- `expression`可以使用之后出现的任何`inner_structure`作为`element`
- 每次遍历的`expression`将成为新列表的一个元素
- 每个`for-in(-if)?`表达式
    - 定义`inner_structure`, 并遍历`external_structure`中每一个`inner_structure`
    - 先出现的`for-in(-if)?`表达式包含后出现的`for-in(-if)?`表达式
    - 后出现的`for-in(-if)?`表达式
        - 类似于前一个`for-in(-if)?`表达式的for循环代码块的一部分
        - `external_structure`部分可以访问之前各个`for-in(-if)?`表达式定义的变量
        - `condition`部分可以访问之前各个及本个`for-in(-if)?`表达式定义的变量
        - 被执行前一个`for-in(-if)?`表达式的执行遍数*`len(external_structure)`遍
    - 从`inner_structure`中提取`element`的`for-in(-if)?`表达式应位于从`external_structure`中提取`inner_structure`的`for-in(-if)?`表达式之后

列表推导结构与等价for语句:

```python3
level_0_list = \  # Nested list
[  # Level 0
    [  # Level 1
        [  # Level 2
            # ...
        ],
        # ...
        [
            # ...
        ]
    ],
    # ...
    [
        # ...
    ]
]

new_list = \
[expression(level_1_list, level_2_list, ..., level_n_list, element)
    for level_1_list in level_0_list if condition(level_1_list)
        for level_2_list in level_1_list if condition(level_1_list, level_2_list)
            ...
                for element in level_n_list if condition(level_1_list, level_2_list, ..., level_n_list, element)
]

invalid_new_list = \
[expression(level_1_list, level_2_list, ..., level_n_list, element)
    for element in level_n_list if condition(level_1_list, level_2_list, ..., level_n_list, element)  # INVALID: level_1_list, ..., level_n_list not defined yet
        for level_n_list in level_n-1_list if condition(level_1_list, level_2_list, ..., level_n_list)  # INVALID: level_1_list, ..., level_n-1_list not defined yet
            ...
                for level_1_list in level_0_list if condition(level_1_list)
]

# Equivalent to
new_list = []
for level_1_list in level_0_list:
    if condition(level_1_list):
        for level_2_list in level_1_list:
            if condition(level_1_list, level_2_list):
                ...
                    for element in level_n_list:
                        if condition(level_1_list, level_2_list, ..., level_n_list, element):
                            new_list.append(expression(level_1_list, level_2_list, ..., level_n_list, element))
```

- 可用于将多层嵌套的可迭代结构展平 Flatten

## 4. Usage

### 4.1. Using List as Stack

```python
stack = [1, 2, 3]
stack.append(4)  # == stack.push(6)
stack[-1]  # == Last Element
stack.pop()  # == Last Element
```

### 4.2. Using List as Queue

- Not Recommend: O(n)
- Recommend `collections.deque` instead

```python
queue = [1, 2, 3]
queue.append(4)
queue[0]  # == First Element
queue.pop(0)  # == First Element
```
