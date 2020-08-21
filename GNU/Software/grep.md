# grep

```man
grep [OPTION]... "PATTERN"(\n"PATTERN")* [PATH|-]...

Path:  # Glob Supported: *, **, ?, [], [^]
Recursive Search Default to .
Non-Recursive Search Default to -

Option:
  Pattern:
  -G|--basic-regexp  # BRE; Default
  -E|--extended-regexp  # ERE
  -P|--perl-regexp  # PCRE
  -F|--fixed-strings  # Literal Substring

  Matching Control:
  {-e|--regexp "PATTERN"(\n"PATTERN")*}  # Multiple -e are OR'ed; Used to Protect Pattern Beginning with -
  {-f|--file PATH}  # Pattern from File, one per line; Multiple -f are OR'ed
  -i|--ignore-case
  --no-ignore-case  # Default
  -v|--invert-match  # Filter instead of Match
  -w|--word-regexp  # Whole Word Match
  -x|--line-regexp  # Whole Line Match

  Output Control:
  -c|--count  # Count Instead of Print
  --colo[u]r[=never|always|auto]
  -L|--files-without-match  # Print Not Matched File's Name
  -l|--files-with-matches  # Print Matched File's Name
  -m|--max-count COUNT  # Skip a File after COUNT Matches
  -o|--only-matching  # Matched Part Only
  -q|--quiet|--silent  # No Output
  -s|--no-messages  # No Error Messages
    Line Format:
    -b|--byte-offset
    -H|--with-filename  # Default for Multiple Files Search
    -h|--no-filename  # Default for Single File Search
    --label=LABEL  # Label stdin
    -n|--line-number
    -T|--initial-tab  # Aligned Line Column Start
    -u|--unix-byte-offsets  # Ignore \r
    -Z|--null  # Null Char Delimeter

    Context:
    -A|--after-context COUNT  # Print COUNT Lines After Matching Lines; -- Seperated
    -B|--before-context COUNT  # Print COUNT Lines Before Matching Lines; -- Seperated
    ((-C|--context) |-)COUNT  # Print COUNT Context Lines

  File Filter:
  --exclude GLOB  # Exclude Files with Base Name Matches GLOB; Glob: *, ?, []
  --exclude-from GLOB_FILE  # Exclude Files with Base Name Matches Glob in GLOB_FILE
  --exclude-dir GLOB  # Exclude Directories with Base Name Matches GLOB; Glob: *, ?, []
  --include GLOB  # Only Include Files with Base Name Matches GLOB
  -r|--recursive
  -R|--dereference-recursive  # Recursive While Following Symbolic Links
    Special File:
    -a|--text  # Binary File as Text File
    --binary-files=binary|without-match|text  # Binary File as TYPE File
    -D|--devices read|skip  # Device/FIFO/Socket Action
    -d|--directories recurse|read|skip  # Directory Action
    -I  # Ignore Binary File

  Miscellaneous:
  --line-buffered
  -U|--binary  # Text File as Binary File, with \r\n untouched
  -z|--null-data  # Null Char Delimeter I/O

  Info:
  --help
  -V|--version

Exit:
0  # Line Matched
1  # No Line Matched
2  # Error Occurred
```

```bash
grep -rP PCRE .
CMD | grep -P PCRE
```

`grep [OPTION]... PATTERN [FILE]...`

## 1. Option

- Pattern Selection and interpretation
    - `-P`, `-perl-regexp`
        - PATTERN is PCRE
        - `-`in PATTERN
            - `-`: Arg
            - `\-`: Literal
- Output control
    - `-r`, `--recursive`

## 2. File

- Default: `.`
- `-`: stdin
