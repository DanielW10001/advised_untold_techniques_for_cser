# ps: Process Status

`ps [OPTION]...`

- Option
    - Selection
        - `-PID`
        - All
            - `-A`, `-e`: All
            - `-a`: All except session leaders and daemon
            - `-d`: All except session leaders
            - `-N`, `--deselect`: All except matched
        - Filter
            - `-p|--pid|-q|--quick-pid PID(,PID)*`
            - `-C CMD(,CMD)*`: Command Name
            - `-G|--Group GROUP(,GROUP)*`: Group Name/RealID
            - `--group GROUP(,GROUP)*`: Group Name/EffectiveID
            - `--ppid PID(,PID)*`: Parent PID
            - `-s|--sid SID(,SID)*`: Session ID
            - `-t|--tty TTY(,TTY)*`: TTY, can be `[/dev/][tty]SUFFIX` or `-` for daemon
            - `-U|--User USER(,USER)*`: User Name/RealID
            - `-u|--user USER(,USER)*`: User Name/EffectiveID
