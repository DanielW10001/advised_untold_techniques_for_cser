# sort: Sort Text File Lines

```man
sort [OPTION]... [FILE]...

File:
-  # Default
PATH

Option:
  Ordering:
    -b|--ignore-leading-blanks
    -d|--dictionary-order
    -f|--ignore-case  # Fold lower case to upper case characters
    -g|--general-numeric-sort
    -i|--ignore-noprinting
    -M|--month-sort  # 'JAN'<...<'DEC'
    -h|--human-numeric-sort  # K, M, G, T, P
    -n|--numeric-sort
    -R|--random-sort  # Shuffle
    --random-source=FILE
    -r|--reverse
    --sort=general-numeric|human-numeric|month|numeric|random|version
    -V|--version-sort
  Other:
    --batch-size=COUNT  # Merge most COUNT inputs at once; for more use temp files
    -c|--check[=diagnose-first]  # Check instead of Sort
    -C|--check=quiet|silent  # Check instead of Sort, but do not report first bad line
    --compress-program=CMD  # Compress temp file with CMD, Decompress with CMD -d
    --debug  # Annotate Sorting Part
    --files0-from=PATH_LIST_FILE  # PATH_LIST_FILE contains list of input files, NUL-Seperated
    -k|--key=START_WORD[.START_CHAR][bdfgiMhnRrV][,END_WORD[.END_CHAR][bdfgiMhnRrV]]  # Default to Whole Line
    -m|--merge  # Merge instead of Sort
    -o|--output=FILE  # Write to FILE instead of Default stdout
    -s|--stable  # Stabilize Sort
    -S|--buffer-size=NUM[%bKMGTPEZY]  # Main Memory Buffer Size
    -t|--field-separator=SEP  # Default to \s
    -T|--temporary-directory=DIR  # Default to $TMPDIR or /tmp
    --parallel=COUNT  # Sort Parallel Count
    -u|--unique  # Deduplicate
    -z|--zero-terminated  # NUL instead of \n as Line Delimiter
    --help
    --version
```
