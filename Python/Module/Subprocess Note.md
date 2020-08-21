# Subprocess Note

```python3
from subprocess import Popen, PIPE, run, TimeoutExpired, CalledProcessError

# Run command
try:
    finishedpcs = run(r'command'|[r'command'(, r'argument')*][, stdin=PIPE|file_obj][, input=inputstr][, stdout=PIPE|file_obj][, stderr=PIPE|file_obj], capture_output=True[, shell=True][, timeout=seconds][, check=True][, cwd=r'targetdir'], text=True)  # Run command and wait for it to complete
except TimeoutExpired:
    # Timeout Handler
except CalledProcessError:
    # Error Handler
finishedpcs.stdout
finishedpcs.stderr

# Subprocess
# Start
with Popen(r'command'|[r'command'(, r'argument')*][, stdin=PIPE|file_obj][, stdout=PIPE|file_obj][, stderr=PIPE|file_obj][, shell=True][, cwd=r'targetdir'], text=True) as subpcs:  # Run command but not wait for it to complete

    # Communicate
    subpcs.stdin.write('input content\n')
    subpcs.stdin.flush()

    subpcs.stdout.readline()  # Read till '\n'
    subpcs.stdout.readlines()  # Read till EOF

    subpcs.stderr.readline()  # Read till '\n'
    subpcs.stderr.readlines()  # Read till EOF

# Manually Start
subpcs = Popen(r'command'|[r'command'(, r'argument')*][, stdin=PIPE|file_obj][, stdout=PIPE|file_obj][, stderr=PIPE|file_obj][, shell=True][, cwd=r'targetdir'], text=True)  # Run command but not wait for it to complete

# Manually Exit
try:
    output_str, error_str = subpcs.communicate(input=inputstr, timeout=seconds)  # 发送数据, 关闭管道并等待子进程结束
except TimeoutExpires:
    subpcs.kill()
    output_str, error_str = subpcs.communicate()
```

- 主要用于创建 (非交互性) 子进程, 执行外部命令和程序, 类似于Shell
- 可以使用管道进行进程间文本通讯
    - 通过向构造器 (如`Popen()`) 传入特殊值 (标志) `subprocess.PIPE`, 可为子进程的`stdin`, `stdout`, `stderr`新建独立的管道
    - 为子进程创建的管道可通过`subpcs.stdin`, `subpcs.stdout`, `subpcs.stderr`访问
    - 为子进程创建的管道与文件管道类似, 可通过`write()`, `read()`, `readline()`, `readlines()`读写
    - 注意: `subprocess.PIPE`本身并不是一个管道, 不可读写, 仅仅只是用于代表 新建管道 操作的枚举常量. 每个被传入`subprocess.PIPE`的输入/输出并没有与所谓的`subprocess.PIPE`"管道"关联, 也没有共享同一个`subprocess.PIPE`"管道", 而是各自与为自己新建的, 独立的, 以自己的名称命名的管道相关联
    - 注意: 与文件管道不同, 某一时间点进程通讯管道中可能尚未含有`EOF`标记, 这可能导致以`EOF`作为读取结束标记的方法 (如`read()`, `readlines()`) 持续等待, 甚至导致死锁. 通过为方法设置等待时限并捕获超时时方法抛出的`TimeoutExpired`异常来避免持续等待与死锁
    - 注意: 不推荐使用`subpcs.stdin.write()`, `subpcs.stdout.read()`, `subpcs.stderr.read()`. 若子进程向缓冲区写入过多内容, 可能导致子进程阻塞与死锁. 应考虑使用`subpcs.communicate()`代替
- 推荐的调用子进程的方式是在任何受支持的用例中使用`run()`函数. 对于更进阶的用例,也可以使用底层的`Popen`接口.
- 使用时应注意
    - 创建子进程后, 父进程是否阻塞以等待子进程结束
    - 函数的返回内容
    - 子进程返回值非0时, 父进程应如何处理
- 可通过`subprocess.Popen(..., bufsize = 0)`来关闭缓冲区, 实现实时通讯

