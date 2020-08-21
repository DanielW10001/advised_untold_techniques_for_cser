# Poetry

```python3
# New package
poetry new package
cd package
vim pyproject.toml  # [tool.poetry]
# Or
mkdir PACKAGE
cd PACKAGE
poetry init

# Exist package
vim pyproject.toml  # [tool.poetry]

# Deploy package
poetry install

# Manage package
poetry show
poetry search package
poetry add( package)+
poetry remove( package)+
poetry update( package)*

# Use package
poetry shell
poetry run command
```

Python dependency management and packaging.

- Depandency Management
    - Allows you to declare the libraries your peoject depends on
    - Manage (Install/Update) them for you

[TOC]

## 1. Meta Managemenet

### 1.1. Installation

- `curl --proxy socks5://127.0.0.1:1080 -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python`
- Temporary workaround for poetry on python 3.8: `cd C:\Users\danie\.poetry\lib\poetry\_vendor; cp -r ./py3.7 ./py3.8`

### 1.2. Uninstallation

- `curl -O --proxy socks5://127.0.0.1:1080 -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py`
- `python ./get-poetry.py --uninstall`

### 1.3. Update

- `poetry self:update`

### 1.4. Cache

- Windows: `C:\Users\<username>\AppData\Local\pypoetry\Cache`

## 2. Config

### 2.1. Mirror

- Will respect pip config file `pip.ini`
- Add mirror to per project config

### 2.2. Tab Completion for Shell

```bash
# Bash
poetry completions bash > /etc/bash_completion.d/poetry.bash-completion

# Oh-My-Zsh
mkdir $ZSH/plugins/poetry
poetry completions zsh > $ZSH/plugins/poetry/_poetry
vim ~/.zshrc
# Add:
# plugins(
#     poetry
#     ...
#     )
```

### 2.3. Poetry

- Config file
    - POSIX: `~/.config/pypoetry/config.toml`
    - Windows: `C:\Users\<UserName>\AppData\Roaming\pypoetry\config.toml`
- Config
    - `poetry config[ OPTIONS] CONFIG_KEY CONFIG_VALUE...`
    - Edit `config.toml` directly
- `settings.virtualenvs.create`
    - Create new virtualenv if not exist
    - Value: boolean (true/false)
        - Default: true
- `settings.virtualenvs.in-project`
    - Create virtualenv inside project dir
    - Value: boolean (true/false)
        - Default: false
- `settings.virtualenvs.path`
    - Dir virtualenvs putted
    - Value: string
        - Default
            - Windows: `C:\Users\<UserName>\AppData\Local\pypoetry\Cache\virtualenvs`
            - POSIX: `~/.cache/pypoetry/virtualenvs`
- `repositories.<RepoName>`
    - URL of Alternative repo `<RepoName>`
    - Value: String
    - E.g.: `poetry config repositories.foo https://foo.bar/simple/`
- `http.basic.<RepoName>`
    - Credential for repo
    - Value: String String
    - E.g.: `poetry config http.basic.foo username password`

## 3. Concept

- Dependency Package
    - (Default) Production Dependency
    - Developement Dependency

### 3.1. Project and Package

- Every project with `pyproject.toml` is a package

### 3.2. Virtual Environment

- All packages are isolated in project-level virtual envs

### 3.3. `poetry.lock`

- Keep all package and version info
- Keep in Version Control

### 3.4. `pyproject.toml`

- Project dependency meta file: `pyproject.toml`
    - Instead of `Pipfile`, `requirements.txt`, `setup.py`, `setup.cfg`, `MANIFEST.in`
