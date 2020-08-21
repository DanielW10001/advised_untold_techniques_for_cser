# OS

```python3
# OS Info
os.uname()  # OS info
os.name  # OS name
os.confstr(name)  # Get config `name`
os.confstr_names  # Config name dictionary
os.sysconf(name)  # Get sys config `name`
os.sysconf_names  # Sys config name dictionary
os.cpu_count()

# Env Var
os.environ  # Environment Variable Dictionary
os.get_exec_path()  # Get PATH list
os.umask(mask)  # Set process umask

# Process Info
os.getlogin()  # Get login user name
os.getpid()  # Get current process id
columns, lines = os.get_terminal_size()

# File Descriptor
os.fdopen(fd)  # Get file object from file descriptor
os.isatty(fd)  # Check if `fd` is a tty
master_fd, slave_fd = os.openpty()  # Open terminal pair
read_fd, write_fd = os.pipe()  # Create pipe

# Module Function
function in os.supports_dir_fd  # Check if function supports dir_fd
function in os.supports_effective_ids  # Check if function supports effective_ids
function in os.supports_fd  # Check if function supports fd
function in os.supports_follow_symlinks  # Check if function supports follow_symlinks

# File System
os.PathLike  # Abstract path class
os.chdir(path)  # cd
os.getcwd()  # pwd
os.listdir(path='.')  # ls path
os.chmod(path, mode)  # chmod
os.mkdir(path, mode=0o777)  # mkdir path
os.makedirs(path, mode=0o777, exist_ok=False)  # mkdir -p path
os.remove(file)  # rm file
os.unlink(file)  # rm file
os.rmdir(dir)  # rm dir; dir is empty
shutil.rmtree(dir)  # rm -r dir
os.removedirs(path)  # Remove path and its all empty parent dirs
os.rename(src, dst)  # mv src dst; existing dst will not be overwriten
os.replace(src, dst)  # mv src dst; existing dst will be overwriten
os.renames(src, dst)  # mv src dst; creating all parent dir of dst; removedirs(src)

for root, subdirs, files in os.walk(path[, topdown=False]):  # Iterate over all subentries of path
    for subdir in subdirs:  # subdir: str; subdirs: List[str]
        # Modify subdirs to control iteration
        subdirs.append(subdir)
        subdirs.remove(subdir)  # Remove subdir from iteration
        # ...
    for file in files:  # file: str
        # ...

with os.scandir(path) as entries:  # Iterate over dir entries
    for entry in entries:
        entry  # File path
        entry.path  # File path
        entry.name  # File name
        entry.is_dir()
        entry.is_file()
        entry.is_symlink()
        stat_result = entry.stat()  # Get status
stat_result = os.stat(path)  # Get status
stat_result.st_mode  # File mode
stat_result.st_dev  # Device
stat_result.st_nlink  # Hard link number
stat_result.st_uit  # Owner uid
stat_result.st_gid  # Owner gid
stat_result.st_size  # Size in bytes
stat_result.st_atime  # Time of most recent access in seconds
stat_result.st_mtime  # Time of most recent content modification in seconds
stat_result.st_ctime  # Time of most recent metadata change or creation in seconds
stat_result.st_flags  # POSIX User defined flags
stat_result.st_file_attributes  # Windows file attributes
stat_result.st_file_attributes & stat.FILE_ATTRIBUTE_*  # Windows file attribute (0 or 1)

os.chroot(path)  # chroot
os.chflags(path, flags)  # chflags
os.link(src, linkpath)  # ln src linkpath
os.symlink(src, linkpath)  # ln -s src linkpath
os.readlink(path)  # Get link target
os.mkfifo(path, mode=0o666)  # mkfifo path
os.pathconf(path, name)  # Get path config
os.pathconf_names  # Path config var names
os.sync()  # Force write of everything to disk
os.truncate(path, length)  # Truncate `path` to `length` bytes
os.utime(path, (atime, mtime))  # Set access and modified time

# Process Management
os.execvp(file, (arg(, arg)*))  # Replace current process with `file`
os._exit(exit_code)  # Exit after fork
if os.fork():  # Fork
    # Parent
else:
    # Child
pid, master_fd = forkpty()  # Fork with pty
if pid:
    # Parent
else:
    # Child
os.kill(pid, sig)  # Send `pid` `sig`
os.killpg(pgid, sig)  # Send `pgid` `sig`
os.startfile(path[, operation='open'])  # Start file `path`
child_pid, exit_code = os.wait()  # Wait for a child process to exit
os.waitid(idtype, id, options)  # Wait for a process event
os.waitpid(pid, options)  # Wait for exit of `pid`

# Random
os.getrandom(size)  # Get up to `size` random bytes
os.urandom(size)  # Return a string of `size` random bytes
```

