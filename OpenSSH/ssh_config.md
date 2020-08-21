# ssh_config

- System:
    - Windows: `%ProgramData%/ssh/ssh_config`
    - POSIX: `/etc/ssh/ssh_config`
- User: `~/.ssh/config`

```ssh_config
# VAR="VALUE"
# Comment; "" for string; Time: `NUM[sSmMhHdDwW]?`
Include PATH( PATH)*
IgnoreUnknown PATTERN( PATTERN)*

Host PATTERN( PATTERN)*  # Matching Host; * for any content; ? for any char; ! for negating
Match (all|canonical|final|exec COMMAND|(host|originalhost|user|localuser PATTERN))  # Matching Condition; ! for negating
    # Host Specific Config

    HostName HOSTNAME  # Connecting Host
    Port PORT
    User USER

    PreferredAuthentications hostbased,publickey,password,keyboard-interactive
    PubkeyAuthentication no
    IdentityFile IDPATH( IDPATH)*
    IdentitiesOnly yes
    RevokedHostKeys KEY_LIST_PATH
    PasswordAuthentication no
    NumberOfPasswordPrompts NUM
    KbdInteractiveAuthentication no

    ProxyCommand connect.exe -S HOST:PORT %h %p
    ProxyCommand /usr/bin/nc -X 5 -x HOST:PORT %h %p
    ProxyCommand none
    ProxyJump [USER@]HOST[:PORT](, [USER@]HOST[:PORT])*

    AddressFamily (inet|inet6)
    BatchMode yes
    Compression yes
    LogLevel (QUIET|FATAL|ERROR|INFO|VERBOSE|DEBUG|DEBUG1|DEBUG2|DEBUG3)
    RequestTTY (no|yes|force|auto)
    SendEnv VAR( VAR)*
    SetEnv VAR=VALUE( VAR=VALUE)*

    ConnectionAttempts NUM
    ConnectTimeout SEC
    ServerAliveCountMax NUM
    ServerAliveInterval SEC
    TCPKeepAlive no

    PubkeyAcceptedKeyTypes [+-^]?ALGO(, ALGO)*
    CASignatureAlgorithms ALGO(, ALGO)*
    CertificateFile PATH
    CheckHostIP no
    Ciphers [+-^]?ALGO(, ALGO)*
    HostKeyAlgorithms [+-^]?ALGO(, ALGO)*
    HostKeyAlias ALIAS
    KexAlgorithms [+-^]?ALGO(, ALGO)*
    AddKeysToAgent yes|ask|confirm
    ForwardAgent yes
    RekeyLimit NUM[KMG]? TIME
    UserKnownHostsFile PATH( PATH)*
    StrictHostKeyChecking yes|accept-new|no|off
    UpdateHostKeys yes|no|ask
    VerifyHostKeyDNS yes|ask
    VisualHostKey yes
    HashKnownHosts yes

    GatewayPorts yes
    DynamicForward [BIND_IP:]PORT
    LocalForward [BIND_IP:]PORT HOST:PORT
    RemoteForward [BIND_IP:]PORT [HOST:PORT]
    ExitOnForwardFailure yes
    Tunnel yes|point-to-point|ethernet
    TunnelDevice LOCAL_TUN[:REMOTE_TUN]
    ClearAllForwardings yes

    PermitLocalCommand yes
    LocalCommand COMMAND
    RemoteCommand COMMAND

# General Config
```

- Pattern List: `PATTERN(,PATTERN)*`
- Tokens
    - `%%`: Literal `%`
    - `%C`: Connection Hash (Hash of `%l%h%p%r`)
    - `%L`: Local hostname
    - `%l`: Local hostname including domain name
    - `%u`: Local username
    - `%d`: Local user's home dir
    - `%i`: Local user ID
    - `%T`: Local `tun` or `tap` interface
    - `%h`: Remote hostname
    - `%k`: Remote hostkey alias/hostname
    - `%n`: Original remote hostname
    - `%r`: Remote username
    - `%p`: Remote port
- Env Var: `${VAR}`
