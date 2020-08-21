# File

## 1. Open

- Open file
- Mode
    - `r`: Read (default)
    - `w`: Write
    - `x`: Exclusive write, failing if file already exists
    - `a`: Appending: `file.write()` to append
    - `b`: Binary mode
    - `t`: Text mode (default)
    - `+`: Read & Write
- `seek(OFFSET, whence=(SEEK_CUR|SEEK_END))` to move cursor
- Raise exceptioin if parent dir not exist
- Line end auto convertion will be activited in text mode

```python
with open(r'<FileDir>', r'<Mode>') as <FileObjectName>:
    # ...
```

## 2. Codecs.open

- `codecs.open(filename=r'<FileDir>', mode='<Mode>', encoding='<Encoding>', errors='<ErrorHandler>', buffering=<Buffering>)`
- Open file with givin coding
- Always auto add binary mode 'b'
    - Line end auto convertion not applied

## 3. Redirect STD

```python
import sys
sys.stdin = codecs.open(filename=r'FileDir', mode='r', encoding='Encoding')
sys.stdout = codecs.open(filename=r'FileDir', mode='w', encoding='Encoding')
sys.stderr = codecs.open(filename=r'FileDir', mode='w', encoding='Encoding')
```

## 4. Method

### 4.1. Readlines

- `readlines([sizehint])`
- Read all lines on the input stream
    - Including line ends
- `sizehint`: Max bytes read

## 5. `write()`

- `write(str)`: Write `str`
- 注意: 与`print()`不同, `write()`不会在末尾添加`end` (print默认为`\n`)

## 6. Usage

- Can be used as list of lines: `for line in file:`