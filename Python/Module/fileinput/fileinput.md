# fileinput

```python3
with fileinput.input([files][, inplace][, backup][, openhook=fileinput.hook_encoded('utf-8')]) as lines:
    for line in lines:
        fileinput.filename()
        fileinput.fileno()  # File descriptor
        fileinput.lineno()  # Cumulative line number
        fileinput.filelineno()  # File line number
        fileinput.isfirstline()  # Check if this is the first line of current file
        fileinput.isstdin()
        fileinput.nextfile()  # Next iteration will skip to next file
        fileinput.close()  # End iteration
        print(replacement)

# Workaround for inplace utf-8 editing
with open('swp', 'w', encoding='utf-8') as swpfile:
    with fileinput.input(file, openhook=fileinput.hook_encoded('utf-8')) as lines:
        for line in lines:
            swpfile.write(newline(line))
os.replace('swp', file)
```

- 模块fileinput用于迭代一系列文本文件中的所有行
- `fileinput.input([files[, inplace[, backup]]]) -> Iterable[str]`: 返回一个包含各文件所有行的可迭代对象
    - `files`: 迭代的目标文件 (序列)
        - 当不提供值时, 默认对标准输入或命令行参数所指定的各文件进行迭代
            - 标准输入: `cat file( file)*|py script.py`
            - 命令行参数: `py script.py file( file)*`
        - 可以提供单个文件的路径字符串, 此时, 将对所指定的文件进行迭代: `r'filedir'`
        - 可以提供包含多个文件路径字符串的可迭代对象, 此时, 将对所指定的各文件进行迭代: `[r'filedir'(, r'filedir')*]`
    - `inplace`: 就地修改, 默认为`False`
        - `True`: 在对各行迭代过程中, 将`sys.stdout`重定向到当前行所在文件的所在位置, 使用本轮迭代过程中向`sys.stdout`写入的内容替代原始行及其换行符. 若本轮迭代过程中未向`sys.stdout`写入任何内容, 使用空串`""`替代原始行及其换行符
        - `False`: 不对`sys.stdout`进行重定向, 迭代过程中对`sys.stdout`的写入都将写入到原始`sys.stdout`中
        - 注意: 就地修改很容易破坏文件, 应在非就地修改的情况下仔细测试程序, 确保程序能够正确运行后再让程序就地修改文件
    - `backup`: 当启用就地修改时的备份文件的附加扩展名字符串. 若不指定值或未启用就地修改, 则不进行备份
    - 注意: 与内置函数`input`不同, `fileinput.input`所返回的可迭代对象中的文件行字符串包含文件中的行尾换行符
- `fileinput.filename()`: 返回当前行所在文件的文件名
- `fileinput.lineno()`: 返回当前行的累计行号
- `fileinput.filelineno()`: 返回当前行在当前文件中的行号
- `fileinput.isfirstline()`: 检查当前行是否是当前文件的第一行
- `fileinput.isstdin()`: 检查当前行的所在文件是否是`sys.stdin`
- `fileinput.nextfile()`: 关闭当前文件并让下一轮迭代跳到下一个文件的第一行
    - 累计行号忽略所跳过的行
    - 用于在无需继续处理当前文件时跳过当前文件
- `fileinput.close()`: 关闭整个文件序列并在本轮迭代结束后结束迭代
