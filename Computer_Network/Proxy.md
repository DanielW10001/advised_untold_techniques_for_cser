# Proxy

Forward Proxy 正向代理

```puml
[Client] -> [Proxyer]: From: Client\nBy: Proxyer\nTo: Server
[Proxyer] -> [Server]: From: Proxyer\nBy: None\nTo: Server
[Server] -> [Proxyer]: From: Server\nBy: None\nTo: Proxyer
[Proxyer] -> [Client]: From: Server\nBy: Proxyer\nTo: Client
```

Reverse Proxy 反向代理

```puml
[Client] -> [Proxyer]: From: Client\nBy: None\nTo: Proxyer
[Proxyer] -> [Server]: From: Client\nBy: Proxyer\nTo: Server
[Server] -> [Proxyer]: From: Server\nBy: Proxyer\nTo: Client
[Proxyer] -> [Client]: From: Proxyer\nBy: None\nTo: Client
```

Transparent Proxy 透明代理

```puml
[Client] -> [Proxyer/Hijacker]: From: Client\nBy: None\nTo: Server
[Proxyer/Hijacker] -> [Modified Server]: MODIFIED\nFrom: Client\nBy: None\nTo: Modified Server
[Modified Server] -> [Proxyer/Hijacker]: From: Modified Server\nBy: None\nTo: Client
[Proxyer/Hijacker] -> [Client]: MODIFIED\nFrom: Server\nBy: None\nTo: Client
```
