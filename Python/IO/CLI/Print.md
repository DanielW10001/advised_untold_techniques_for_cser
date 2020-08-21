# Print

- Print string
    - `sep`: Seperator
    - `end`: Appended at end
        - Default: `"\n"`: 换行, 光标到达下一行
        - `"\r"`: 回车, 光标返回行首
            - 可用于刷新本行文字
        - `''`: 空
            - 可用于连续打印
    - `file`: Print Output File
    - `flush`: Flush after print

```python
print(print_content(, print_content)*[, sep='', end='\n', file=sys.stdout, flush=False])
```
