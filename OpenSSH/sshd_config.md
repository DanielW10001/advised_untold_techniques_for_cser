# sshd_config

- System Config:
    - Windows: `%ProgramData%/ssh/sshd_config`
    - Linux: `/etc/ssh/sshd_config`

```sshd_config
# COMMENT; "" for string; Time: `NUM[sSmMhHdDwW]?`
Include PATH( PATH)*

HostCertificate HOST_CERT_PATH( HOST_CERT_PATH)*
HostKey HOST_ID_PATHS( HOST_ID_PATH)*

AuthenticationMethods method(,method)*( method(,method)*)*  # Method: hostbased|publickey|password|keyboard-interactive[:bsdauth|pam|skey]
ChallengeResponseAuthentication no
KbdInteractiveAuthentication yes|no
PasswordAuthentication no
PermitEmptyPasswords yes
PermitRootLogin yes|prohibit-password|forced-commands-only|no
PubkeyAcceptedKeyTypes [+-^]?ALGO(,ALGO)*
PubkeyAuthentication no
StrictModes no

DenyUsers USER[@HOST]( USER[@HOST])*
AllowUsers USER[@HOST]( USER[@HOST])*
DenyGroups GROUP( GROUP)*
AllowGroups GROUP( GROUP)*

ListenAddress HOST[:PORT] [rdomain DOMAIN]
Port PORT
ClientAliveCountMax NUM
ClientAliveInterval SEC
LoginGraceTime SEC
TCPKeepAlive no
Compression no
AddressFamily inet|inet6
RDomain DOMAIN

Ciphers [+-^]?ALGO(,ALGO)*
AuthorizedKeysFile PATH( PATH)*
AuthorizedPrincipalsFile PATH
IgnoreUserKnownHosts yes
CASignatureAlgorithms ALGO(,ALGO)*
HostKeyAlgorithms ALGO(,ALGO)*
AllowAgentForwarding no
UseDNS yes
RekeyLimit NUM[KMG]? TIME
RevokedKeys KEY_LIST_PATH
TrustedUserCAKeys CA_KEY_LIST_PATH
ForceCommand COMMAND
LogLevel QUIET|FATAL|ERROR|INFO|VERBOSE|DEBUG|DEBUG1|DEBUG2|DEBUG3
ExposeAuthInfo yes
HostbasedAcceptedKeyTypes [+-^]?ALGO(,ALGO)*
HostbasedAuthentication yes
HostbasedUsesNameFromPacketOnly yes
IgnoreRhosts shosts-only|no
KexAlgorithms [+-^]?ALGO(,ALGO)*
MACs [+-^]?ALGO(,ALGO)*
PermitListen [HOST:]PORT( [HOST:]PORT)*|none
PermitOpen HOST:PORT( HOST:PORT)*|none
PermitTTY no
PermitTunnel yes|point-to-point|ethernet
PidFile PATH|none
PubkeyAuthOptions touch-required
Subsystem NAME COMMAND  # SFTP: Subsystem SFTP sftp-server

Banner PATH
VersionAddendum SUFFIX
PrintLastLog no
PrintMotd no

PermitUserEnvironment yes|PATTERN
PermitUserRC no
AcceptEnv VAR( VAR)*
SetEnv VAR=VALUE( VAR=VALUE)*

AllowTcpForwarding no|local|remote
DisableForwarding yes
GatewayPorts yes|clientspecified

X11DisplayOffset NUM
X11Forwarding yes

Match (All|(User|Group|Host|LocalAddress|LocalPort|RDomain|Address) PATTERN)(
    SUB_OPTION)*
```

- Tokens
    - `%%`: Literal `%`
    - `%u`: Username
    - `%U`: Numeric User ID
    - `%h`: Home Dir of User
    - `%k`: Key or Certificate
    - `%i`: Key ID of Certificate
    - `%s`: Serial Number of Certificate
    - `%f`: Fingerprint of Key or Certificate
    - `%t`: Type of Key or Certificate
    - `%K`: CA Key
    - `%F`: Fingerprint of CA Key
    - `%T`: Type of CA Key
    - `%D`: Routing Domain
