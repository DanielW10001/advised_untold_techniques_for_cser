# journalctl

```bash
journalctl \  # View Log
    [--utc] \  # UTC instead of Local Time
    [-a|--all] \  # Including Long and Unprintable
    [-r|--reverse] \  # Newest First
    [--no-full] \  # Ellipsize
    [--no-pager] \  # Print instead of less
    [(-o |--output=)MODE] \  # MODE: short[-(iso|precise|iso-precise|full|monotonic|unix)], verbose, export, json[-(pretty|sse|seq)], cat, with-unit
    [-x|--catalog] \  # Include Explanations
    [-q|--quiet] \  # No Warns
    [-f|--follow] \  # Live Update
    [-e|--pager-end] \  # Jump to End
    {(-u |--unit=)UNIT} \
    [--system|--user] \  # System/User Log
    [-k|--dmesg] \  # Kernel Log
    [(-p |--priority=)PRIORITY] \  # PRIORITY: emerg(0), alert(1), crit(2), err(3), warning(4), notice(5), info(6), debug(7)
    [-b[ BOOT]|--boot[=BOOT]] \  # [Most Recent] Boot
    [(-S |--since=)"[YYYY-MM-DD] [HH:MM[:SS]]|now|yesterday|today|tomorrow|[+-](SEC|NUM(m[in]|h[our]|d[ay]|y[ear]))[ ago]"] \
    [(-U |--until=)"[YYYY-MM-DD] [HH:MM[:SS]]|now|yesterday|today|tomorrow|[+-](SEC|NUM(m[in]|h[our]|d[ay]|y[ear]))[ ago]"] \
    [FIELD=VALUE]... \  # FIELD: _(P|U|G)ID; _SYSTEMD_UNIT
    [-n[ NUM]|--lines[=NUM]] \  # Last NUM (Default to 10) Entries
    [(-g |--grep=)PATTERN] [--case-sensitive] \
    [PATH]...  # Originating Paths
journalctl --list-boots  # OFFSET ID BOOT-POWEROFF; Boots are refed with OFFSET or ID
journalctl (-N|--fields)  # List Field Names
journalctl (-F |--field=)FIELD  # List Stored Field Values
journalctl --disk-usage  # Print Disk Usage
journalctl --vacuum-size=NUM[KMGT]  # Clean by Bytes
journalctl --vacuum-files=COUNT  # Clean by File Counts
journalctl --vacuum-time=(SEC|NUM(m[in]|h[our]|d[ay]|y[ear]))  # Clean by Time
```
