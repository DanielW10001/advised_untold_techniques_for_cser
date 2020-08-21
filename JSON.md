# JSON

- JavaScript Object Notation: Data Interchange Format
- Subset of JavaScript
- Support three types of value
    - Object, Mapping, Hashes, Dictionary: Set of key-value pairs
    - Array, Sequence, List: Ordered list of values
    - Scalars: Single not-dividable value

## 1. Object

- Object, Mapping, Hashes, Dictionary: Set of key-value pairs
- Set of key-value pairs in braces
- Key and value are separated by colon
- Key-value pairs are separated by comma

```json
{KEY: VALUE(, KEY: VALUE)*}
```

![Object](_v_images/20200120165615959_29473.png)

## 2. Array

- Array, Sequence, List: Ordered list of values
- Ordered list in brackets
- Values are separated by comma

```json
[VALUE(, VALUE)+]
```

![Array](_v_images/20200120165958633_8966.png)

## 3. Scalars

- Scalars: Single not-dividable value
- String: Text in double quotes: `"CONTENT"`
    - Use backslash escape
- Number: `-?INTEGER(.INTEGER)?[eE][-+]?INTEGER`
- Boolean: `true|false`
- Null: `null`

![Value](_v_images/20200120170009284_10634.png)

![String](_v_images/20200120170118540_23120.png)

![Number](_v_images/20200120170318538_9789.png)

## 4. White Space

- Can be inserted between any pair of tokens

![Whitespace](_v_images/20200120170405652_26159.png)
