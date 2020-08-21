# Powershell

1. 使用'|'创建管道
2. 使用Out-File将输出写入到文本文件.使用-Encoding选项修改编码,使用-append续写(默认覆写).
```powershell
echo "run 50 us"|.\mips_tb_isim_beh.exe|Out-File isimouttemp.txt -Encoding utf8
```
3. 使用findstr查找文件中的行并输出,默认为正则表达式查找.
```powershell
findstr "@" isimouttemp.txt|Out-File isimout.txt -Encoding utf8
```
4. 使用rm删除文件
```powershell
rm .\isimouttemp.txt
```
5. 使用pause暂停
```powershell
pause
```
6. 使用compare-object或fc.exe比较文件内容
```powershell
compare-object (get-content .\1.txt) (get-content .\2.txt)|out-file result.txt
fc.exe /a 1.txt 2.txt|out-file result.txt
```
7. 脚本调用其他脚本时,被调脚本中的命令在主调脚本的目录下执行
8. 使用tree生成当前目录的子目录树
9. 使用ls列出文件,使用-name仅返回文件名
10. foreach循环
```powershell
foreach($a in $list)
{
	do something with $a;
}
```
11. 使用echo打印到控制台,使用echo|out-file -append打印到文件
12. 脚本参数数组: `$args`, 调用参数: `$args[<index>]`, 脚本路径之后(不含脚本路径)的第一个字符串是`$args[0]`

## 1. Convension

- Option: `-NAME [VALUE]`, `NAME` can be unambigious prefix

## 2. CLI Usage

- `pwsh -c "& {<Command>}"`
- Option and Value can be refed with unambitious prefix

## 3. Syntax

- Escap: `

## 4. 管道与重定向

- `|`: 管道
- 输出 STDOUT
    - `>`: 覆盖
    - `>>`: 追加
- 错误 STDERR
    - `2>`: 覆盖
    - `2>>`: 追加
- 全部输出
    - `*>`, `*>>`
    - `1>log.txt 2>&1`
- 指定编码

```pwsh
command 2>&1 | Out-File $mylog -Append -Encoding UTF8
```

```pwsh
Start-Process -File myjob.bat -RedirectStandardOutput $stdOutLog -RedirectStandardError $stdErrLog -wait
Get-Content $stdErrLog, $stdOutLog | Out-File $myLog -Append -Encoding UTF8
```

- 关闭: `&-`

## 5. Job

- `Job`: List jobs
- `Remove-Job ID|* [-Force]`: Remove job
    - `-Force`

## 6. Cmdlet

### 6.1. Help

```powershell
Get-Help SUBJECT [-Online]
```

### 6.2. Get-Command

- `Get-Command NAME`: Locate Command, equivalent of POSIX `which`
- `Get-Command NAME -All`: Locate all

### 6.3. Get-ChildItem

- `Get-ChildItem [[-Path] PATH] [OPTION]`
- Path: Globable
- Option
    - `-Recurse`
    - `-Name`: Name Only
    - `-Exclude FILENAME_PATTERN`
    - `-Depth NUM`
    - `-Force`: Include Hidden
    - `-Attributes {ReadOnly | Hidden | System | Directory | Archive | Device | Normal | Temporary | SparseFile | ReparsePoint | Compressed | Offline | NotContentIndexed | Encrypted | IntegrityStream | NoScrubData}`: AND Logic
        - `-Directory`
        - `-File`
        - `-Hidden`
        - `-ReadOnly`
        - `-System`
- Mode
    - `l`: Link
    - `d`: Dir
    - `a`: Archive
    - `r`: Read-Only
    - `h`: Hidden
    - `s`: System

### 6.4. File

#### 6.4.1. Out-File

`Out-File [-FilePath] PATH [[-Encoding] ENCODING] [-Append] [-Force] [-InputObject PSOBJ]`

- `ENCODING`: `UTF8`

#### 6.4.2. Get-Content

`Get-Content|cat [-Path] PATH [-Encoding ENCODING]`

### 6.5. Service

- `Set-Service [-Name] NAME [-DisplayName DISPLAYNAME] [-Description DESCRIPTION] [-StartupType Automatic|AutomaticDelayedStart|Disabled|Manual] [-Status Paused|Running|Stopped]`: Set name, displayname, description, startup type or status of a service
- `Restart-Service NAME`
- `Start-Service NAME`
- `Stop-Service NAME`

### 6.6. Net Work

- `netstat -ano -p tcp | grep -P "PORT"`: Check PID that uses PORT

### 6.7. Process

- `Get-Process -Id ID`
- `Stop-Process|kill -Id ID`

### 6.8. FireWall Rule

- `Show-NetFirewallRule | Out-File rule.txt -Encoding UTF8`

```powershell
Set-NetFirewallRule \
[-Name] NAME \
[-NewDisplayName 'DISPLAY_NAME'] \
[-Desription 'DESC'] \
[-Enabled True|False] \
[-Direction Inbound|Outbound] \
[-Action Allow|Block] \
[-Owner OWNER] \
[-Protocol TCP|UDP|Any] \
[-LocalAddress LOCAL_ADDR] \
[-RemoteAddress REMOTE_ADDR] \
[-LocalPort NUM] \
[-RemotePort NUM] \
[-Program "PATH"] \
[-Package PACKAGE] \
[-Service NAME] \
[-LocalUser USER] \
[-RemoteUser USER] \
[-Confrm]

New-NetFirewallRule \
[-DisplayName] 'DISPLAY_NAME' \
[-Name NAME] \
[-Desription 'DESC'] \
[-Enabled True|False] \
[-Direction Inbound|Outbound] \
[-Action Allow|Block] \
[-Owner OWNER] \
[-Protocol TCP|UDP|Any] \
[-LocalAddress LOCAL_ADDR] \
[-RemoteAddress REMOTE_ADDR] \
[-LocalPort NUM] \
[-RemotePort NUM] \
[-Program "PATH"] \
[-Package PACKAGE] \
[-Service NAME] \
[-LocalUser USER] \
[-RemoteUser USER] \
[-Confrm]
```

### 6.9. Registry

```powershell
New-ItemProperty \
[-Path] "PATH" \
[-Name] NAME \
[-PropertyType String|DWORD] \
[-Value "VALUE"] \
[-Force]
```

- Path: Registry Database is a Dedicated Drive with following drive mark
    - `HKLM:` for `HKEY_LOCAL_MACHINE`

### 6.10. File

#### 6.10.1. Link

```powershell
New-Item \
-ItemType SymbolicLink \
-Path 'PATH' \
-Target 'TGT_PATH'
```

### 6.11. findstr

- `findstr "<Regex>" <File>`
- PCRE

### 6.12. ForFiles

- `/P <Path>`
    - 开始搜索的路径
    - 默认为`.`
- `/M <Regex>`
    - 文件搜索正则
- `/S`
    - 递归搜索
- `/C "<Command>"`
    - 为每个文件执行的命令
    - 可用变量
        - `@file`: 文件名
        - `@fname`: 文件主名 (无扩展名)
        - `@ext`: 文件扩展名
        - `@path`: 文件绝对路径
        - `@relpath`: 文件相对路径
        - `@isdir`: 是否是目录, 返回"TRUE"或"FALSE"

## 7. Env Var

- Access: `$env:VAR`