## 1. Frequently Used Arguments
- 在本模块`Popen`类的构造器及其他模块方法中通用的参数
- 若非需要, 留为默认值即可
- `args`
    - 用于调用子进程的命令
    - 可以是字符串或字符串序列
        - 推荐使用字符串序列而非字符串, 这可以避免不当参数转义或文件路径空格带来的错误
        - 若传入一个必须使用Shell解析的字符串 (如同时包含命令与参数的字符串), 则必须设置`shell = True`, 以使用Shell解析命令
- `stdin`, `stdout`, `stderr`
    - 指定子进程标准输入, 输出, 错误的文件句柄
    - 合法值
        - `subprocess.PIPE`: 新建一个独立的对应管道
        - `subprocess.DEVNULL`: 使用特殊文件`os.devnull`
        - `subprocess.STDOUT` (仅对应`stderr`参数): 将子进程的标准错误合并至其标准输出
        - 现存的文件描述符 (正整数)
        - 现存的文件对象
        - `None` (默认): 不进行重定向, 子进程的流继承自父进程
- 若`encoding`或`errors`被指定, 或`text`被设置为`True`, 则`stdin`, `stdout`, `stderr`将通过指定的`encoding`和`errors`, 或`io.TextIOWrapper` (默认) 以文本模式打开; 否则, 将以二进制模式打开
    - 若以文本模式打开, 且`newline`参数设置为`None` (默认)
        - `stdin`: 输入中的换行符'\n'将被转换为默认换行符`os.linesep`
        - `stdout`, `stderr`: 输出中的换行符将被转换为'\n'
    - 若以二进制模式打开, 则没有编码, 解码与换行符转换
- `shell`
    - 指定是否使用shell解析`args`
    - 若设置为`True`, 则使用shell解析`args`

## 2. Exception
- 此模块中的异常都继承自`SubprocessError`
- 在子进程中抛出的异常, 将在下一次创建新子进程时在父进程中抛出
    - 常见异常
        - `OSError`: 文件不存在
        - `ValueError`: 参数无效
- `check_all()`与`check_output()`方法在子进程返回值非零时抛出`CalledProcessError`异常
- 所有接受`timeout`参数的方法 (如`call()`, `Popen.communicate()`) 将在超时时抛出`TimeoutExpierd`异常

## 3. Module
### 3.1. Method
- subprocess.run()
    - Recommended
    - `subprocess.run(args, *, stdin=None, input=None, stdout=None, stderr=None, capture_output=False, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None, universal_newlines=None)`
    - 运行`args`描述的指令, 等待指令**完成**, 然后返回一个`CompletedProcess`实例
    - 参数
        - `capture_output`
            - 若为`True`, 则通过将`Popen`的`stdout`与`stderr`参数设置为`subprocess.PIPE`标志而为子进程的`stdout`与`stderr`新建独立的管道, 从而捕获子进程的输出
                - 子进程的`stdout`与`stderr`管道可通过`subpcs.stdout`与`subpcs.stdin`访问
                - 子进程管道与文件管道类似, 可通过`write()`, `read()`, `readline()`, `readlines()`读写
                - 注意: 与文件管道不同, 子进程管道在某一时间点可能尚未含有`EOF`标志, 故应设置超时时限, 以免持续等待甚至死锁
                - 注意: `subprocess.PIPE`本身并不是一个管道, 不可读写, 仅仅只是用于代表 新建管道 操作的枚举常量. 每个被传入`subprocess.PIPE`的输入/输出并没有与所谓的`subprocess.PIPE`"管道"关联, 也没有共享同一个`subprocess.PIPE`"管道", 而是各自与为自己新建的, 独立的, 以自己的名称命名的管道相关联
            - `stdout`与`stderr`参数不应与`capture_output`参数同时传入
        - `timeout`
            - 将被传递给`Popen.communicate()`
            - 若超时
                - 杀死 (`subpcs.kill()`) 子进程并等待其结束
                - 抛出`TimeoutExpired`异常
        - `input`
            - 若设置为非`None`值
                - 创建内部`Popen`对象, 并设置`stdin = subprocess.PIPE`
                - 内容将被传递给`Popen.communicate()`以及子进程的`stdin`
            - 若指定了`encoding`或`errors`, 或`text = True`, 则可以传入字符串或字节串; 否则, 只能传入字节串
            - `stdin`参数不应与`input`参数同时传入
        - `check`
            - 若设置为`True`, 则检查子进程返回值
                - 若子进程返回值非零, 抛出`CalledProcessError`异常, 异常中将含有调用参数, 返回值, `stdout`, `stderr`的内容 (若捕获)
        - 若`encoding`或`errors`被指定, 或`text`被设置为`True`, 则`stdin`, `stdout`, `stderr`将通过指定的`encoding`和`errors`, 或`io.TextIOWrapper` (默认) 以文本模式打开; 否则, 将以二进制模式打开
        - `env`
            - 将被传递给`Popen`
            - `None` (默认): 子进程继承父进程的环境变量
            - 非`None`: 一个设置子进程环境变量的映射
