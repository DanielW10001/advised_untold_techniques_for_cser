# Re(gex) Note

re is the regex package in standard python library.

- PCRE (Perl Compatible Regular Expression)
- 支持str与bytes类型
- 推荐使用原始字符串`r"<String>"`书写Regex
- 修改方法总返回字符串副本而非就地修改

```python
import re

# target可以是字符串或文本文件
re.search(r"<Regex>", target) # 在target中查找匹配
re.match(r"<Regex>", target) # 在target中查找以target下标0为起始位置的匹配子串
re.full_match(r"<Regex>", target) # 检查target是否全匹配
result = re.sub(r'REGEX', 'REPLACEMENT', target)
```

## 1. 概念
- 字符
- 位置: 字符之间

## 2. 语法
- 为避免歧义, 重复修饰符 (RepetitionQualifier) (*, +, ?, {m,n}, 等) 不能直接嵌套, 应使用`(?:<Pattern><RepeQua>)<RepeQua>`的形式嵌套使用
- 消费: 目标串字符被视为已匹配
    - 空串消费
        - 当成功匹配到空串时, 下一次匹配不会再次匹配到该位置的空串
        - 即不会匹配相邻空串
- 捕获: 内容存入栈中, 可通过`\<Number>`引用
- `.`
    - 默认: 匹配除换行符外的任意字符
    - `DOTALL`:  匹配任意字符 (含换行符)
- `^`
    - 默认: 匹配字符串的开头位置
    - `MULTILINE`: 匹配行首位置
- `$`
    - 默认: 匹配字符串的末尾位置, **不匹配行尾位置**
    - `MULTILINE`: 匹配行尾位置
    - 注意: 当心字符串的末尾换行, 这将导致一个末尾空行
- `*`
    - 贪婪匹配前一个字符0到任意次
- `*?`
    - 厌恶匹配前一个字符0到任意次
- `+`
    - 贪婪匹配前一个字符1到任意次
- `+?`
    - 厌恶匹配前一个字符1到任意次
- `?`
    - 贪婪匹配前一个字符0到1次
- `??`
    - 厌恶匹配前一个字符0到1次
- `{m}`
    - (贪婪) 匹配前一个字符m次
- `{m, n}`
    - 贪婪匹配前一个字符m到n次
- `{m, n}?`
    - 厌恶匹配前一个字符m到n次
