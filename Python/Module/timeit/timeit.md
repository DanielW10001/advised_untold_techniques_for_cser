# timeit

- timeit: Measure execution time of small code snippets

## 1. Python Interface

```python3
import timeit
timeit.timeit(stmt=('statement'|callable), setup='setup', number=execute_number, globals=global_scope)  # Return execution time in seconds
timeit.repeat(stmt=('statement'|callable), setup='setup', repeat=repeat, number=execute_number, globals=global_scope)  # Return list of execution time in seconds
```

## 2. Command Line Interface

```bash
python -m timeit [OPTIONS...] [statement...]
```

- Option
    - `-n N`, `--number=N`: Execute `statement` for `N` times
    - `-r N`, `--repeat=N`: Repeat timer for `N` (default 5) times
    - `-s S`, `--setup=S`: Execute `S` (default `pass`) once initially
    - `-p`, `--process`: Measure process time rather than default wallclock time
    - `-u U`, `--unit=U`: Specify `U` as time unit. `U` can be `nsec`, `usec`, `msec`, `sec`
    - `-v`, `--verbose`: Print raw timing results. Repeat this option for more digits precision.
    - `-h`, `--help`: Print usage message
