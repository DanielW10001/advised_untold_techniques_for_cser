# String

## 1. `str`

- `str`
- String
- Construct str from value: `str(value)`

### 1.1. Format

- `{<Expression>}`: Replace with Expression Value
- `{{`: Replace with `{`
- `}}`: Replace with `}`

### 1.2. Literial

- Machine Oriented: `'<Str>'`
- Human Oriented: `"<Str>"`
- Formatted: `(f|F)"...{Variable}..."`

### 1.3. Method

#### 1.3.1. `format()`

- Replace `{}` with value of var at corespondent position: `"<String>".format(<Var>(, <Var>)*)`

#### 1.3.2. `capitalize()`

- Return a copy of the string with its first character capitalized and the rest lowercased.

#### 1.3.3. `casefold()`

- Return a casefolded copy of the string. Casefolded strings may be used for caseless matching.
- Casefolding is similar to lowercasing but more aggressive because it is intended to remove all case distinctions in a string.

## 2. `repr`

- A code string that will construct this value
- Construct repr from value: `repr(value)`

## 3. Unicode code point & str

- Unicode code point -> str: `char_str = chr(char_unicode_code_point)`
- str -> Unicode code point: `char_unicode_code_point = ord(char_str)`
    - ATTENTION: `char` means character rather than `Char` Class or `ctypes.c_char`
