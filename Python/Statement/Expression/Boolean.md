# Boolean

## 1. Operator

### 1.1. 比较运算符

- 比较运算符: 用于执行比较, 运算结果是布尔值`bool`
    - `x == y`: 相等性检验: x等于y
    - `x != y`: 不等性检验: x不等于y
    - `x < y`: 小于性检验: x小于y
    - `x <= y`: 小于等于性检验: x小于或等于y
    - `x > y`: 大于性检验: x大于y
    - `x >= y`: 大于等于性检验: x大于或等于y
    - `x is y`: 相同性检验: x是y, x与y是同一个对象
    - `x is not y`: 不同性检验: x不是y, x与y是不同的对象
    - `x in y`: 成员资格检验: x是容器y的成员
    - `x not in y`: 非成员资格检验: x不是容器y的成员
- 不允许比较不兼容的类型
- 链式比较: 同时使用多个比较运算符: `value( op value)*`
    - 每个比较运算符与其相邻的两运算对象组成的条件均为真时, 链式比较才为真

#### 1.1.1. 相等运算符

- 检查两个对象是否相等: `var == var`
- 注意: `=`是赋值运算符, 用于修改而非比较值

#### 1.1.2. 相同运算符

- 相等与相同
    - 相等: 值, 结构, 状态相同
    - 相同: 是同一个对象
    - 值相等的两个变量可能引用不同对象; 引用相同对象的两个变量的值一定相等
- `==`相等与`is`相同不同
    - `==`: 相等性检验: 检查两对象是否相等, 而非相同
    - `is`: 相同性检验: 检查两对象是否相同, 而非相等
- 注意: 将`is`用于数值与字符串等不可变基本值时, 由于Python的内部处理方式, 结果是不可预测的

#### 1.1.3. 成员资格运算符

- 也可用于检验子串: `substring in string`

#### 1.1.4. 字符串和序列的比较

- 字符串根据字符的Unicode码点进行字典序比较
- `ord(one_character_string) -> int`: 返回单字符字符串中字符的Unicode码点
- `chr(unicode_code_point) -> str`: 返回Unicode码点对应的单字符字符串
- 注意: 大写字母的Unicode码点小于小写字母的Unicode码点
    - 可通过`str.lower()`进行忽略大小写的比较
- 序列根据元素值进行字典序比较
- 若序列的元素也是序列, 根据同样的规则进行递归比较

### 1.2. 布尔运算符

- 考虑到所有类型的值都可解释为真值, 所有表达式都返回可以解释为真值的值
- 布尔运算符用于组合真值
    - `and`: 且: 左右运算对象都为真时返回真, 否则返回假
    - `or`: 或: 左右运算对象都为假时返回假, 否则返回真
    - `not`: 非: 右运算对象为假时返回真, 否则返回假
- 优先级: `not` > `and` > `or`
- 短路逻辑 (延迟求值): 布尔运算符只做必要的计算
    - `x and y`: 若x为假, 返回x; 若x为真, 返回y
        - 返回关键值: 第一个假或最后一个真
        - 当x为假时, **y不会被执行**
    - `x or y`: 若x为真, 返回x; 若x为假, 返回y
        - 返回关键值: 第一个真或最后一个假
        - 当x为真时, **y不会被执行**
        - Can be used to set fall back value
            - `<Value> or <FallBackValue>`
            - If `<Value>` is not empty (and thus not False), return `<Value>`
            - If `<Value>` is empty (and thus is False), return `<FallBackValue>`
            - Thus , this expression always return a not empty (not False) value

```python
# Ordinary Way
if <Condition1>:
    function(<Condition1>)
else:
    function(<Condition2>)

# Short Circuit Way
function(<Condition1> or <Condition2>)
```

- `any(iterable) -> bool`: 内置函数, 当`iterable`中任意元素为真时返回真, 否则返回假
- `all(iterable) -> bool`: 内置函数, 当`iterable`中所有元素都为真时返回真, 否则返回假

## 2. 条件表达式

- 条件表达式: `true_value if condition else false_value`
    - 使用if与else确定值的表达式: 若条件为真, 则值为`true_value`, `false_value`不会被计算; 若条件为假, 则值为`false_value`, `true_value`不会被计算
    - 类似于C中的三目运算符`?:`
