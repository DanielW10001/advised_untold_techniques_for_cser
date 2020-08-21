# pathlib

```python3
# Construct
path = pathlib.Path(path(, path)*)
path = pathlib.Path()  # Current working directory
path = pathlib.Path.cwd()  # Current working directory
path = pathlib.Path.home()  # Home directory

# Property
{path}  # Immutable and hashable
path ==|!=|<|<=|>=|> path  # Comparable and orderable
os.fspath(path)  # Implemented os.PathLike protocol

# Operator
path = (path|'dir') / (path|'dir')  # Navigate
str(path)  # Path string
bytes(path)  # Path bytes

# Feature
path.parts  # Tuple of components

path.drive  # Drive letter
path.root  # Dir root
path.anchor  # Drive and root

path.parents  # Tuple of ancestors
path.parent  # Direct parent

path.name  # Final component
path.suffix  # Extension name
path.suffixes  # List of Extension names
path.stem  # Main name

path.as_posix()  # Forward slashes string
path.as_uri()  # URI string

path.is_absolute()
path.is_reserved()
path.match('glob_pattern')  # Check if match; glob_pattern: ?, *, **, []
path.exists()  # Check if path exists
path.is_dir()  # Check if path is a dir
path.is_file()  # Check if path is a file
path.is_mount()  # Check if path is a mount point
path.is_symlink()  # Check if path is a symbolic link
path.is_socket()  # Check if path is a socket
path.is_fifo()  # Check if path is a fifo
path.is_block_device()  # Check if path is a block device
path.is_char_device()  # Check if path is a char device
path.samefile(otherpath)  # Check if `path` and `otherpath` is the same file

path.group()  # Owner group name
path.owner()  # Owner name
stat = path.stat()  # Get status
stat = path.lstat()  # Get status of symbolic link
stat.st_mode  # File mode
stat.st_dev  # Device
stat.st_nlink  # Hard link number
stat.st_uit  # Owner uid
stat.st_gid  # Owner gid
stat.st_size  # Size in bytes
stat.st_atime  # Time of most recent access in seconds
stat.st_mtime  # Time of most recent content modification in seconds
stat.st_ctime  # Time of most recent metadata change or creation in seconds
stat.st_flags  # POSIX User defined flags
stat.st_file_attributes  # Windows file attributes

path = path.joinpath(otherpath(, otherpath)*)
path = path.relative_to(path)  # Compute relative path
path = path.with_name(name)  # New path with final component changed to `name`
path = path.with_suffix(suffix)  # New path with extension name changed to `suffix`
path = path.expanduser()
path = path.resolve()  # Resolve path

path.iterdir()  # Iterable over direct subpaths
path.glob(r'glob_pattern')  # Iterable over matched subpaths; glob_pattern: ?, *: any filename, **: any subdir, []; `'**/*'` for any file
path.rglob(r'glob_pattern')  # Iterable over recursively matched subpaths; glob_pattern: ?, *: any filename, **: any subdir, []; `'*'` for any file

path.touch([mode][, exist_ok=True])  # Create file `path`
path.mkdir([mode][, parents=True][, exist_ok=True])  # Make dir `path`
target_path = path.rename(target_path)  # Rename `path` to `target_path` and return it
target_path = path.replace(target_path)  # Rename `path` to and repalce existing `target_path` and return it
path.rmdir()  # Remove empty dir
path.unlink([missing_ok=True])  # Remove file or symlink
path.symlink_to(target)  # Make `path` a symbolic link to `target`
path.link_to(target)  # Make `path` a hard link to `target`
path.chmod(mode)  # Change mode and permissions
path.lchmod(mode)  # Change mode and permissions of symbolic link

with path.open([mode][, encoding='utf-8']) as file:  # Open file
    # ...
path.write_text(text[, encoding='utf-8'])  # Write and overwritten
text = path.read_text([encoding='utf-8'])  # Get content as str
path.write_bytes(bytes)  # Write and overwritten
bytes = path.read_bytes()  # Get content as bytes
```
