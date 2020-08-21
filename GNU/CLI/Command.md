# Command

```bash
<Name>[ <Option>]...[ <Argument>]
```

- Option
    - Start and Concatenate with dash `-`
    - Delimit with space
    - May require additional value `<Argument>` next to it
    - Require a space or equal sign between option and its argument: `-<Name> <Argument>`, `--<Name>( |=)<Argument>`
    - Boolean option
        - True: `-<Name>`, `--<Name>`
        - False: `--no-<Name>`
    - Short Option: `-<Name>[ <Argument>]`
        - Start with single dash
        - Short option that do not need argument can be used immediately next to each other: `-O -L -v` <-> `-OLv`
    - Long Option: `--<Name>[( |=)<Argument>]`
        - Start with two dash
    - `-h`, `--help`, `-?` for help
- Argument
    - A value used by command or option
- Meta Char
    - `!`: `\!` for literal `!`