- Path parameters can be passed as either `str` or `bytes`, and returned path will be the same type as the parameter.
- All functions raise `OSError` when arguments are not accepted by the os.
- File names, commmand line arguments and environment variables are represented as `str` with file system encoding `sys.getfilesystemencoding()`
- `os.error`: Alias for `OSError`
- `os.name`: `'posix'`, `'nt'` or `'java'`
- `listdir(r'<Dir>') -> list[str]`: List all file names in `<Dir>`
- `getcwd() -> str`: Get current working dir

## 1. Process Handling

- `os.ctermid()`: Return the filename of controling terminal of the process
- `os.environ`: A mapping object representing environment variables. This mapping may be used to modify and delete the environment variable in addition to query them.
    - Calling `os.putenv()` directly does not change `os.environ`, so it is better to modify `os.environ` instead
- `os.environb`: Bytes version of `os.environ`. Available when `os.supports_bytes_environ` is `True`.
- `os.fsencode(filename)`: Encode `filename` to file system encoding.
- `os.fsdecode(filename)`: Decode `filename` from file system encoding.
- `os.fspath(path)`: Return `path` as file system representation: `str` or `bytes`
- `os.PathLike`: An abstract base class for object representing path
    - `__fspath__()`: Abstract method. Return the file system path representation, `str` or `bytes`, of this object.
- `os.getenv(key, default=None)`: Return the value of the environment variable `key` if it exists, or `default` if it does not. Return type is `str`.
- `os.getenvb(key, default=None)`: Return the value of the environment variable `key` if it exists, or `default` if it does not. Return type is `bytes`. Available when `os.supports_bytes_environ` is `True`.
- `os.get_exec_path(env=None)`: Return the list of directories that will be searched for a named executable when launching a process.
    - `env`: Environment variable dictionary to lookup `PATH` in. By default, when `env` is `None`, `os.environ` is used.
- `os.getegid()`: Return the effective group id of current process
- `os.geteuid()`: Return the effective user id of current process
- `os.getgid()`: Return the real group id of current process
- `os.getgrouplist(user, group)`: Return list of group ids that `user` belongs to. If `group` is not in the list, it is included.
- `os.getgroups()`: Return list of supplemental group ids associated with current process
- `os.getlogin()`: Return the name of user logged in on the controlling terminal of current process. `getpass.getuser()` is recommended over this.
- `os.getpgid(pid)`: Return the process group id of `pid` process. If `pid` is 0, return the process group id of current process.
- `os.getpgrp()`: Return current process group id
- `os.getpid()`: Return current process id
- `os.getpriority(which, who)`: Get program scheduling priority
    - `which`: `os.PRIO_PROCESS`, `os.PRIO_PGRP` or `os.PRIO_USER`
    - `who`: Process identifier for `os.PRIO_PROCESS`, process group identifier for `os.PRIO_PGRP` or user ID for `PRIO_USER`. Zero value denotes calling process, process group of calling process or real user ID of calling process respectively.
- `os.getresuid()`: Return tuple `(ruid, euid, suid)` denoting the current process's real, effective and saved user ids.
- `os.getresgid()`: Return tuple `(rgid, egid, sgid)` denoting the current process's real, effective and saved group ids.
- `os.getuid()`: Return the current process's real user id.
- `os.initgroups(username, gid)`: Call the system `initgroups()` to initialize the group access list with groups that `username` belonges to and `gid` group.
- `os.putenv(key, value)`: Set environment variable `key` to `value`
    - Such changes to the environment affect subprocesses.
    - Recommend to assign to items of `os.environ` instead: Assignment to items of `os.environ` are automatically translated into calls to `putenv()`, but calls to `putenv()` do not update `os.environ`.
- `os.setegid(egid)`: Set the current process's effective group id to `egid`
- `os.seteuid(euid)`: Set the current process's effective user id to `euid`
- `os.setgid(gid)`: Set the current process's group id to `gid`
- `os.setgroups(groups)`: Set the list of supplemental group ids associated with the current process to `groups`
    - `groups`: Sequence of group id integer
    - Typically available to superuser
