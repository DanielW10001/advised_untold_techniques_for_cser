# chmod: Change File Mode

`chmod [OPTION]... MODE[,MODE]... FILE...`
`chmod [OPTION]... OCTAL FILE...`

- `MODE`: `[ugoa]*(MSET+|USET+|NSET)`
    - `MSET`: Mode Setting: `[+-=][rwxXst]*`
    - `USET`: User Setting: `[+-=][ugo]`
    - `NSET`: Number Setting: `[+-=][0-7]+`
- `OPTION`
    - `-R`, `--recursive`
