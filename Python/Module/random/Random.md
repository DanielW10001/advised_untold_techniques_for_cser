# Random

Pseudo-random number generator

- Sampling: 抽取
- Replacement: 放回
- Sampling with Replacement: 有放回抽取
- Sampling without Replacement: 无放回抽取

```python
import random

# Integer
# Uniform Selection from Range
random.randrange(stop)
random.randrange(start, stop[, step])
random.randint(sub, sup)

# Float
# Uniform Selection from [0.0, 1.0)
random_float = random.random()
# Selection from Range under given Distribution: See module doc

# Sequence
# Uniform Selection of Element
random.choice(sequence)
# Sample with Replacement
random.choices(collection, k=number)
# Sample without Replacement
random.sample(collection, number)

# List
# In-Place Shuffle
random.shuffle(list)

# String
# Generate random string
''.join(random.choices(string.ascii_letters + string.digits, k=LENGTH))
```

[TOC]

## 1. Function

- Share Module State
- Actually is bound Method of a Hidden Instance of `random.Random`

### 1.1. State Control

- `random.seed(a=None, version=2)`: Initialize Module Random Number Generator
    - `a`: Seed
        - If omitted or `None`, os randomness source or current system time is used
- `random.getstate()`: Get snapshot of current state as State Object
- `random.setstate(state)`: Set state with State Object
- `random.getrandbits(k)`: Get an integer with `k` random bits

### 1.2. Integer

- `random.randrange(stop)`
- `random.randrange(start, stop[, step])`
    - Randomly select an element from `range(start, stop[, step])`
- `random.randint(a, b)`: Get a random integer `N` such that `a <= N <=b`

### 1.3. Sequence( & Set)

- `random.choice(seq)`: Randomly select an element from `seq`
- `random.choices(population, weights=None, cum_weights=None, k=1)`
    - Get a `k` sized list of elements chosen from `population` with replacement
    - `weight`: A weight (number) sequece of each element of `population`
        - If given, selection are made according to it
    - `cum_weight`: A comulative weights sequence of each element of `population`
        - If given, selection are made according to it
    - If neither `weight` nor `cum_weight` are given, selections are made with equal probability
- `random.shuffle(x[, random])`: Suffle sequence `x` in place
    - `random`: 0-argument fuction returning a random float in [0.0, 1.0)
- `random.sample(population, k)`: Return a `k` length list of unique elements chosen from the `population` sequence or set
    - Random sampling without replacement
    - `random.sample(x, k=len(x))`: Shuffle immutable sequence and return a new shuffled list

### 1.4. Float

- `random.random()`: Next random floating number in [0.0, 1.0)

## 2. Class

### 2.1. `random.Random`

- Implements all module funtion as method

#### 2.1.1. Method

- `__init__([seed])`: Construct with `seed`

### 2.2. `random.SystemRandom`

- Use `os.urandom()` from randomness source provided by os
- Implements all module funtion as method

#### 2.2.1. Method

- `__init__([seed])`: Construct with `seed`