- subprocess.call()
    - 父进程阻塞以等待子进程结束
    - 返回返回值
    - 被`run`取代
- subprocess.check_call()
    - 父进程阻塞以等待子进程结束
    - 检查子进程返回值, 若返回值为0, 则返回0; 否则举出subprocess.CalledProcessError错误
        - 可通过try-except块处理
- subprocess.check_output()
    - 父进程阻塞以等待子进程结束
    - 返回子进程的标准输出内容
    - 检查子进程返回值, 若返回值非0, 则举出subprocess.CalledProcessError错误
        - 可通过try-except块处理

```python
import subprocess
completedProcess = subprocess.run(["<Command>"[, "<Argument>"]...)
completedProcess.returncode # Get return code
completedProcess.stdout # Get stdout
completedProcess.stderr # Get stderr
return_code = subprocess.call(["<Command>"[, "<Argument>"]...)
return_code = subprocess.call("<ShellCommand>, shell = True) # Call through shell
```

### 3.2. Field
#### 3.2.1. Enum Const

- `DEVNULL`
    - 用于提供给子进程构造方法, 作为指定输入输出使用特殊文件`os.devnull`的标记
- `subprocess.PIPE`
    - 提供给子进程构造方法, 作为新建标准流管道的标记
    - 提供该标记的输入/输出, 将与为其新建的独立管道相关联. 该管道可通过`subpcs.stdin`, `subpcs.stdout`, `subpcs.stderr`访问
    - 注意: `subprocess.PIPE`本身并不是一个管道, 不可读写, 仅仅只是用于代表 新建管道 操作的枚举常量. 每个被传入`subprocess.PIPE`的输入/输出并没有与所谓的`subprocess.PIPE`"管道"关联, 也没有共享同一个`subprocess.PIPE`"管道", 而是各自与为自己新建的, 独立的, 以自己的名称命名的管道相关联
    - 常用于`Popen.communicate()`
- `STDOUT`
    - 提供给子进程构造方法, 作为 与子进程的标准输出使用同一管道 的标记
    - 常通过`Popen(..., stderr = STDOUT, ...)`来合并子进程的标准输出流与标准错误流

## 4. Class

### 4.1. SubprocessError
- 此模块其他异常的超类

### 4.2. TimeoutExpired
- 等待子进程的过程超时时抛出
- `SubprocessError`的子类

#### 4.2.1. Method

#### 4.2.2. Field
- `cmd`
    - 用于创建子进程的指令
- `timeout`
    - 超时秒数
- `output`, `stdout`
    - 子进程的标准输出内容 (若捕获)
- `stderr`
    - 子进程的标准错误内容 (若捕获)

### 4.3. CalledProcessError
- 对子进程执行`check_call()`或`check_output()`, 且子进程退出状态码非零时抛出

#### 4.3.1. Method
#### 4.3.2. Field
- `returncode`
    - 子进程退出状态码
    - 在POSIX中, 负值`-n`表示子进程被信号`n`中断
- `cmd`
    - 用于创建子进程的指令
- `timeout`
    - 超时秒数
- `output`, `stdout`
    - 子进程的标准输出内容 (若捕获)
- `stderr`
    - 子进程的标准错误内容 (若捕获)


### 4.4. CompletedProcess
- 代表运行结束的进程, `subprocess.run()`的返回值类型

