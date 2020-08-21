# systemd: System Management Daemon

## 1. Unit

- unit: `UNITNAME.TYPE`: Managed Resource, Basic Management Unit, Defined with Unit File
    - Filesystem related Units are named after the Path, with leading slash removed, other slashes changed to dashes: `DIR(-DIR)*`
    - Can depend on other units
    - Can has multiple component units
    - State:
        - Load/Unload: Config Parsed/Unparsed
        - Active/Inactive: Running/Not Running
        - En/Disabled: Start/Not Start when triggered (e.g.: at boot)
        - Static: One-Off Only; Used by Other Units
        - Masked/Unmasked: Blocked/Unblocked
    - Features: Socket|Bus|Path|Device-Based Activation, Implicit Dependency Mapping, Template, Security, Snippet

### 1.1. Type

- Types and Unit File Name Extension:
    - `.service`: Daemon Process: `ssh.service`; Definition: Start, Stop, Dependency
    - `.socket`: Network/IPC/Activation Socket; Activation Socket must have associated `.service`
    - `.device`
    - `.mount`
    - `.automount`: Automatically Mounted at boot. Must have matching `.mount`
    - `.swap`
    - `.target`: Synchronization Point for/System State defined with other units
        - Instances: `basic.target`, `multi-user.target`, `graphical.target`, `swap.target`, `poweroff.target`, `reboot.target`, `rescue.target`, `halt.target`
        - `systemd` has a default target to boot the system to, usually `multi-user.target`
    - `.path`: Activation Path; must have matching `.service`
    - `.timer`: Timer for Delayed/Scheduled Activation; must have matching unit
    - `.snapshot`: System Temp Snapshot within Session
    - `.slice`: Linux Control Group Node; used to control its processes' access to resources; named with hierarchical path in `cgroup` tree
    - `.scope`: Externally Created Processes Management Scope

## 2. Unit File

- Unit File: Unit Definition

### 2.1. Location

- Location:
    - Default, Read-Only: `/lib/systemd/system/UNITNAME.TYPE[.d/SNIPPET.conf]`
    - Runtime, Within Session: `/run/systemd/system/UNITNAME.TYPE[.d/SNIPPET.conf]`
    - User-Define, Overriding: `/etc/systemd/system/UNITNAME.TYPE[.d/SNIPPET.conf]`

### 2.2. Format

```systemd
[[X-]SECTION]
OPTION=[1|yes|on|true|0|no|off|false|(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))|VALUE]
```

### 2.3. Content

