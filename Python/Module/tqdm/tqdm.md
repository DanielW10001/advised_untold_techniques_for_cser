# tqdm

Progress Bar for CLI

[TOC]

## 1. Install

```powershell
pip[env] install tqdm
```

## 2. Usage

```python
from tqdm import tqdm

# Without Progress Bar
for element in iterable:
    # ...

# With Proress Bar
for element in tqdm(iterable[, unit: str="it"]):
    # ...
```

- `tqdm()`: Create a new tqdm instance
    - `unit`: Unit description str of each iteration
- `trange()` == `tqdm(range())`