#### 4.4.1. Method
- `check_returncode()`
    - 若`returncode`非零, 则抛出`CalledProcessError`异常

#### 4.4.2. Field
- `args`
    - 用于启动进程的参数
    - 可能是字符串或字符串列表
- `returncode`
    - 子进程的退出状态码
    - 0代表子进程成功运行完成
    - 在POSIX中, 负值`-n`表示子进程被信号`n`中断
- `stdout`, `stderr`
    - 从子进程捕获到的标准输出, 标准错误内容
    - 一个字节串或字符串
    - 若未进行标准输出, 标准错误捕获, 则为`None`

### 4.5. Popen
- Process Open 类, 代表子进程
- `subprocess`模块的底层进程类, 用于创建与管理子进程
- 非常灵活, 可用于处理模块方法无法处理的特殊用例
- 父进程与子进程并行执行
    - 可通过`Popen.wait()`方法, 使父进程阻塞以等待子进程结束
- 可通过`subprocess.Popen(..., bufsize = 0)`来关闭缓冲区, 实现实时通讯

```python
import subprocess

# Create
subpcs = subprocess.Popen(r"<Command>", stdin = subprocess.PIPE, stdout = subprocess.PIPE, cwd = "<ChangeWorkingDirectory>")
subpcs = subprocess.Popen(["<Command>"[, "<Argument>"]...]) # Open with arguments

# Usage
output = subpcs.communicate("<Input>") # Input and close subpcs.stdin, block until subprocess end and return it's output
subpcs.wait() # Block and wait
subpcs.poll() # Check status
subpcs.kill()
subpcs.send_signal() # Send signal
subpcs.terminate()
subpcs.pid # Get PID
subpcs.stdin # Get subprocess's stdin stream if set to subprocess.PIPE
subpcs.stdout # Get subprocess's stdout stream if set to subprocess.PIPE
subpcs.stderr # Get subprocess's stderr stream if set to subprocess.PIPE
inputBytes = inputStr.encode("<Coding>")
outputBytes = subpcs.communicate(inputBytes)[0] # subprocess.communicate(<InputBytes>) returns a 2-elements list: [<OutputBytes>, <ErrorBytes>]
outputStr = outputBytes.decode("<Coding>")

# Pipe Line
subpcs1 = subprocess.Popen("<Command>", stdout = subprocess.PIPE)
subpcs2 = sbuprocess.Popen("<Command>", stdin = subpcs1.stdout, stdout = subprocess>PIPE)
output = subpcs2.communicate("<Input>")
```
#### 4.5.1. Method
- `Popen()`构造器
    - `Popen(args, bufsize=-1, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=True, shell=False, cwd=None, env=None, universal_newlines=None, startupinfo=None, creationflags=0, restore_signals=True, start_new_session=False, pass_fds=(), *, encoding=None, errors=None, text=None)`
    - 新建并 (并行) 执行子进程
        - 在POSIX下, 此方法使用类似`os.execvp()`的方式执行子进程
        - 在Windows下, 此方法调用了`CreateProcess()`方法
    - 参数
        - `args`
            - 执行子进程的命令字符串或字符串序列
            - 若提供字符串序列 (推荐), 则运行程序由第一项字符串指定
                - 可使用`args = shlex.split(arg_line)`分隔命令字符串为命令字符串序列
                - 在Windows下, 字符串序列将通过特定方式转换为单一字符串, 这是因为`CreateProcess()`方法仅接受单一字符串
            - 若提供字符串
                - 在POSIX下, 字符串被解释为欲执行程序的路径
                    - 不能使用单个字符串同时提供程序路径与参数
        - `shell`
            - 指定是否使用shell解析`args`
            - 若指定`shell = True`
                - 推荐为`args`传递单一字符串而非字符串序列
                - 在POSIX下
                    - 默认shell为`/bin/sh`
                    - 若`args`为字符串, 则此字符串将作为命令由shell解析执行
                    - 若`args`为字符串序列, 则仅第一项作为命令由shell解析执行, 其余的字符串将作为传递给shell的参数 (而非命令)
                    - 即: `Popen(args, ..., shell = True, ...)`等价于`Popen(['/bin/sh', '-c', args[0], args[1], ...])`
                - 在Windows下
                    - 默认shell由环境变量`COMSPEC`指定
                    - 仅在需要执行内置shell命令 (如`dir`, `copy`等) 时设置`shell = True`
                    - 当执行批处理文件或基于控制台的可执行文件时, 不需要设置`shell = True`
                - 可使用`shlex.quote()`转义命令中的空白字符与shell元字符
        - `bufsize`
            - 由`open()`函数用于为`stdin`, `stdout`, `stderr`新建管道文件对象
            - 0
                - 不使用缓冲区
                - 读取与写入是一个系统调用, 返回short型值
            - 1
                - 行缓冲
                - 仅在文本模式中可用
            - 其余正值
                - 使用对应大小的缓冲区
            - 负值 (默认)
                - 使用系统默认`io.DEFAULT_BUFFER_SIZE`大小的缓冲区
        - `executable`
            - 指定替代的执行程序
            - 若`shell = False`
                - 执行`executable`参数所指定的程序, 并将`args`作为参数传递给`executable`所指定的程序
            - 若`shell = True`
                - 在POSIX下, 使用`executalbe`参数所指定的程序作为shell, 而非默认的`/bin/sh`
        - `stdin`, `stdout`, `stderr`
            - 指定子进程的标准输入, 标准输出, 标准错误的文件句柄
            - 合法值
                - `PIPE`: 新建一个独立的对应的管道
                - `DEVNULL`: 使用特殊的`os.devnull`文件
                - (仅针对`stderr`参数) `STDOUT`: 将标准错误合并至标准输出管道
                - 现存的文件描述符 (正整数)
                - 现存的文件对象
                - 默认: `None`: 不进行重定向, 继承父进程的管道句柄
        - `preexec_fn`
            - 在POSIX下, 若设为可调用对象, 则在刚创建子进程时调用该对象
            - 在多线程时是不安全的, 可能造成死锁, 谨慎使用
            - 使用`env`而非`preexec_fn`来修改子进程环境变量
            - 使用`start_new_session`而非由`preexec_fn`调用`os.setsid()`
        - `close_fds`
            - 若为`True`
                - 在POSIX下, 除0, 1, 2之外的所有文件描述符都将在子进程执行前关闭
                - 在Windows下, 子进程不会继承任何句柄, 除非在`STARTUPINFO.IpAttributeList`的`handle_list`的键中显式传递, 或通过标准句柄重定向传递
        - `pass_fds`
            - 在POSIX下, `pass_fds`中的文件描述符将在父子进程间保持打开
            - 若提供`pass_fds`, 则`close_fds`强制为`True`
        - `cwd`
            - 若不为`None`, 则在执行子进程之间改变工作目录为`cwd`
            - 可以是`str`或`path-like`对象
            - 注意: 当执行文件的目录是相对目录时, 将先改变工作目录, 再基于`cwd`的目录寻找执行文件
        - `restore_signals`
            - `True` (默认): 在POSIX下, 由Python设置为`SIG_IGN`的所有信号将在执行子进程前为子进程恢复为`SIG_DFL`
        - `start_new_session`
            - `True`: 在POSIX下, 在执行子进程前调用`os.setsid()`
        - `env`
            - 若非`None`, 则应是一个定义子进程环境变量的映射, 子进程将使用定义的环境变量而非继承父进程的环境变量
            - 注意: 在Windows下, 必须为子进程提供`SystemRoot`环境变量
        - 若`encoding`或`errors`被指定, 或`text`为`True`, 则`stdin`, `stdout`, `stderr`以指定的`encoding`和`errors`, 或`io.TextIOWrapper` (默认) 以文本模式打开; 否则, 将以二进制模式打开
        - `startupinfo`
            - 传递给底层`CreateProcess()`的`STARTUPINFO`对象
        - `creationflags`
            - 有效值
                - `CREATE_NEW_CONSOLE`
                - `CREATE_NEW_PROCESS_GROUP`
                - `ABOVE_NORMAL_PRIORITY_CLASS`
                - `BELOW_NORMAL_PRIORITY_CLASS`
                - `HIGH_PRIORITY_CLASS`
                - `IDLE_PRIORITY_CLASS`
                - `NORMAL_PRIORITY_CLASS`
                - `REALTIME_PRIORITY_CLASS`
                - `CREATE_NO_WINDOW`
                - `DETACHED_PROCESS`
                - `CREATE_DEFAULT_ERROR_MODE`
                - `CREATE_BREAKAWAY_FROM_JOB`
    - 支持通过`with`语句使用上下文管理器, 在退出时将关闭文件描述符并等待子进程结束