- `os.setpgrp()`: Call system `setpgrp()`
- `os.setpgid(pid, pgrp)`: Call the system `setpgid()` to set the process group id of `pid` process to `pgrp`
- `os.setpriority(which, who, priority)`
    - `which`: `os.PRIO_PROCESS`, `os.PRIO_PGRP` or `os.PRIO_USER`
    - `who`: Process identifier for `os.PRIO_PROCESS`, process group identifier for `os.PRIO_PGRP` or user ID for `PRIO_USER`. Zero value denotes calling process, process group of calling process or real user ID of calling process respectively.
    - `priority`: Integer in [-20, 19], default to 0. Lower priorities cause more favorable scheduling.
- `os.setregid(rgid, egid)`: Set current process's real and effective group ids.
- `os.setresgic(rgid, egid, sgid)`: Set current process's real, effective and saved group ids.
- `os.setresuid(ruid, euid, suid)`: Set current process's real, effective and saved user ids.
- `os.setreuid(ruid, euid)`: Set the current process's real and effective user ids.
- `os.getsid(pid)`: Call the system call `getsid()`
- `os.setsid()`: Call the system call `setsid()`
- `os.setuid(uid)`: Set the current process's user id to `uid`
- `os.strerror(code)`: Return the error message corresponding to the error code in `code`
- `os.supports_bytes_environ`: `True` if the native OS type of the environment is bytes
- `os.umask(mask)`: Set the current numeric umask and return the previous umask
- `os.uname()`: Returns information identifying the current operating system
    - Returned object has five attributes:
        - `sysname`: Operating system name
        - `nodename`: Name of machine on network
            - Better way: `socket.gethostname()`
        - `release`: Operating system release
        - `version`: Operating system version
        - `machine`: Hardware identifier
- `os.unsetenv(key)`: Unset (delete) the environment variable named `key`
    - Such changes to the environment affect subprocesses.
    - Recommend to delete items of `os.environ` instead: Deletion to items of `os.environ` are automatically translated into calls to `unsetenv()`, but calls to `unsetenv()` do not update `os.environ`.

## 2. File Object Creation

- These fuctions create new file objects
- Use `os.open()` to open file descriptors
- `os.fdopen(fd, *args, **kwargs)`: Return an open file object connected to the file descriptor `fd`
    - Used to wrap file descriptor into file object with `write()` and `read()` methods
    - This is the same as the `open()` built-in function and accepts the same arguments, except the first argument of `fdopen()` is a file descriptor

## 3. File Descriptor Operations

- These functions operate on I/O streams referenced using file descriptors.
- File descriptors are small integers corresponding to a file that has been opend by the current process
    - Standard input is usually file descriptor 0
    - Standard output is usually file descriptor 1
    - Standard error is usually file descriptor 2
    - Further files opend by a process will then be assigned 3, 4, 5, and so forth
    - On POSIX, sockets and pipes are also seen as files, and referenced by file descriptors
- Note the distinction of file object and file descriptor
    - File object: A handler object to access file content, has `write()` and `read()` methods
    - File descriptor: An integer that corresponding to a file
- The `file_object.fileno()` method can be used to obtain the file descriptor associated with a file object
- File descriptor is intended for low-level usage compared to file object
    - Usually `os.ffunction(fd)` has a `os.function(path)` equivalent
- Note that using the file descriptor directly will bypass the file object methods, ignoring aspects such as an internal buffering of data.
- `os.close(fd)`: Close file descriptor `fd`
    - This function is intended for low-level I/O and must be applied to a file descriptor rather than a file object
    - To close a file object rather than a file descriptor, use its `close()` method.
- `os.closerange(fd_low, fd_high)`: Close all file descriptors in [fd_low, fd_high), ignoring errors
- `os.copy_file_range(src, dst, count, offset_src=None, offset_dst=None)`: Copy `count` bytes from file descriptor `src`, starting from offset `offset_src`, to file descriptor `dst`, starting from offset `offset_dst`
    - If `offset_src` is `None`, then `src` is read from the current position; respecively for `offset_dst`
- `os.device_encoding(fd)`: Return the encoding of the terminal `fd` as `str`. If `fd` is not connected to a terminal, return `None`.
- `os.dup(fd)`: Return a duplicate of file descriptor `fd`
    - On Windows, when duplicating a standard stream, the new file descriptor is inheritable. Otherwise, the new file descriptor is non-inheritable.
