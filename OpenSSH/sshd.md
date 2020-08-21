# sshd: SSH Daemon

- Restart: `sudo service sshd restart`
- Debug: `/usr/sbin/sshd -ddd`

CLI Options:

```bash
-4  # IPv4 Only
-6  # IPv6 Only
-C CONNECTION_SPEC  # Conditions for Match config; Debug only
-c HOST_CERT_PATH
-D  # Not Daemon
-d[d[d]]  # Debug Mode
-E LOG_PATH  # Log to LOG_PATH instead of system log
-e  # Log to stderr instead of system log
-f CONFIG_FILE  # Default to /etc/ssh/sshd_config
-g LOGIN_TIME
-h HOST_ID_PATH
-i  # Run from inetd
-o OPTION
-p PORT
-q  # Quiet
-T  # Extended Test
-t  # Test
-uNUM  # `utmp` remote host name length limit
```