```python
with Popen(...) as subpcs:
    # Do something
```

- `poll()`
    - 检查子进程是否已终止
        - 若子进程已终止, 设置并返回`returncode`属性
        - 否则, 返回`None`
- `wait([timeout = <TimeoutSecond>])`
    - 等待子进程终止, 并返回`returncode`属性
    - 若超时, 则抛出`TimeoutExpires`异常
    - 当子进程的输出占满了管道缓冲区时, 子进程会阻塞以等待缓冲区释放空间. 此时调用`wait()`将造成死锁 (互相等待)
        - 使用`communicate()`代替`wait()`以避免死锁
    - 本方法基于轮询而非阻塞实现等待. 使用`asyncio`以进行阻塞等待
- `communicate(input = None, timeout = None)`
    - 与子进程进行单次交互, 并关闭子进程
    - 向`stdin`写入数据, 关闭`stdin` (发送`EOF`), 再从`stdout`与`stderr`读取数据直到`EOF`, 接着等待子进程结束
    - 返回`(stdout_data, stderr_data)`元组
    - 若以文本模式打开, 输入输出均是字符串; 否则, 均是字节串
    - 注意
        - 读写子进程流的前提是该流创建为`PIPE`模式, 否则读写内容均为`None`
        - 当心子进程输出过多导致内存溢出. 考虑通过直接写入文件来处理大型输出
        - 不能多次`communicate()`, 因为`communicate()`会关闭`stdin`管道
    - 若超时, 则抛出`TimeoutExpires`异常, 但不会杀死子进程. 捕获异常后可以选择杀死或重新等待
    - 常用结构:

