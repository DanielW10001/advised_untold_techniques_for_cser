# PySocks

- Install: `pip install pysocks`
- Module name: `socks`
- Usage

```python3
import socket
import socks
socks.set_default_proxy(socks.SOCKS5, "localhost", 1080)  # Set Proxy
socket.socket = socks.socksocket
```