```systemd
[Unit]
Description=DESC
Documentation=(man:SUBJECT(CHARPT)|URL)( (man:SUBJECT(CHARPT)|URL))*
Requires|Wants|BindsTo|Conflicts=UNIT( UNIT)*  # Can be .target
Before|After=UNIT( UNIT)*  # Can be .target
Condition*=CONDITION
Assert*=ASSERT

[Service]
TimeoutSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))
# Start
(ExecStartPre=[-]PATH ARGS)*  # `-` for tolerance
ExecStart=[-]PATH ARGS
(ExecStartPost=[-]PATH ARGS)*
TimeoutStartSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))
# Reload Config
(ExecReload=[-]PATH ARGS)*
# Stop
ExecStop=[-]PATH ARGS  # Default to kill
(ExecStopPost=[-]PATH ARGS)*
TimeoutStopSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))
# Restart
RestartSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))
Restart=always|on-(success|failure|abnormal|abort|watchdog)
# Type
Type=simple|forking|oneshot|dbus|notify|idle
RemainAfterExit=yes|no  # oneshot
PIDFile=PATH  # forking
BusName=NAME  # dbus
NotifyAccess=none|main|all  # notify

[Socket]  # For Communication/Activation
Service=UNIT  # Default to UNITNAME.service
Accept=yes  # Additional Instance for each Connection
# Socket Path
ListenStream=PATH  # TCP
ListenDatagram=PATH  # UDP
ListenSequentialPacket=PATH  # Unix Socket
ListenFIFO=PATH  # FIFO Buffer
# Unix Socket
SocketUser=OWNER  # Default to root
SocketGroup=GROUP  # Default to root
# Unix Socket or FIFO Buffer
SocketMode=PERMISSION_MODE

[Mount]
What=RESOURCE_PATH
Where=MOUNT_PATH
Type=FILE_SYSTEM
Options=OPTION(,OPTION)*
SloppyOptions=yes|no
DirectoryMode=PERMISSION
TimeoutSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))

[Automount]  # Auto Mounted at Boot
Where=MOUNT_PATH
DirectoryMode=PERMISSION

[Swap]
What=SPACE_PATH
Priority=INT
Options=OPTION(,OPTION)*
TimeoutSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))

[Path]  # For Activation
Unit=UNIT  # Default to UNITNAME.service
# Trigger
PathExists=PATH
PathExistsGlob=PATTERN
PathChanged=PATH
PathModified=PATH
DirectoryNotEmpty=PATH
# Pre Watch
MakeDirectory=yes
DirectoryMode=PERMISSION

[Timer]  # For Activation
WakeSystem=yes  # Wake system from suspend state
# Unit Activation
Unit=UNIT  # Default to UNITNAME.service
Persistent=yes  # Keep inactive period's triggers
#     Delay
OnActiveSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))  # to Timer Setted (Activated)
OnBootSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))  # to Boot
OnStartupSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))  # to systemd Start
#     Absolute
OnCalendar=([DAY] [YEAR-MONTH-DAY[..DAY]] HH[,HH]:MM[:SS]|daily|weekly|monthly|yearly)
#     Time Window
AccuracySec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))
# Timer Resetting
#     Delay
OnUnitActiveSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))  # to Unit Activation
OnUnitInactiveSec=(SEC|NUM(ms|m[in]|h[our]|d[ay]|y[ear]))  # to Unit Inactivate Mark

[Slice|Scope]
# ...

[Install]  # En/Disable Behavior
(Required|Wanted)By=UNIT  # Commonly [multi-user].target; indicate /etc/systemd/system/UNIT.requires|wants/SYMLINK_TO_WANTED
Alias=UNIT
Also=UNIT( UNIT)*
DefaultInstance=FALLBACK_NAME
```

### 2.4. Template

- Allow Runtime Infos
- Template Unit File: `TEMPLATE_NAME@.TYPE`
- Instance Unit File: `TEMPLATE_NAME@INSTANCE_NAME.TYPE`
    - Usually a symlink to template unit file
- Var Specifier: Interpreted when as instance
    - `%n`: Unit Name
    - `%N`: Unescaped Unit Name
    - `%p`: Template Name
    - `%P`: Unescaped Template Name
    - `%i`: Instance Name
    - `%I`: Unescaped Instance Name
    - `%f`: Unescaped `/INSTANCE_NAME`
    - `%c`: Control Group
    - `%u`: User Name
    - `%U`: UID
    - `%H`: Host Name
    - `%%`: Literal `%`

## 3. Architecture

- Parts
    - `system`: Process
    - `journal`: Centralized Log
    - `timedate`: Time & Date
    - `login`
    - `hostname`
- CLI Utility: `PARTctl`
- Daemon: `PARTd`: Management Background Process

### 3.1. Daemon Config

- Path: `/etc/systemd/DAEMON.conf`

```systemd
[Journal]  # journald.conf
Storage=persistent  # Keep Former Boots
# Limit
SystemMaxUse=NUM[KMGT]  # Disk Bytes
SystemKeepFree=NUM[KMGT]  # Free Disk Bytes
SystemMaxFileSize=NUM[KMGT]  # Single Log Bytes
RuntimeMaxUse=NUM[KMGT]  # Runtime Disk Bytes
RuntimeKeepFree=NUM[KMGT]  # Runtime Free Disk Bytes
RuntimeMaxFileSize=NUM[KMGT]  # Runtime Single Log Bytes
```

## 4. Ref

- [systemd Unit](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)