```python
subpcs = Popen(...)
try:
    out, err = subpcs.communicate(timeout = 15)
except TimeoutExpires:
    subpcs.kill()
    out, err = subpcs.communicate()
```

- `send_signal(signal)`
    - 向子进程发送信号`signal`
    - 在Windows下
        - `SIGTERM`是`terminate()`的一个别名
        - `CTRL_C_EVENT`, `CTRL_BREAK_EVENT`可以被发送给 以 含有`CREATE_NEW_PROCESS_GROUP`的 `creationflags`参数 创建的 子进程
- `terminate()`
    - 终止子进程
    - 在POSIX下, 发送`SIGTERM`
    - 在Windows下, 调用Win32API中的`TerminateProcess()`
- `kill()`
    - 杀死子进程
    - 在POSIX下, 发送`SIGKILL`
    - 在Windows下, `kill()`是`terminate()`的别名

#### 4.5.2. Field
- `args`
    - 用于创建子进程的命令字符串 (序列)
- `stdin`
    - 若创建时指定为`PIPE`
        - 一个可写流对象. 取决于其他创建参数, 可能是字符流或字节流
    - 否则, 为`None`
    - 注意: 当心由于缓冲区满而导致的`stdin.write()`死锁, 可使用`communicate()`规避
- `stdout`, `stderr`
    - 若创建时指定为`PIPE`
        - 一个可读流对象. 取决于其他创建参数, 可能是字符流或字节流
    - 否则, 为`None`
    - 注意: 当心由于缓冲区满而导致的`stdout.read()`, `stderr.read()`死锁, 可使用`communicate()`规避
- `pid`
    - 子进程的`pid`
    - 注意: 若创建时设置了`shell = True`, 则为子shell的`pid`
- `returncode`
    - 子进程的退出码, 由`poll()`, `wait()`和`communicate()`设置
    - 若子进程尚未结束, 则为`None`
    - 在POSIX下, 负值`-n`表示子进程被信号`n`中断
