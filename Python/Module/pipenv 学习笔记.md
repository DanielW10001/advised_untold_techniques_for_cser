# pipenv 学习笔记
## 1. Install pipenv
```bash
sudo apt install pipenv
pip install --user pipenv
python -m site --user-base bin # get local bin
# zsh
vim ~/.zshrc
# Windows: Add Local Bin to Path
```
add local bin to $PATH:
```
export PATH=<LocalBin>:$PATH
```

## 2. Config

- Share pip config file
- System Variable
    - `PIPENV_DEFAULT_PYTHON_VERSION`: `py`
    - `PIPENV_PYPI_MIRROR`: `https://pypi.tuna.tsinghua.edu.cn/simple/`
- Zsh completion
    - Add `eval "$(pipenv --completion)"` to `~/.zshrc`

## 3. Install packages for project
```bash
.../project_dir $ pipenv install <PackageName>
```
Install for develop only:
```bash
.../project_dir $ pipenv install
```
## 4. Using installled packages
```python
import <PackageName>

# Do something
```
Run commands in pipenv's cloned shell:
```bash
$ pipenv run <Command>
```
Run script in pipenv:
```bash
$ pipenv run python <ScriptDie>
```
Jump into and out from pipenv's cloned shell:
```bash
$ pipenv shell
$ exit
```

## 5. Update Package
```bash
pipenv update  # Update every package
pipenv update <Package>
```
## 6. Shareing project
Set up requested packages for another system:
```bash
$ pipenv install
```
Set up dev env:
```bash
$ pipenv install --dev
```
## 7. Rm virtual env

```bash
$ pipenv --rm
```

## 8. Version Control

- Keep Both `Pipfile` and `Pipfile.lock` in version control
