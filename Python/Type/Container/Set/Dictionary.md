# Dictionary

- `Dict[<KeyType>, <ValueType>]`

## 1. Support

- `list(d)`: Key List
- `len(d)`: Number of Items
- `d[key]`: Ref to coresponde value item
    - Raise `KeyError` if `key` not presented
- `d[key] = value`: Set `d[key]` to `value`
- `del d[key]`: Remove `d[key]`
- `key (not)? in d`: Membership check
- `iter(d)`: Return a iterator over keys

## 2. Literal

- Curel Bracket: `{<Key>: <Value>(, <Key>: <Value>)*}`

## 3. Method

- `d.clear()`: Remove all items
- `d.copy()`: Return a shallow copy
- `@classmethod dict.fromkeys(iterable[, value])`: Create dictionary with each key from `iterable` and values set to `value`
    - `value`: Defualt `None`
- `d.get(key[, default])`: Return value of `key` or `default`
    - `default`: Default `None`
- `d.items()`: View of items
- `d.keys()`: View of keys
- `d.values()`: Views of values
- `d.pop(key[, default])`: If key presented, remove it and return its value, else return `default`
    - If `default` is not given and key not presented, raise `KeyError`
- `d.popitem()`: Remove and return `(key, value)` pair in Last In First Out order

## 4. Subclass

### 4.1. Dictionary View

#### 4.1.1. Support

- `len(dictview)`: Number of entries
- `iter(dictview)`: Iterator over entries
- `x in dictview`: Membership check