- Implements [PEP 518](https://www.python.org/dev/peps/pep-0518/)
- Orchestrate project and its dependencies
- Keep in Version Control

#### 3.4.1. Structure

```toml
([<Property>(.<Property>)*](
<VarName> = <Value>)*
)*
```

- `[<Property>(.<Property>)*]`
    - Indecate section
- `<VarName>`: Variable Name
- `<Value>`
    - `str`: `"<Content>"`
    - `List`: `[<Element>(, <Element>)*]`

#### 3.4.2. Content

```toml
[tool.poetry]  # Required
name = "<ProjectName>"
version = "X.X.X"
description = "<Description>"
authors = ["<Author> \<<Email>\>"(, "<Author> \<<Email>\>")*]

[[tool.poetry.source]]  # To install from alternative repo
name = "<RepoName>"
url = "<RepoURL>"
[default = true]  # Replace PyPI with this repo
# Mirror
# name = "tuna"
# url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
# default = true

[tool.poetry.scripts]
<ScriptName> = "<Module>:<Fuction>"

[tool.poetry.repositories]
<RepoName> = "<RepositoriesURL>"  # PyPI by default

[tool.poetry.dependencies]
python = "*|^<CurrentVersion>"
<PackageName> = "<VersionConstraint>"
<PackageName> = {version = "<VersionConstraint>"[, python = "<VersionConstraint>"]}  # Installed only for specific python
<PackageName> = [  # Multiply rule
{version = "<VersionConstraint>"[, python = "<VersionConstraint>"]}(,
{version = "<VersionConstraint>"[, python = "<VersionConstraint>"]})*
]
<PackageName> = {git = "<GitRepoURL>"[, rev = "REV"][, tag = "TAG"][, branch = "BRANCH"][, python = "<VersionConstraint>"]}  # Git Package
<PackageName> = {path = "<Package[Arch]Dir>", develop = (true|false)[, python = "<VersionConstraint>"]}  # Local package

[tool.poetry.dev-dependencies]
<PackageName> = "<VersionConstraint>"
<PackageName> = {version = "<VersionConstraint>"[, python = "<VersionConstraint>"]}  # Installed only for specific python
<PackageName> = [  # Multiply rule
{version = "<VersionConstraint>"[, python = "<VersionConstraint>"]}(,
{version = "<VersionConstraint>"[, python = "<VersionConstraint>"]})*
]
<PackageName> = {git = "<GitRepoURL>"[, rev = "REV"][, tag = "TAG"][, branch = "BRANCH"][, python = "<VersionConstraint>"]}  # Git Package
<PackageName> = {path = "<Package[Arch]Dir>", develop = (true|false)[, python = "<VersionConstraint>"]}  # Local package
```

### 3.5. Project Structure

```powershell
<ProjectName>
├── pyproject.toml
├── README.rst
├── <ProjectName>
│   └── __init__.py
└── tests
    ├── __init__.py
    └── test_<ProjectName>.py
```

### 3.6. Packaging

- Library package
    - Format
        - `sdist`: Source format Distribution
        - `wheel`: Compiled format distribution

### 3.7. Repository/Index

- Used to install/publish package
- Default: PyPI indecated by `pip.ini`, named `pypi`
- Config with:
    - For pulish: `config.toml`, `poetry config`
    - For install: `pyproject.toml`

### 3.8. Version Constraint

- Define what versions are legal to Ensure Package Compability

#### 3.8.1. Caret Requirement

- Caret Requirement `^X.X.X`
- Version is legal if the version number is larger and does not modify the left-most non-zero part of `major.minor.patch` group
- E.g.:
    - `^2.2.0`: `>=2.2.0 <3.0.0`
        - Legal: `2.14.0`
        - Illegal: `3.0.0`
    - `^0.1.13`: `>=0.1.13 <0.2.0`
        - Legal: `0.1.14`
        - Illegal: `0.2.0`
    - `^0.0.11`: `>=0.0.11 <0.0.12`
        - Illegal: `0.0.12`

#### 3.8.2. Tilde Requirement

- Tilde Requirement `~X.X.X`
- Version is legal if the version number is larger and does not modify the major( and minor if given) part of `major.minor.patch` group
- E.g.:
    - `~1.2.3`: `>=1.2.3 <1.3.0`
    - `~1.2`: `>=1.2.0 <1.3.0`
    - `~1`: `>=1.0.0 <2.0.0`

#### 3.8.3. Wildcard Requirement

- Wildcard Requirement: `(*|X).(*|X).(*|X)`
- Version is legal if the version number matches the `major.minor.patch` group, where `*` matches any number

#### 3.8.4. Inequality Requirement

- Inequality Requirement: `(<|<=|!=|>=|>) X.X.X`
- Version is legal if the version number meet the inequality

#### 3.8.5. Exact Requirement

- Exact Requirement: `X.X.X`
- Version is legal if the version number is the Exact Required one

#### 3.8.6. Multiple Requirement

- Multiple Requirement: `<Requirement>(, <Requirement)+`
    - Or `["<Requirement>"(, "<Requirement>")+]` in `pyproject.toml`
- Version is legal if the version number meet every requirement

## 4. Usage

- `poetry <Command> [<Options>] [<Arguments>]`
- Command can be called by short name if it is not ambiguous

### 4.1. Global Options

- `--verbose[=VERBOSE]`, `-v`, `-vv`, `-vvv`: Verbose level from 1 to 3, default 1
- `--help`, `-h`: Display help and quit
- `--quiet`, `-q`: No ouput message
- `--ansi`: Force ANSI output
- `--no-ansi`: Disable ANSI output
- `--version`, `-V`: Display version and quit

### 4.2. Help

- `poetry`, `poetry list`: Get complete list of commands
- `poetry --help`, `poetry list --help`: Get help info

### 4.3. Config

- `poetry config [options] [setting-key] [setting-value1] ... [setting-valueN]`
    - `setting-key`: Config option name
    - `setting-value`: Config value
    - Option
        - `--unset`: Remove config option value
        - `--list`: List current config variable
        - `--local`: Repo local config

### 4.4. Virtual Environment

#### 4.4.1. Run

- `poetry run <Command>`: Execute command in virtualenv
- `poetry run <ScriptName>`: Execute script defined in `pyproject.toml` in virtualenv

#### 4.4.2. Shell

- `poetry shell`: Spawn virtual environment shell according to `$SHELL` env varible

### 4.5. Project

#### 4.5.1. Setup

- `poetry new <ProjectName>`: Create new Directory and Project
    - This will create `<ProjectName>` directory with initial project structure
    - Option
        - `--name <PackageName>`: Name package differently than project
        - `--src`: Use a `src` dir over package dir
- `poetry init`: Set `pyproject.toml` interactively
    - `--name`: Name of the package
    - `--description`: Description of the package
    - `--author`: Author of the package
    - `--dependency`: Package to require with a version constraint
        - Format: `<Package>:<VersionConstraints>`
    - `--dev-dependency`: Development requirements

#### 4.5.2. Package

- `poetry build[ OPTIONS]`: Build `sdist` and `wheel` archieve
    - Option
        - `-F FORMAT`, `--format FORMAT`: Limit format to `sdist` or `wheel`

#### 4.5.3. Publish

- `poetry publish( <Options>)?`
    - Option
        - `--build`: Build before publish
        - `(-r|--repository) <RepoName>`: Publish target repo, defualt to PyPI
        - `(-u|--username) <UserName>`
        - `(-p|--password) <Password>`

#### 4.5.4. Check

- `poetry ckeck`: Check `pyproject.toml`

#### 4.5.5. Version

- `poetry version BUMPRULE`: Bump the version of project
    - `BUMPRULE`: `patch`, `minor`, `major`, `prepatch`, `preminor`, `premajor`, `prerelease`

### 4.6. Dependency

#### 4.6.1. Search

- `poetry search[ OPTIONS]( <PackageName>)+`: Search for packages on remote index
    - Option
        - `-N`, `--only-name`: Search only in name

#### 4.6.2. Add

- `poetry add[ OPTIONS]( <PackageName>[ PKGOPTIONS])+`: Add and **Install** dependency
    - Option
        - `(-E <Package>)*`, `--extras "<Package>( <Package>)*"`: Extras to activate for dependency
        - `--dry-run`: Output but not execute operation
    - Package Name: `"name[@]version_constraint"`
    - Package Option
        - `--git <GitRepoURL>`: Specify git url
        - `--path <PackageDir>`: Specify directory of package (with `pyproject.toml`)/`sdist`/`wheel`
            - Will be installed in editable/develop mode, changes in local dir will be reflected directly in env
        - `-D`, `--dev`: Add as dev dependency
        - `--optional`: Add as optional dependency
- Edit `[tool.poetry.dependencies]` of `pyproject.toml`: Add dependency

#### 4.6.3. Install

- `poetry install[ OPTIONS]`: Install defined dependencies in `pyproject.toml` and `poetry.lock`
    - Option
        - `--develop <PackageName>`: Install package in editable/develop mode
        - `--no-dev`: Do not install dev package
        - `(-E <Package>)*`, `--extras "<Package>( <Package>)*"`: Install extra package

#### 4.6.4. Update

- `poetry update[ OPTIONS]`: Update all packages
- `poetry update[ OPTIONS]( <PackageName>)+`: Update package(s)
    - Option
        - `--dry-run`: Output but not execute operations
        - `--no-dev`: Do not install dev dependencies
        - `--lock`: Update lockfile but not install packages

#### 4.6.5. Remove

- `poetry remove[ OPTIONS]( <Package>[ PCKOPTIONS])+`
    - Option
        - `--dry-run`: Output but not execute operations
    - Package Option
        - `-D`, `--dev`: Remove from dev dependencies

#### 4.6.6. Show

- `poetry show[ OPTIONS]`: Show all installed packages
- `poetry show[ OPTIONS] PACKAGE`: Show detail of package
    - Option
        - `--no-dev`: Do not show dev package
        - `--tree`: Show as tree
        - `-l`, `--latest`: Show latest available version
        - `-o`, `--outdated`: Show latest available version of outdated package

#### 4.6.7. Lock

- `poetry lock`: Locks packages specified in `pyproject.toml` without installing
