# cp

`cp [OPTION]... SOURCE... DEST`

- Overwrite `DEST` by default
- `OPTION`
    - `-b`: Backup with `~` suffix
    - `--preserve=ATTR(,ATTR)*`: `ATTR`: `mode`, `ownership`, `timestamps`, `context`, `links`, `xattr`, `all`: Preserve Attributes
    - `-p`: `--preserve`: `--preserve=mode,ownership,timestamps`
    - `-R`, `-r`, `--recursive`
    - `-u`, `--update`: Override only newer