- `os.dup2(fd, fd2, inheritable=True)`: Duplicate file descriptor `fd` to `fd2`, closing `fd2` first if necessary. Return `fd2`. The new file descriptor is inheritable by default or non-inheritable if `inheritable` is `False`.
- `os.fchmod(fd, mode)`: Change the mode of the file `fd` to numeric `mode`
    - `mode` may take one of the following values or bitwise ORed combinations of them
        - `stat.S_ISUID`: Set UID bit
        - `stat.S_ISGID`: Set group ID bit
            - For a directory it indecate that:
                - Files created under this directory inherit their group ID from this directory rather than the effective group ID of the creating process
                - Directories created under this directory inherit their `stat.S_ISGID` bit set from this directory.
            - For a file that does not have the group execution bit `S_IXGRP` set, the set-group-ID bit indicates mandatory file/record locking.
        - `stat.S_ISVTX`: Sticky bit. When this bit is set on a directory it means that a file in this directory can be renamed or deleted only by the owner of the file, by the owner of the directory, or by a privileged process.
        - `stat.S_IRWXU`: Mask for file owner permissions
        - `stat.S_IRUSR`: Owner has read permission
        - `stat.S_IWUSR`: Owner has write permission
        - `stat.S_IXUSR`: Owner has execute permission
        - `stat.S_IRWXG`: Mask for group permissions
        - `stat.S_IRGRP`: Group has read permission
        - `stat.S_IWGRP`: Group has write permission
        - `stat.S_IXGRP`: Group has execute permission
        - `stat.S_IRWXO`: Mask for permission for others
        - `stat.S_IROTH`: Others have read permission
        - `stat.S_IWOTH`: Others have write permission
        - `stat.S_IXOTH`: Others have execute permission
        - `stat.S_ENFMT`: System V file locking enforcement
            - This flag is shared with `stat.S_ISGID`: file/record locking is enforced on files that do not have the group execution bit (`stat.S_IXGRP`) set
        - `stat.S_IREAD`: Unix V7 synonym for `S_IRUSR`
        - `stat.S_IWRITE`: Unix V7 synonym for `S_IWUSR`
        - `stat.S_IEXEC`: Unix V7 synonym for `S_IXUSR`
- `os.fchown(fd, uid, gid)`: Change the owner and group id of file `fd` to numeric `uid` and `gid`. To leave one of the ids unchanged, set it to -1
- `os.fdatasync(fd)`: Force write of file `fd` to disk. Does not force update of metadata.
- `os.fpathconf(fd, name)`: Return system configuration information relevant to file `fd`
    - `name`: Configuration value to retrieve
        - May be a string or integer
        - Names known to os are given in dictionary `os.pathconf_names`
- `os.fstat(fd)`: Return status `stat_result` object of file `fd`
- `os.fstatvfs(fd)`: Return information of filesystem containing file `fd`
- `os.fsync(fd)`: Force write of file `fd` to disk
    - For a buffered Python file object `f`, `f.flush(); os.fsync(f.fileno())` to ensure that all internal buffers associated with `f` are written to disk
- `os.ftruncate(fd, length)`: Truncate the file `fd` so that it is at most `length` bytes in size
- `os.get_blocking(fd)`: Get the blocking mode of file `fd`: `False` if the `0_NONBLOCK` flag is set, `True` if the flag is cleared
- `os.isatty(fd)`: Return `True` if file `fd` is open and connected to a tty device, else `False`
- `os.lockf(fd, cmd, len)`: Apply, test or remove a POSIX lock on an open file `fd`
    - `cmd`: Command to use, one of `os.F_LOCK`, `os.F_TLOCK`, `os.F_ULOCK`, `os.F_TEST`
    - `len`: Section of the file to lock
- `os.lseek(fd, pos, how)`: Set the current position of file `fd` to `pos`, modified by `how`. Return the new cursor position in bytes, starting from the beginning.
    - `how`
        - `os.SEET_SET` or `0` to set the position relative to the beginning of the file
        - `os.SEET_CUR` or `1` to set the position relative to the current position
        - `os.SEEK_END` or `2` to set the position relative to the end of the file
