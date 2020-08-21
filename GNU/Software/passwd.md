# passwd

- `passwd OPTION`: Set new password
- Option
    - `-l USER`, `--lock USER`
    - `-u USER`, `--unlock USER`
    - `-S USER`, `--status USER`
- User
    - Disable: `sudo passwd -l USER`
    - Check if enabled: `sudo passwd -S USER`
- `root`
    - Enable: `sudo passwd`
