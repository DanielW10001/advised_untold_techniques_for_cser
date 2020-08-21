# permission

- `~`: `755`
- `~/.ssh`: `700`
    - `authorized_keys`: `600`, owned by user
    - `config`: `600`
    - Private Key: `400`
    - `*.pub`: `644`
    - `known_hosts`: `600`
- `/etc/ssh`
    - `ssh[d]_config`: `644`
    - Private Key: `400`, owned by root
    - `*.pub`: `644`
    - `ssh_known_hosts`: `644`
