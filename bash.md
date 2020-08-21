# bash

- `set -o vi`: Vi Mode
- `alias ALIAS="CONTENT"`: Set alias
- `<C-p>`: Last command
- `<C-n>`: Next command
- `<C-f>`: Move cursor right
- `<C-b>`: Move cursor left
- `COMMAND; COMMAND`: Concatenate command
- Short circuit execution
    - `COMMAND || COMMAND`: If first one is true then second one is not executed
    - `COMMAND && COMMAND`: If first one is false then second one is not executed
- Execute Script: `[bash] PATH`
- `<`: Read stdin from file
- `|`: Pipe stdout to stdin
- `>`: Write stdout to file
- `>>`: Append stdout to file

## 1. Escaping

```bash
\n  # Python: r'\n'
'\n'  # Python: '\n'
```
