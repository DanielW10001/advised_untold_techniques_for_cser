# ufw: Uncomplicated Firewall

## 1. CLI

`ufw OPTION COMMAND`

- `COMMAND`
    - Firewall
        - `enable`
        - `disable`
        - `reload`
        - `reset`
        - `status [numbered|verbose]`
            - `numbered`: With Rule IDs
        - `default POLICY`: Set Default Policy
            - `POLICY`: `allow|deny|reject [incoming|outgoing|routed]`
        - `show REPORT`
            - `REPORT`: `raw|builtins|(before|user|after|logging)-rules|listening|added`
        - `logging on|off|low|medium|high|full`: Logging Level
        - `version`
    - Rule
        - `RULE`: Add
            - ID
            - `[prepend] allow|deny|reject|limit {in|out [on INTERFACE]} [log|log-all] CONNECTION [comment COMMENT]`
                - `CONNECTION`
                    - `APPNAME`
                    - Local Port
                        - `PORT[/PROTOCOL]`
                            - `PORT`: `NUM|NUM:NUM|SERVICE(,NUM|NUM:NUM|SERVICE)*`
                                - `SERVICE`: `ssh|smtp|http|telnet`
                            - `PROTOCOL`: `tcp|udp|ah|esp|gre|ipv6|igmp`
                                - Default to `tcp,udp`
                    - `[PROTO] [FROM] [TO]`
                        - `proto PROTOCOL`
                        - `from SRC [port PORT| app APPNAME]`
                        - `to DEST [port PORT | app APPNAME]`
                        - `SRC`, `DEST`: `any|IP[/BITS]`
        - `delete RULE`: Delete Rule
        - `insert ID RULE`: Insert Rule at Num
            - `RULE`: `PORT[/PROTOCOL]`
    - Route
        - `route RULE`: Add Route Rule
        - `route delete RULE`: Delete Route Rule
        - `route insert ID RULE`: Insert Route Rule at Num
    - Application Profile
        - `APPNAME`: App Name or `all`
        - `app list`
        - `app info APPNAME`
        - `app update [--add-new] APPNAME`
        - `app default skip|POLICY`: Set Default App Policy
- `OPTION`
    - `-h`, `--help`
    - `--dry-run`
    - `--version`
- Rules are evaled and matched as sequence. Once hit, following rules will be ignored.

## 2. Config

- `/etc/default/ufw`: High Level Config
- `/etc/ufw/`
    - `user[6].rules`: User Rules
    - `applications.d/`: App Support
        - `APP.INI`: App Profile
    - `sysctl.conf`: Kernel Network Config
    - `ufw.conf`: Enable on Boot and Log Level
    - `before.init`: Pre Init Script
    - `after.init`: Post Init Script
    - `before[6].rules`: Pre Eval Rules
    - `after[6].rules`: Post Eval Rules

`APP.INI` Format:

```ini
[NAME]
title=TITLE
description=DESC
ports=PORT[/PROTOCOL](|PORT[/PROTOCOL])*
```

## 3. Log

- `IN`, `OUT`: In/Outcoming Interface
- `SRC`, `DST`: Source/Destination IP
- `SPT`, `DPT`: Source/Destination Port
- `PROTO`: Protocol, `TCP` or `UDP`
- `LEN`: Packet Length
- `MAC`: Dest & Src MAC & EtherType Fields
- `TTL`: Time (Jump Count) to Live
- `WINDOW`: Expected Reply Packet Size
- `SYN`: Three-Way Handshake
- `URGP`: Urgent
- IP Header
    - `TOS`: TOS
    - `PREC`: Precdence
