# tempfile

```python3
with tempfile.(Named|Spooled)?TemporaryFile([max_size][, mode='w+t'], encoding='utf-8'[, prefix][, dir]) as tempfile:
    named_tempfile.name  # Full name, aka path
    spooled_tempfile.rollover()  # Write to disk

with tempfile.TemporaryDirectory([prefix][, dir]) as tempdirname:
    tempdirname  # Full name, aka path

tempfile.gettempdir()  # Get default temp dir
tempfile.gettempprefix()  # Get default temp prefix
```
