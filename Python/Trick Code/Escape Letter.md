# Escape Letter

```python
# 'a' -> '\a', 'b' -> '\b'
letter_str  # 'a', 'b', ...
esc_str = eval('\'\\' + letter_str + '\'')
esc_str = {'a': '\a', ..., 'z': '\z'}[letter_str]
```
