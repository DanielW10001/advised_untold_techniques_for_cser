# shutil

```python3
# Directory and file
# Copy
shutil.copyfileobj(srcobj, dstobj)  # Copy file object content
shutil.copyfile(srcpath, dstpath)  # Copy content
shutil.copy(srcpath, dstpath)  # Copy content and permission
shutil.copy2(srcpath, dstpath)  # Copy content and metadata
shutil.copymode(srcpath, dstpath)  # Copy permission
shutil.copystat(srcpath, dstpath)  # Copy permission, last access time, last modification time, flags, extended attributes
shutil.copytree(srcpath, dstpath[, ignore=shutil.ignore_patterns('glob_pattern'(, 'glob_pattern')*)][, dirs_exist_ok])  # Copy dir tree; glob_pattern: ?, *, **, []
# Move
shutil.move(srcpath, dstpath)  # Move file or dir
# Remove
shutil.rmtree(path)  # Remove dir tree
# Info
usage = shutil.disk_usage(path)  # Disk usage in bytes
usage.total
usage.used
usage.free
shutil.which(cmd[, mode=os.F_OK])
# Modify
shutil.chown(path[, user][, group])

# Archived file
shutil.make_archive(arch_base_name, format=('zip'|'tar'), src_dir[, logger=logger])
shutil.unpack_archive(filename[, extract_dir])

# Terminal
size = shutil.get_terminal_size()
size.columns
size.lines
```
