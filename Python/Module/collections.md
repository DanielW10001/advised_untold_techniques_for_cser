# collections

[TOC]

## 1. Class

### 1.1. `deque`

```python
from collections import deque

queue = deque(list)
queue.append(element)
queue[0]  # Peek
queue.popleft()  # Poll
```

- Support
    - Interation
        - Subscript Ref: `deque[-1]`
    - Pickling
    - `len(deque)`
    - `reversed(deque)`
    - `copy.copy(deque)`
    - `copy.deepcopy(deque)`
    - `element in deque`
    - `__add__()`
    - `__mul__()`
    - `__imul__()`

#### 1.1.1. Method

##### 1.1.1.1. `__init__()`

- `deque([init_content: iterable[, maxlen: int]])`
- Construct a new deque with `content` or empty if not provided
- `maxlen`: If provided, when add item after full, opposite end items are discarded

##### 1.1.1.2. `append()`

- Add `element` to the right end: `deque.append(element: object)`

##### 1.1.1.3. `appendleft()`

- Add `element` to the left end: `deque.appendleft(element: object)`

##### 1.1.1.4. `clear()`

- Remove all element: `deque.clear()`

##### 1.1.1.5. `copy()`

- Return a shallow copy: `deque.copy()`

##### 1.1.1.6. `count()`

- Count the number of appearence: `deque.count(element: object)`

##### 1.1.1.7. `extend()`

- Extend the right end: `deque.extend(content: iterable)`

##### 1.1.1.8. `extendleft()`

- Extend the left end: `deque.extendleft(content: iterable)`

##### 1.1.1.9. `index()`

- Return the first index of `element`: `deque.index(element: object[, start: int[, stop: int]])`

##### 1.1.1.10. `insert()`

- Insert `element` at `index`: `deque.insert(index: int, element: object)`

##### 1.1.1.11. `pop()`

- Remove and return right end element: `deque.pop()`

##### 1.1.1.12. `popleft()`

- Remove and return left end element: `deque.popleft()`

##### 1.1.1.13. `remove()`

- Remove the first occurrence of `element`: `deque.remove(element: object)`

##### 1.1.1.14. `reverse()`

- Reverse the deque: `deque.reverse()`

##### 1.1.1.15. `rotate()`

- Rotate `step` right: `deque.rotate(step: int)`
    - Rotate left if `step` is negative

#### 1.1.2. Attribute

- `maxlen`: Maximum size
