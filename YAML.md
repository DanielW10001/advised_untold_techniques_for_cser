# YAML

- Yet Another Markup Language: 常用于书写配置文件
- 文件后缀名: `.yaml`, `.yml`
- 特性
    - 方便人类读写
    - 兼容JSON: 流式格式 Flow Form 就是JSON
- 标识符大小写敏感
- 缩进代表层级关系
    - 只能使用空格, 不能使用Tab
    - 空格数越少, 层级越高 (浅)
    - 相同空格数的缩进代表相同层级
- 大括号代表层级关系, 大括号中的逗号分隔不同域
- 注释: 从`#`开始直到末尾的内容作为注释被忽略
- 支持三种值
    - 对象 Object, 映射 Mapping, 哈希 Hashes, 字典 Dictionary: 键值对集合
    - 数组 Array, 序列 Sequence, 列表 List: 一组线性排列的值
    - 纯量 Scalars: 单个不可再分的值

## 1. 对象

- 对象 Object, 映射 Mapping, 哈希 Hashes, 字典 Dictionary: 键值对集合
- 普通格式: 相同缩进下的键值对集合
- 流式格式: 同一对大括号中的键值对集合
- 键值对用冒号分隔, 冒号左侧是键, 右侧是值
- 键可以是标识符
- 值可以是对象, 数组或纯量
- 最外层/顶层的对象可以没有缩进或大括号

```yaml
# Normal Form
(KEY: VALUE
)+

# Flow Form
{?KEY: VALUE(, KEY: VALUE)*}?
```

- 复杂对象: 问号与冒号标识的键与值
    - 此时, 键除了可以是标识符外, 还可以是对象, 数组或纯量

```yaml
? KEY
: VALUE
```

- `<<: VALUE`: 将右侧值的所有键值对合并到本对象中, 后出现的键值对覆盖先出现的键值对

## 2. 数组

- 数组 Array, 序列 Sequence, 列表 List: 一组线性排列的值
- 普通格式: 相同缩进下的无序值集合
- 流式格式: 同一对中括号中的值集合
- 值可以是对象, 数组或纯量
- 最外层/顶层的数组可以没有缩进 (普通格式), 但必须有中括号 (流式格式)

```yaml
# Normal Form
(- VALUE
)+

# Flow Form
[VALUE(, VALUE)*]
```

## 3. 纯量

- 纯量 Scalars: 单个不可再分的值
- 字符串 String: (引号括起来的) 文本: `('|")?(\||>)?(-|+)?CONTENT('|")?`
    - 可以跨越多行
    - 缩进会被忽略
    - 引号代表特殊字符处理模式
        - 默认: 空格作为分隔符, 反斜杠作为字面字符
            - 使用单引号表示作为字面字符的单引号: `'`
            - 使用双引号表示作为字面字符的双引号: `"`
        - `'`: 空格作为字面字符, 反斜杠作为字面字符
            - 使用连续两个单引号表示作为字面字符的单引号: `''`
            - 使用双引号表示作为字面字符的双引号: `"`
        - `"`: 空格作为字面字符, 反斜杠作为转义字符
            - 使用单引号表示作为字面字符的单引号: `'`
            - 使用连续两个双引号表示作为字面字符的双引号: `""`
    - 文本开头的符号代表处理模式
        - 换行符处理
            - `|`: 保留换行符
            - 默认, `>`: 换行符转换为空格
        - 末尾换行符处理
            - 默认: 保留至多一个末尾换行符
            - `+`: 保留全部末尾换行符
            - `-`: 忽略末尾换行符
- 布尔值: `true|false`
- 整数
    - 八进制: `0oINTEGER`
    - 十进制: `(-|+)?INTEGER`
    - 十六进制: `0xINTEGER`
- 浮点数: `(-|+)?INTEGER(.INTEGER)?((e|E)(-|+)?INTEGER)?`
    - `(-|+)?.inf`: Infinity
    - `.nan`: Not a Number
- Null: `~`, `null`
    - 相当于JSON中的`null`
- 日期与时间: `日期(T|t)时间(+|-)时区`: `YYYY-MM-DD(T|t)HH:MM:SS.SS(+|-)HH:MM`
- 日期: `YYYY-MM-DD`
- Tag Property: `TAG VALUE`
- Typed Value: `TYPE VALUE`
    - `TYPE`:
        - `!`: Default type (`!!map`, `!!seq`, `!!str`)
        - `!TYPE`, `!<TYPE>`, `!!TYPE`: 强制类型转换
            - `TYPE`: `str`, `map`, `seq`, `set`
        - `!TAG!TYPE`: Requires `%TAG !TAG! PREFIX` and means `PREFIXTYPE`

## 4. 锚点

- `&REFERENCE_IDENTIFIER VALUE`: 设置可以引用`VALUE`的锚点
- `*REFERENCE_IDENTIFIER`: 引用锚点对应的`VALUE`

## 5. 文档

- `%DIRECTIVE`: Directive Indicator
    - `%YAML VERSION`
    - `%TAG TAG_HANDLE PREFIX`
- `---`: 文档的开始
- `...`: 文档的结束