- `\`
    - `\<Number>`
        - 引用捕获组内容, 匹配与已经匹配的`<Number>`捕获匹配组的匹配内容相同的子串
        - `<Number>`<=99
            - 若`<Number>`首位为0, 或是三位八进制数, 则将被视为字符的码元而非捕获组引用
        - 在`[]`字符组内, 任何数字转义都将被视为字符的码元而非捕获组引用
    - `\<Letter>`
        - `\A`: 匹配字符串的开始位置
        - `\b`: 匹配单词 (word) 边界的位置
        - `\B`: 匹配非单词 (word) 边界的位置
        - `\d`: 匹配数字字符
        - `\D`: 匹配非数字字符
        - `\s`: 匹配空白字符
        - `\S`: 匹配非空白字符
        - `\w`: 匹配词语 (数字, 字母与下划线) 字符
        - `\W`: 匹配非词语 (数字, 字母与下划线) 字符
        - `\Z`: 匹配字符串末尾位置
        - 标准Python非打印字符: `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\u`, `\U`, `\v`, `\x`, `\\`
            - 注意: 由于`\b`被用于匹配单词边界, 它仅在字符组`[\b]`中匹配退格
        - Unicode转义序列: `\uXXXX`, `\UXXXXXXXX`
        - ASCII转义序列: `\<Number>`, 仅在`<Number>`首位为0, 或是三位八进制数时
    - `\<Character>`
        - 转义 (除数字和字母外的) 任何字符为普通字符
        - 若`<Character>`不是数字或字母, 则匹配`<Character>`本身
            - 对于普通字符, 这意味着匹配普通字符本身
            - 对于元字符, 这意味着将特殊字符转义为普通字符, 即消除其特殊含义, 匹配该字符本身
    - 注意: 注意区分Python转义与Regex转义: `"\n"` (而非`r"\n"`) 是一个 (Regex) 普通 (非打印) 字符
- `[]`
    - 表示字符组
    - `[<Character>...]`: 匹配列出字符中的任意一个字符
    - `-`
        - `[a-z]`: 匹配字符范围中的任意一个字符
        - 若需要匹配`-`字符本身
            - 置于字符组首或尾: `[-a]`, `[a-]`
            - 转义: `[a\-b]`
    - `^`
        - `[^<Characer>...]`: 反字符组, 匹配字符组外的任意一个字符
        - 若`^`不在字符组首位, 则失去特殊意义, 成为普通字符
    - 接受字符类 (如`\w`) 作为元素
    - 除`\`, `^`, `-`, `]`以外的其余元字符皆失去特殊意义, 成为普通字符
- `|`
    - 短路或匹配: 从左到右依次匹配, 一旦某个模式匹配成功, 则整个或表达式匹配成功, 右侧模式不再尝试匹配
- `()`
    - 创建匹配组
        - 捕获匹配组: 消费, 捕获
            - 0号捕获匹配组是整个正则表达式本身
            - 捕获匹配组按左括号顺序从左到右, 从1开始编号
            - 同一个捕获组匹配多次, 则只捕获最后一次匹配的内容
            - `(<Pattern>)`
                - 创建捕获匹配组
                - 使用`\<Number>`引用捕获组内容
                    - 可用于替换或**匹配**
            - `(?P<<Name>><Pattern>)`
                - 创建命名捕获匹配组
                - 匹配组名必须是有效的Python标识符, 且与匹配组必须一一对应
                - 参与捕获匹配组引用计数
                - 引用
                    - 正则表达式内
                        - `(?P=<Name>)`
                        - `\<Number>`
                    - 匹配对象
                        - 获取内容: `match.group("<Name>")`
                        - 获取匹配子串起始索引: `match.begin("<Name>")`
                        - 获取匹配子串结束索引: `match.end("<Name>")`
                    - `re.sub()`的`repl`参数
                        - `\g<<Name>>`
                        - `\g<<Number>>`
                        - `\<Number>`
        - 非捕获匹配组: 消费, 不捕获
            - `(?:<Pattern>)`
                - 创建非捕获匹配组
            - `(?<Flag>...<Pattern>)`
                - 创建非捕获匹配组, 并为本匹配组设置标记
                - Flag: 设置标记, 等效于向 `re.compile()` 传递 `flag` 参数
                    - `a` re.A: 只匹配ASCII字符
                    - `i` re.I: 忽略大小写
                    - `L` re.L: 语言依赖
                    - `m` re.M: 多行模式
                    - `s` re.S: `.`匹配任意字符 (含换行符)
                    - `u` re.U: Unicode匹配
                    - `x` re.X: 冗长模式
            - `(?[<Flag>...][-<Flag>...]:<Pattern>)`
                - 创建非捕获匹配组, 并为本匹配组设置或去除标记
            - `(?P=<Name>)`
                - 创建非捕获匹配组, 匹配与已经匹配的`<Name>`命名捕获匹配组的匹配内容相同的子串
            - `(?(<Number>|<Name>)<TruePattern>[|<FalsePattern>])`
                - 条件非捕获匹配组, 若`<Number>`捕获组或`<Name>`捕获组匹配成功, 则尝试匹配`<TruePattern>`, 否则尝试匹配`<FalsePattern>` (若给出)
        - `(?#<Comment>)`: 不消费, 不捕获
            - 注释, 被忽略
        - 零宽断言: 不消费, 不捕获
            - `(?=<Pattern>)`
                - 先行正向匹配零宽断言, 匹配 之后的字符串匹配`<Pattern>` 的位置
            - `(?!<Pattern>)`
                - 先行反向匹配零宽断言, 匹配 之后的字符串不匹配`<Pattern>` 的位置
            - `(?<=<Pattern>)`
                - 后行正向匹配零宽断言, 匹配 之前的字符串匹配`<Pattern>` 的位置
                - `<Pattern>`必须是定长模式, 不支持`*`, `+`等不定长模式
                    - `regex`模块支持变长后行正向匹配零宽断言
                    - `(?<=^|CHAR)PATTERN` -> `(?![^CHAR])PATTERN`, `(?:(?<=^)|(?<=CHAR))PATTERN` or `(?:^|CHAR)(?P<NAME>PATTERN)`
            - `(?<!<Pattern>)`
                - 后行反向匹配零宽断言, 匹配 之前的字符串不匹配`<Pattern>` 的位置
                - `<Pattern>`必须是定长模式, 不支持`*`, `+`等不定长模式
                    - `regex`模块支持变长后行反向匹配零宽断言
                    - `(?<!^|CHAR)PATTERN` -> `(?<=[^CHAR])PATTERN`, `(?<!^)(?<!CHAR)PATTERN` or `[^CHAR](?P<NAME>PATTERN)`
                - 注意: 在字符串的首位置, 后行尝试匹配的是空串

## 3. Module
### 3.1. Method

- `<Pattern>`
    - 可以是正则表达式字符串或正则表达式对象
- 修改方法总返回字符串副本而非就地修改
- `re.compile(<Pattern>[, flags = <Flag>[|<Flag>]...])`
    - 将正则表达式字符串编译为正则表达式对象
    - `<Flag>`
        - `re.A`, `re.ASCII` (`(?a)`): ASCII匹配 (而非Unicode)
        - `re.DEBUG`: 显示编译Debug信息
        - `re.I`, `re.IGNORECASE` (`(?i)`): 大小写不敏感匹配
        - `re.L`, `re.LOCALE` (`(?L)`): 本地化语言匹配, 本地化`\w`, `\W`, `\b`, `\B`, 大小写敏感性 (不推荐使用)
        - `re.M`, `re.MULTILINE` (`(?m)`): 多行匹配模式
            - `^`: 匹配行首位置而非字符串首位置
            - `$`: 匹配行尾位置而非字符串尾位置
        - `re.S`, `re.DOTALL` (`(?s)`): `.`匹配任意字符 (含换行符), 而非除换行符外的任意字符
        - `re.X`, `re.VERBOSE` (`(?x)`): 高可读性编写模式
            - 空白字符被忽略, 以允许换行, 缩进与空白分隔
                - 字符组中, 反斜杠后, RepetitionQualifier前, `(?:)`内, `(?P<Name>)`内的空白符不被忽略, 而作为空白字符参与匹配
            - `#`作为行注释符, 其后的内容作为注释
                - 字符组中, 反斜杠后的`#`不作为行注释符, 而作为`#`字符参与匹配

```python
pattern = re.compile(r"<Pattern>"[, flags = <Flag>[|<Flag>]...])
```

- `re.search(<Pattern>, <ContentStr>[, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`查找`<ContentStr>`的匹配子串, 并返回第一次匹配的匹配对象
    - 若无匹配, 则返回`None`. 注意: 这与匹配到空串不同

```python
match = re.search(r"<Pattern>", "<Content>"[, flags = <Flag>[|<Flag>]...])
```

- `re.match(<Pattern>, <ContentStr>[, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`查找`<ContentStr>`的从首位置开始的匹配子串, 并返回第一次匹配的匹配对象
    - 若无匹配, 则返回`None`. 注意: 这与匹配到空串不同
    - 注意: 即使是`MULTILINE`多行模式, `re.match()`也只查找从首位置开始的匹配子串, 而非从行首位置开始的匹配子串

```python
match = re.match(r"<Pattern>", "<Content>"[, flags = <Flag>[|<Flag>]...])
```

- `re.fullmatch(<Pattern>, <ContentStr>[, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`尝试匹配整个`<ContentStr>`, 并返回匹配对象
    - 若无匹配, 则返回`None`. 注意: 这与匹配到空串不同

```python
match = re.fullmatch(r"<Pattern>", "<Content>"[, flags = <Flag>[|<Flag>]...])
```

- `re.split(pattern = <Pattern>, string = <ContentStr>[, maxsplit = <MaxSplit>, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`搜索`<ContentStr>`, 并将其中每个匹配子串作为一个分界符, 将`<ContentStr>`由分界符分割为多个`str`并存储在列表中返回
        - 默认: 作为分界符的匹配子串不被存储到返回`str`列表中, 而被抛弃
        - 若`<Pattern>`中含有捕获匹配组, 则捕获内容也存储在返回`str`列表中
    - 若`maxsplit`非零, 则从左到右最多进行`maxsplit`次分割, 剩下的字符串作为返回`str`列表的最后一个元素
    - 注意
        - 当心字符串首尾分割出的空串
        - `<Pattern>`匹配到的空串也会分割字符串, 但**仅在匹配空串互不相邻的情况下生效**

```python
strList = re.split(pattern = r"<Pattern>", string = "<ContentStr>"[, maxsplit = <MaxSplit>, flags = <Flag>[|<Flag>]...]))
```

- `re.findall(pattern = <Pattern>, string = <ContentStr>[, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`从左到右搜索`<ContentStr>`, 并返回所有匹配子串 (或匹配`str`元组) 组成的`str`列表
    - 若`<Pattern>`包含捕获匹配组, 则返回匹配`str`元组而非匹配子串
    - 注意: 匹配空串也会包含在结果中

```python
strList = re.findall(pattern = r"<Pattern>", string = "<Content>"[, flags = <Flag>[|<Flag>]...])
```

- `re.finditer(pattern = <Pattern>, string = <ContentStr>[, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`从左到右搜索`<ContentStr>`, 并返回对应所有匹配子串 (或匹配`str`元组) 的迭代器`iterator`
    - 若`<Pattern>`包含捕获匹配组, 则迭代器对应匹配`str`元组而非匹配子串
    - 注意: 匹配空串也会包含在迭代器结果中

```python
iterator = re.finditer(pattern = r"<Pattern>", string = "<Content>"[, flags = <Flag>[|<Flag>]...])
```

- `re.sub(pattern = <Pattern>, repl = <Replacement>, string = <ContentStr>[, count = <Count>, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`从左到右搜索`<ContentStr>`, 并使用`<Replacement>`替换每次匹配, 返回替换后的字符串
    - `<Replacement>`
        - 字符串
            - 任何转义序列都会被处理
                - 非打印字符会被转义: `\n`
                - 数字转义会作为捕获组引用: `\<Number>`
                - 命名捕获组引用: `\g<<Name>>`
                - 捕获组引用: `\g<<Number>>`
                - 未知转义序列会被保持原样: `\&`
        - 函数
            - 接受匹配对象作为参数, 返回替换后的字符串
    - `count`: 替换的最大次数
    - 注意
        - 相邻的连续匹配空串作为单一匹配被替换

```python
rstStr = re.sub(pattern = r"<Pattern>", repl = r"<Replacement>", string = "<ContentStr>"[, count = <Count>, flags = <Flag>[|<Flag>]...])

def replFunc(match):
    ...
rstStr = re.sub(pattern = r"<Pattern>", repl = replFunc, string = "<ContentStr>"[, count = <Count>, flags = <Flag>[|<Flag>]...])
```

- `re.subn(pattern = <Pattern>, repl = <Replacement>, string = <Content>[, count = <Count>, flags = <Flag>[|<Flag>]...])`
    - 使用`<Pattern>`从左到右搜索`<ContentStr>`, 并使用`<Replacement>`替换每次匹配, 返回替换后的字符串与替换次数的元组
    - 注意
        - 相邻的连续匹配空串作为单一匹配被替换

```python
(rstStr, count) = re.subn(pattern = r"<Pattern>", repl = r"<Replacement>", string = "<Content>"[, count = <Count>, flags = <Flag>[|<Flag>]...])
```

- `re.escape(pattern = <Pattern>)`
    - 为`<Pattern>`中所有正则表达式元字符添加转义, 使其成为普通字符
    - 用于为 包含正则表达式元字符的字面 (Literal) 字符串 创建匹配模式
    - 不能用于创建`<Replacement>`: `<Replacement>`不是匹配模式

```python
patternStr = re.escape("python.exe") # patternStr = r"python\.exe"
match = re.match(patternStr, "python.exe") # Match a literal string
```

- `re.purge()`
    - 清除正则表达式缓存

### 3.2. Field
#### 3.2.1. Enum Const
- `<Flag>`
    - `re.A`, `re.ASCII` (`(?a)`): ASCII匹配 (而非Unicode)
    - `re.DEBUG`: 显示编译Debug信息
    - `re.I`, `re.IGNORECASE` (`(?i)`): 大小写不敏感匹配
    - `re.L`, `re.LOCALE` (`(?L)`): 本地化语言匹配, 本地化`\w`, `\W`, `\b`, `\B`, 大小写敏感性 (不推荐使用)
    - `re.M`, `re.MULTILINE` (`(?m)`): 多行匹配模式
        - `^`: 匹配行首位置而非字符串首位置
        - `$`: 匹配行尾位置而非字符串尾位置
    - `re.S`, `re.DOTALL` (`(?s)`): `.`匹配任意字符 (含换行符), 而非除换行符外的任意字符
    - `re.X`, `re.VERBOSE` (`(?x)`): 高可读性编写模式
        - 空白字符被忽略, 以允许换行, 缩进与空白分隔
            - 字符组中, 反斜杠后, RepetitionQualifier前, `(?:)`内, `(?P<Name>)`内的空白符不被忽略, 而作为空白字符参与匹配
        - `#`作为行注释符, 其后的内容作为注释
            - 字符组中, 反斜杠后的`#`不作为行注释符, 而作为`#`字符参与匹配

## 4. Class
### 4.1. Error
#### 4.1.1. Method
- `re.error(msg = <Message>[, pattern = <Pattern>, pos = <Position>])`

#### 4.1.2. Field
### 4.2. 正则表达式类
- 编译后的正则表达式

```python
pattern = re.compile(r"<Pattern>"[, flags = <Flag>[|<Flag>]...])
```

#### 4.2.1. Method

- `Pattern.search(string = <ContentStr>[, pos = <Position>[, endpos = <EndPosition>]])`
    - 从左到右搜索`<ContentStr>`的第一个匹配, 并返回相应的匹配对象
    - 若无匹配, 则返回`None`. 注意: 这和匹配空串不同
    - `pos`: `<ContentStr>`中开始搜索的索引
    - `endpos`: `<ContentStr>`中结束搜索的索引

```python
match = pattern.search("<ContentStr>"[, pos = <Position>[, endpos = <EndPosition>]])
```

- `Pattern.match(string = <ContentStr>[, pos = <Position>[, endpos = <EndPosition>]])`
    - 查找`<ContentStr>`的从首位置开始的匹配子串, 并返回第一次匹配的匹配对象
    - 若无匹配, 则返回`None`. 注意: 这和匹配空串不同

```python
match = pattern.match("<ContentStr>"[, pos = <Position>[, endpos = <EndPosition>]])
```

- `Pattern.fullmatch(string = <ContentStr>[, pos = <Position>[, endpos = <EndPosition>]])`
    - 尝试匹配整个`<ContentStr>`, 并返回匹配对象
    - 若无匹配, 则返回`None`. 注意: 这与匹配到空串不同

```python
match = pattern.fullmatch("<ContentStr>"[, pos = <Position>[, endpos = <EndPosition>]])
```

- `Pattern.split(string = <ContentStr[, maxsplit = <MaxSplit>])`
    - 搜索`<ContentStr>`, 并将其中每个匹配子串作为一个分界符, 将`<ContentStr>`由分界符分割为多个`str`并存储在列表中返回
        - 默认: 作为分界符的匹配子串不被存储到返回`str`列表中, 而被抛弃
        - 若`pattern`中含有捕获匹配组, 则捕获内容也存储在返回`str`列表中
    - 若`maxsplit`非零, 则从左到右最多进行`maxsplit`次分割, 剩下的字符串作为返回`str`列表的最后一个元素
    - 注意
        - 当心字符串首尾分割出的空串
        - `pattern`匹配到的空串也会分割字符串, 但仅在匹配空串互不相邻的情况下生效

```python
strList = pattern.split(string = "ContentStr"[, maxsplit = <MaxSplit>])
```

- `Pattern.findall(string = <ContentStr>[, pos = <Position>[, endpos = <EndPosition>]])`
    - 从左到右搜索`<ContentStr>`, 并返回所有匹配子串 (或匹配`str`元组) 组成的`str`列表
    - 若`pattern`包含捕获匹配组, 则返回匹配`str`元组而非匹配子串
    - 注意: 匹配空串也会包含在结果中

```python
strList = pattern.findall(string = "<Content>"[, pos = <Position>[, endpos = <EndPosition>]])
```

- `Pattern.finditer(string = <ContentStr>[, pos = <Position>[, endpos = <EndPosition>]])`
    - 从左到右搜索`<ContentStr>`, 并返回对应所有匹配子串 (或匹配`str`元组) 的迭代器`iterator`
    - 若`pattern`包含捕获匹配组, 则迭代器对应匹配`str`元组而非匹配子串
    - 注意: 匹配空串也会包含在迭代器结果中

```python
iterator = pattern.finditer(string = "<Content>"[, pos = <Position>[, endpos = <EndPosition>]])
```

- `Pattern.sub(repl = <Replacement>, string = <ContentStr>[, count = <Count>])`
    - 从左到右搜索`<ContentStr>`, 并使用`<Replacement>`替换每次匹配, 返回替换后的字符串
    - `<Replacement>`
        - 字符串
            - 任何转义序列都会被处理
                - 非打印字符会被转义: `\n`
                - 数字转义会作为捕获组引用: `\<Number>`
                - 命名捕获组引用: `\g<<Name>>`
                - 捕获组引用: `\g<<Number>>`
                - 未知转义序列会被保持原样: `\&`
        - 函数
            - 接受匹配对象作为参数, 返回替换后的字符串
    - `count`: 替换的最大次数
    - 注意
        - 相邻的连续匹配空串作为单一匹配被替换

```python
rstStr = pattern.sub(repl = r"<Replacement>", string = "<ContentStr>"[, count = <Count>])

def replFunc(match):
    ...
rstStr = pattern.sub(repl = replFunc, string = "<ContentStr>"[, count = <Count>])
```

- `Pattern.subn(repl = <Replacement>, string = <ContentStr>[, count = <Count>])`
    - 从左到右搜索`<ContentStr>`, 并使用`<Replacement>`替换每次匹配, 返回替换后的字符串与替换次数的元组
    - 注意
        - 相邻的连续匹配空串作为单一匹配被替换

```python
(rstStr, count) = pattern.subn(repl = r"<Replacement>", string = "<Content>"[, count = <Count>])
```

#### 4.2.2. Field

- `Pattern.flags`
    - 匹配标记
    - 包括`re.compile()`参数, `(?...)`内联标记与隐性标记
- `Pattern.groups`
    - 捕获组的数量
- `Pattern.groupindex`
    - 命名捕获组的名称到其捕获组编号的映射的字典
- `Pattern.pattern`
    - 正则表达式字符串

### 4.3. 匹配类
#### 4.3.1. Method

- `Match.expand(template = <Template>)`
    - 对`<Template>`进行捕获组引用替换并返回替换后的字符串
    - 捕获组引用: `\<Number>`, `\g<<Number>>`, `\g<<Name>>`
    - 未匹配的捕获组引用为空串

```python
rstStr = match.expand(template = r"<Template>")
```

- `Match.group([(<Number>|"<Name>"), ...])`
    - 返回捕获组内容
    - 若没有提供参数
        - 返回0号捕获组内容
    - 若提供一个参数
        - 返回对应捕获组内容
    - 若提供多个参数
        - 返回一个由对应捕获组内容组成的元组

```python
match = re.search(pattern, contentStr)
match.group([(<Number>|"<Name>"), ...])
```

- `Match.__getitem__(<Number>)`
    - 允许以列表的方式获取捕获组内容

```python
match = re.search(pattern, contentStr)
match[<Number>] # 引用捕获组内容
```

- `Match.groups([default = <Default>])`
    - 返回所有捕获组的内容组成的元组
    - `default`决定未匹配的捕获组的返回内容, 默认为`None`

```python
match = re.search(pattern, contentStr)
match.groups()
match.groups(<Default>)
```

- `Match.groupdict([default = <Default>])`
    - 返回命名捕获组与其内容组成的字典

```python
match.groupdict()
match.groupdict(<Default>)
```

- `Match.start([(<Number>|"<Name>")])`
    - 返回对应捕获组的匹配子串开始索引
    - 默认为0号捕获组
    - 若捕获组未匹配, 则返回-1

```python
match.start()
match.start((<Number>|"<Name>"))
```

- `Match.end([(<Number>|"<Name>")])`
    - 返回对应捕获组的匹配子串结束索引
    - 默认为0号捕获组
    - 若捕获组未匹配, 则返回-1

```python
match.end()
match.end((<Number>|"<Name>"))
```

- `Match.span([group = (<Number>|"<Name>")])`
    - 返回二元组`(match.start(group), match.end(group))`
    - `group`默认为0

#### 4.3.2. Field
- 匹配对象总对应一个布尔值`True`. 可以通过`if`语句判断是否匹配

```python
match = re.search(pattern, contentStr)
if match:
    # Do something
```

- `Match.pos`
    - `search()`等方法搜索的开始索引

- `Match.endpos`
    - `search()`等方法搜索的结束索引

- `Match.lastindex`
    - 最后一个匹配的捕获组的索引
    - 若没有捕获组匹配, 则为`None`

- `Match.lastgroup`
    - 最后一个匹配的捕获组的名称
    - 若最后一个匹配的捕获组没有名称, 或没有捕获组匹配, 则为`None`

- `Match.re`
    - 产生该匹配的正则表达式对象

- `Match.string`
    - 产生该匹配的`<ContentStr>`

## 5. 转义

- 使用正则表达式时涉及两层转义
    - 字符串转义
    - 正则表达式转义
    - E.g.:
        - `'"'`匹配`"`
            - 字符串转义: `"`->`"`
            - 正则表达式转义: `"`->`"`
        - `'\"'`匹配`"`
            - 字符串转义: `\"`->`"`
            - 正则表达式转义: `"`->`"`
        - `'\\"'`匹配`"`
            - 字符串转义: `\\"`->`\"`
            - 正则表达式转义: `\"`->`"`
        - `'\\\"'`匹配`"`
            - 字符串转义: `\\\"`->`\"`
            - 正则表达式转义: `\"`->`"`
        - `'\\\\"'`匹配`\"`
            - 字符串转义: `\\\\"`->`\\"`
            - 正则表达式转义: `\\"`->`\"`
- 通过原始字符串使用正则表达式时涉及两层转义
    - 原始字符串转义
    - 正则表达式转义
    - E.g.:
        - `r'"'`匹配`"`
            - 原始字符串转义: `"`->`"`
            - 正则表达式转义: `"`->`"`
        - `r'\"'`匹配`"`
            - 原始字符串转义: `\"`->`\"`
            - 正则表达式转义: `\"`->`"`
        - `r'\\"'`匹配`\"`
            - 原始字符串转义: `\\"`->`\\"`
            - 正则表达式转义: `\\"`->`\"`
