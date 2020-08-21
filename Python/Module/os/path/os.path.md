# os.path

```python3
os.path.abspath(path)  # Return normalized `path`
os.path.basename(path)  # Return basename of `path`
os.path.commonpath(paths)  # Return longest common sub-path of `paths`
os.path.commonprefix(paths)  # Return longest common prefix of `paths`
os.path.dirname(path)  # Return the directory name of `path`
os.path.exists(path)  # Check if `path` exists
os.path.expanduser(path)  # Expand '~' in `path`
os.path.expandvars(path)  # Expand `$VAR`, `${VAR}` and `%VAR%` in `path`
os.path.getatime(path)  # Get access time of `path`
os.path.getmtime(path)  # Get modification time of `path`
os.path.getctime(path)  # Get creation or metadata change time of `path`
os.path.getsize(path)  # Get size in bytes of `path`
os.path.isabs(path)  # Check if `path` is an absolute path
os.path.isfile(path)  # Check if `path` is a file
os.path.isdir(path)  # Check if `path` is a dir
os.path.islink(path)  # Check if `path` is a symbolic link
os.path.ismount(path)  # Check if `path` is a mount point
os.path.join(path(, path)*)  # Join paths
os.path.normcase(path)  # Normalize case of `path`
os.path.normpath(path)  # Normalize `path`
os.path.realpath(path)  # Return the canonical path of `path`
os.path.relpath(path, start=os.curdir)  # Return a relative path to `path` from `start`
os.path.samefile(path1, path2)  # Check if `path1` and `path2` are the same file
os.path.sameopenfile(fd1, fd2)  # Check if `fd1` and `fd2` are the same file
os.path.samestat(stat1, stat2)  # Check if `stat1` and `stat2` refer to the same file
directory, file = os.path.split(path)
drive, dir_in_drive = os.path.splitdrive(path)
root, extension_name = os.path.splitext(path)
os.path.supports_unicode_filenames  # If OS supports unicode filenames
```

- `os.path`: Handle pathnames
- Path parameters can be passed as either `str` or `bytes`, and returned path will be the same type as the parameter.
- Path will not be automaticlly expanded. Invoke `os.path.expanduser()` and `os.path.expandvars()` to expand path.
- `os.path` is always suitable for the host os. Use `posixpath` and `ntpath` for POSIX and Windows OS.
- `os.path.abspath(path)`: Return normalized absolutized `path`
- `os.path.basename(path)`: Return the file name of `path`
    - If `path` ends with slash, returned file name is empty string.
    - If `path` indicates a directory but dose not end with slash, returned file name is the directory name, in which case indecated directory is seen as a file.
- `os.path.commonpath(paths)`: Return the longest common sub-path of each path in the sequence `paths`.
    - Unlike `os.path.commonprefix()`, this returns a valid path
- `os.path.commonprefix(paths)`: Return the longest common path prefix (taken character-by-character) of each path in the sequence `paths`
    - Unlike `os.path.commonpath()`, this may return an invalid path
- `os.path.dirname(path)`: Return the directory name of `path`
- `os.path.exists(path)`: Return `True` if `path` exists. Return `False` for broken symbolic links.
- `os.path.lexits(path)`: Return `True` if `path` exists. Return `True` for broken symbolic links.
- `os.path.expanduser(path)`: Return `path` with `~` and `~USER` expanded to user's home directory
- `os.path.expandvars(path)`: Return `path` with `$VAR`, `${VAR}`, `%VAR%` expanded to the value of variable
- `os.path.getatime(path)`: Return the time of last access of `path`
- `os.path.getmtime(path)`: Return the time of last modification of `path`
- `os.path.getctime(path)`: Return the time of last metadata change or creation of `path`
- `os.path.getsize(path)`: Return the size in bytes of `path`
- `os.path.isabs(path)`: Return `True` if `path` is an absolute path
- `os.path.isfile(path)`: Return `True` if `path` is an existing regular file. Return `False` if `path` is an existing directory.
- `os.path.isdir(path)`: Return `True` if `path` is an existing directory.
- `os.path.islink(path)`: Return `True` if `path` is an existing symbolic link directory.
- `os.path.ismount(path)`: Return `True` is `path` is a mount point.
- `os.path.join(path(, path)*)`: Return concatenation of `path`s.
    - If a `path` is an absolute path, all previous `path` are ignored and joining continues from the absolute path `path`.
- `os.path.normcase(path)`: Return normalized `path`. On Windows, convert all characters to lowercase and forward slashes  backward slashes.
- `os.path.normpath(path)`: Normalize `path` by collapsing redundant separators, up-level references `..` and same-level references `.`. On Windows, converts forward slashes to backward slashes.
- `os.path.realpath(path)`: Return the canonical path of `path`, eliminating any symbolic links.
- `os.path.relpath(path, start=os.curdir)`: Return relative path to `path` from `start`.
- `os.path.samefile(path1, path2)`: Return `True` is both path refer to the same file or directory.
- `os.path.sameopenfile(filepath1, filepath2)`: Return `True` is bothpath refer to the same file.
- `os.path.samestat(stat1, stat2)`: Return `True` if both stat tuples refer to the same file.
- `os.path.split(path)`: Split `path` into tuple `(directory, file)`.
- `os.path.splitdrive(path)`: Split `path` into tuple `(drive, tail)` where `drive` is a mount point.
- `os.path.splittext(path)`: Split `path` into tuple `(root, extendfilename)`. Leading periods on the filename are ignored.
- `os.path.supports_unicode_filenames`: `True` if os supports unicode filenames.