- `os.open(path, flags, mode=0o777, *, dir_fd=None)`: Open the file `path` and set various flags according to `flags` and possibly its mode according to `mode`. Return the file descriptor for the newly opened file.
    - This function is intended for low-level I/O. For normal usage, use the built-in function `open()`
    - To wrap a file descriptor in a file object, use `fdopen()`
    - When computing `mode`, the current umask value is first masked out
    - The new file descriptor is non-inheritable
    - Flag constants are defined in `os` module and can be combined using bitwise OR operator `|`
    - This function supports paths relative to directory descriptor with `dir_fd` parameter.
- `os.openpty()`: Open a pseudo-terminal pair. Return a pair of file descriptors `(master, slave)` for the pty and the tty, respectively
    - The new file descriptors are non-inheritable
    - For a more portable approach, use `pty` module
- `os.pipe()`: Create a pipe. Return a pair of file descriptors `(r, w)` usable for reading and writing, respectively
    - The new file descriptors are non-inheritable
- `os.pipe2(flags)`: Create a pipe with `flags` set atomically. Return a pair of file descriptors `(r, w)` usable for reading and writing, respectively.
    - `flags` can be constructed by ORing together one or more of these values: `os.O_NONBLOCK`, `os.O_CLOEXEC`
- `os.posix_fallocate(fd, offset, len)`: Ensures that enough disk space is allocated for the file `fd` starting from `offset` and continuing for `len` bytes.
- `os.posix_fadvise(fd, offset, len, advice)`: Announces an intention to access data in a specific pattern thus allowing the kernel to make optimizations. The advice applies to the region of file `fd` starting at `offset` and  continuing for `len` bytes
    - `advice` is one of `os.POSIX_FADV_NORMAL`, `os.POSIX_FADV_SEQUENTIAL`, `os.POSIX_FADV_RANDOM`, `os.POSIX_FADV_NOREUSE`, `os.POSIX_FADV_WILLNEED` or `os.POSIX_FADV_DONTNEED`
- `os.pread(fd, n, offset)`: Read at most `n` bytes from file `fd` at a position of `offset`, leaving the file offset unchanged. Return a `bytes` containing the bytes read.
- `os.preadv(fd, buffers, offset, flags=0)`: Read from a file descriptor `fd` at a position of `offset` into mutable bytes-like objects `buffers`, leaving the file offset unchanged. Transfer data into each buffer until it is full and then move on the the next buffer in the sequence to hold the rest of data. Return the total number of bytes actually read.
    - The flags argument contains a bitwise OR of zero or more of the following flags: `os.RWF_HIPRI`, `os.RWF_NOWAIT`
- `os.pwrite(fd, str, offset)`: Write the bytestring in `str` to file `fd` at position `offset`, leaving the file offset unchanged. Return the number of bytes actually written.
- `os.pwritev(fd, buffers, offset, flags=0)`: Write the `buffers` contents to file `fd` at a offset `offset`, leaving the file offset unchanged. Buffers are processed in array order. Return the total number of bytes actually written.
    - `buffers`: Sequence of bytes-like objects
    - `flags`: Contains a bitwise OR of zero or more of the following flags: `os.RWF_DSYNC`, `os.RWF_SYNC`
- `os.read(fd, n)`: Read at most `n` bytes from file `fd`. Return a bytestring containing the bytes read.
    - This function is intended for low-level I/O. To read from a file object, use its `read()` or `readline()` methods.
- `os.sendfile(out, in, offset, count[[, headers][, trailers], flags=0])`: Cope `count` bytes from file `in` to file `out` starting at `offset`. Return the number of bytes sent. When EOF is reached return 0.
- `os.set_blocking(fd, blocking)`: Set the blocking mode of file `fd`. Set the `os.O_NONBLOCK` flag if blocking is `False`, clear the flag otherwise.
- `os.readv(fd, buffers)`: Read from file `fd` into a number of mutable bytes-like objects `buffers`. Return the total number of bytes actually read.
- `os.tcgetpgrp(fd)`: Return the process group associated with the terminal given by `fd`
- `os.tcsetpgrp(fd, pg)`: Set the process group associated with the terminal given by `fd` to `pg`.
- `os.ttyname(fd)`: Return a string which specifies the terminal device associated with file `fd`. If `fd` is not associated with a terminal device, an exception is raised.
- `os.write(fd, str)`: Write the bytestring in `str` to file `fd`. Return the number of bytes actually written.
    - This function is intended for low-level I/O. To write to a file object, use its `write()` method
