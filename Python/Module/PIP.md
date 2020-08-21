# PIP

## 1. Upgrade

```zsh
pip install --upgrade <Package> # Upgrade Package
pip install -U <Package> # Upgrade Package
```

## 2. Mirror

```powershell
vim ~/AppData/Roaming/pip/pip.ini  # Windows
vim ~/.config/pip/pip.conf  # POSIX
```

- Notice: Tailing `/` is important

```plain
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
trusted-host = https://pypi.tuna.tsinghua.edu.cn/simple/

[[source]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
verify_ssl = true
name = "pypi"
```