- `os.writev(fd, buffers)`: Write the contents of `buffers` to file `fd`. `buffers` must be a sequence of bytes-like objects. Buffers are processed in array order. Returns the total number of bytes actually written.

### 3.1. Querying the size of a terminal

- `os.get_terminal_size(fd=STDOUT_FILENO)`: Return the size of the terminal window `fd` as `(columns_in_characters, lines_in_characters)`
    - Recommand `shutil.get_terminal_size()` instead

### 3.2. Inheritance of File Descriptors

- A file descriptor has an inheritable flag which indecates if the file descriptor can be inherited by child processes. File descriptors created by Python are non-inheritable by default.
- On POSIX, non-inheritable file descriptors are closed in child processes at the execution of a new program, other file descriptors are inherited.
- On Windows, non-inheritable handles and file descriptors are closed in child processes, except for standard streams (0 stdin, 1 stdout, 2 stderr), which are always inherited.
    - Using `spawn*` functions, all inheritable handles and all inheritable file descriptors are inherited.
    - Using `subprocess` module, all file descriptors except standard strams are closed, and inheritable handles are only inherited if the `close_fds` parameter is `False`
- `os.get_inheritable(fd)`: Get inheritable flag of file `fd` as boolean
- `os.set_inheritable(fd, inheritable)`: Set inheritable flag of file `fd` to `inheritable`
- `os.get_handle_inheritable(handle)`: Get inheritable flag of handle `handle` as boolean
- `os.set_handle_inheritable(handle, inheritable)`: Set inheritable flag of handle `handle` to `inheritable`

## 4. Files and Directories

- Normally `path` arguement must be a string specifying a path. However, some functions also accept an open file descriptor for `path` argument
    - Use `os.supports_fd` function set to check whether or not `path` can be a file descriptor: `fuction in os.supports_fd`
    - An error will be raise when specify `dir_fd` or `follow_symlinks` with `path` as a file descriptor
- If `dir_fd` is not `None`, it is a file descriptor referring to the directory which `path` is based on. If `path` is absolute, `dir_fd` is ignored
    - Use `os.supports_dir_fd` function set to check whether or not `dir_fd` is supported: `function in os.supports_dir_fd`
- If `follow_symlinks` is `False`, and the last element of `path` is a symbolic link, the function will operate on the symbolic link itself rather than the file pointed to by the link.
- If `follow_symlinks` is `True` (default), and the last element of `path` is a symbolic link, the function will operate on the file pointed to by the symbolic link rather than the link itself.
    - Use `os.supports_follow_symlinks` function set to check whether or not `follow_symlinks` is supported: `function in os.supports_follow_symlinks`
- If `effective_ids` is `True`, function will operate with the effetive uid/gid instead of the real uid/gid
    - Use `os.supports_effective_ids` function set to check whether or not `effective_ids` is supported: `function in os.supports_effective_ids`
- `os.access(path, mode, *, dir_fd=None, effective_ids=False, follow_symlinks=True)`: Use the real uid/gid to test for access to `path`. Return `True` if access is allowed, `False` if not
    - `mode` should be `os.F_OK` to test the existence of `path`, or it can be the inclusive OR of one or more of `os.R_OK`, `os.W_OK` and `os.X_OK` to test permissions.
    - Note that most operations will use the effective uid/gid, therefore this routine can be used in a suid/sgid environment to test if the invoking user has the specified access to `path`
    - It is preferable to use EAFP rather than check before operation.
- `os.chdir(path)`: Change the current working directory to `path`
- `os.chflags(path, flags, *, follow_symlinks=True)`: Set the flags of `path` to the numeric `flags`
    - `flags` may take a bitwise OR of the following values:
        - `stat.UF_NODUMP`: Do not dump the file
        - `stat.UF_IMMUTABLE`: The file may not be changed
        - `stat.UF_APPEND`: The file may only be appended to
        - `stat.UF_OPAQUE`: The directory is opaque when viewed through a union stack
        - `stat.UF_NOUNLINK`: The file may not be renamed or deleted
        - `stat.UF_COMPRESSED`: The file is stored compressed
        - `stat.UF_HIDDEN`: The file should not be displayed in a GUI
        - `stat.SF_ARCHIVED`: The file may be archived
        - `stat.SF_IMMUTABLE`: The file may not be changed
        - `stat.SF_APPEND`: The file may only be appended to
        - `stat.SF_NOUNLINK`: The file may not be renamed or deleted
        - `stat.SF_SNAPSHOT`: The file is a snapshot file
- `os.chmod(path, mode, *, dir_fd=None, follow_symlinks=True)`: Change the mode of `path` to the numeric `mode`
    - `mode` may take one of the following values or bitwise ORed combinations of them
        - `stat.S_ISUID`: Set UID bit
        - `stat.S_ISGID`: Set group ID bit
            - For a directory it indecate that:
                - Files created under this directory inherit their group ID from this directory rather than the effective group ID of the creating process
                - Directories created under this directory inherit their `stat.S_ISGID` bit set from this directory.
            - For a file that does not have the group execution bit `S_IXGRP` set, the set-group-ID bit indicates mandatory file/record locking.
        - `stat.S_ISVTX`: Sticky bit. When this bit is set on a directory it means that a file in this directory can be renamed or deleted only by the owner of the file, by the owner of the directory, or by a privileged process.
        - `stat.S_IRWXU`: Mask for file owner permissions
        - `stat.S_IRUSR`: Owner has read permission
        - `stat.S_IWUSR`: Owner has write permission
        - `stat.S_IXUSR`: Owner has execute permission
        - `stat.S_IRWXG`: Mask for group permissions
        - `stat.S_IRGRP`: Group has read permission
        - `stat.S_IWGRP`: Group has write permission
        - `stat.S_IXGRP`: Group has execute permission
        - `stat.S_IRWXO`: Mask for permission for others
        - `stat.S_IROTH`: Others have read permission
        - `stat.S_IWOTH`: Others have write permission
        - `stat.S_IXOTH`: Others have execute permission
        - `stat.S_ENFMT`: System V file locking enforcement
            - This flag is shared with `stat.S_ISGID`: file/record locking is enforced on files that do not have the group execution bit (`stat.S_IXGRP`) set
        - `stat.S_IREAD`: Unix V7 synonym for `S_IRUSR`
        - `stat.S_IWRITE`: Unix V7 synonym for `S_IWUSR`
        - `stat.S_IEXEC`: Unix V7 synonym for `S_IXUSR`
    - All bits other than `stat.S_IWRITE` and `stat.S_IREAD` are ignored on Windows
- `os.chown(path, uid, gid, *, dir_fd=None, follow_symlinks=True)`: Change the owner and group id of `path` to the numeric `uid` and `gid`. To leave one of the ids unchanged, set it to -1.
    - Recommand `shutil.chown()` instead
- `os.chroot(path)`: Change the root directory of the current process to `path`
- `os.fchdir(fd)`: Change the current working directory to directory `fd`
- `os.getcwd()`: Return a string representing the current working directory
- `os.getcwdb()`: Return a bytestring representing the current working directory
- `os.lchflags(path, flags)`: Set the flags of `path` to the numeric `flags`, like `os.chflags()`, but do not follow symbolic links
- `os.lchmod(path, mode)`: Change the mode of `path` to the numeric `mode`, like `os.chmod()`, but do not follow symbolic links
- `os.lchown(path, uid, gid)`: Change the owner and group id of `path` to the numeric `uid` and `gid`, like `os.chown()`, but do not follow symbolic links
- `os.link(src, dst, *, src_dir_fd=None, dst_dir_fd=None, follow_symlinks=True)`: Create a hard link pointing to `src` named `dst`
- `os.listdir(path='.')`: Return a list containing the names of the entries in the directory given by `path`. The list is in arbitrary order, and does not include the special entries `'.'` and `'..'` even if they are present in the directory.
- `os.lstat(path, *, dir_fd=None)`: Perform the equivalen of an `lstat()` system call on the given `path`, like `os.stat()`, but do not follow symbolic links. Return a `stat_result` object.
- `os.mkdir(path, mode=0o777, *, dir_fd=None)`: Create a directory `path` with numeric mode `mode`
    - If the directory already exists, `FileExistsError` is raised
- `os.makedirs(name, mode=0o777, exist_ok=False)`: Recursive directory creation function. Like `os.mkdir()`, but makes all intermediate-level directories needed to contain the leaf directory.
    - `mode` affects leaf directory only. To set the mode of any newly-created parent directories, set the umask before invoking `os.makedirs()`
    - If `exist_ok` is `False` (default), `FileExistsError` is raised if the target directory already exists
- `os.mkfifo(path, mode=0o666, *, dir_fd=None)`
