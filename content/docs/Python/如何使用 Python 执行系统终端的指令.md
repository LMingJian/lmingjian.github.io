---
title: 如何使用 Python 执行系统终端的指令
date: 2024-12-25T16:10:20+08:00
author: LiangMingJian
---

# 前言

在 Python 开发过程中，往往会遇到这样的需求：调用安装在系统上的第三方软件执行某些命令，或者调用系统上的某些命令来获取信息。此时，如何通过 Python 调用这些命令就是一个问题（这个需求简单的描述就是在 Python 中执行 Windows cmd 或 Linux shell 中的命令）。

在 Python 中，可以使用内置模块 subprocess 用来创建一个子进程，然后通过这个子进程来执行系统上的第三方命令并获取输出结果。

# subprocess.run 方法

在使用 subprocess 模块时，推荐直接使用 run 方法来执行终端命令，只有当需要更复杂控制时才使用底层的 Popen 对象。

**subprocess.run 的基本使用方法是**：

```python
import subprocess  

# 以字符串作为命令传入
result1 = subprocess.run("ls -l")  

# 以列表作为命令传入
result2 = subprocess.run(["ls", "-l"])  

# 上述执行后会在 Python 控制台打印输出结果
"""
total 86
drwxr-xr-x 1 --- ---     0 Sep  4 16:15 ---
drwxr-xr-x 1 --- ---     0 Sep  4 16:15 ---
......
"""

# 同时返回的 result 是一个 CompletedProcess 对象
print(result1)
# CompletedProcess(args='ls -l', returncode=0)
```

注意到，subprocess.run 是有返回值的。subprocess.run 会返回一个 CompletedProcess 对象，这个对象代表一个进程已经结束。

**CompletedProcess 对象包含着以下属性**，可通过点式进行访问：

- `args`：字符串，子进程执行的命令。
- `returncode`：子进程的退出代码，0 表示正常退出。
- `stdout`：子进程捕获的标准输出，默认为 None，需要在 run 进行设置后才会有值。
- `stderr`：子进程捕获的标准错误，默认为 None，同样需要在 run 进行设置后才有。
- `check_returncode()`：检查 returncode 是否为 0，不为 0 则抛出异常。

特别的，subprocess.run 方法支持传入一些参数用以提供更完善的控制。

**常用的 subprocess.run 可选参数有**：

- `capture_output`：捕获 stdout 和 stderr 到返回的 CompletedProcess 对象中。特别注意，**在设置 capture_output 捕获后，命令的输出不会出现在 Python 的控制台，而是在返回的对象中，需要开发者手动进行读取**。

> 要想在执行命令时，不让命令输出出现在控制台，则应当将 `capture_output=True`。即 `result = subprocess.run(["ls", "-l"], capture_output=True)  ` 。

- `stdout` 和 `stderr`：结果输出和错误输出的位置，默认是 None，继承父进程的标准输出，即输出到 Python 控制台。**特别的，该选项不能与 capture_output 共用。**

> stdout 的输出可以选择：subprocess.PIPE（subprocess 对象），`open('output.txt', 'w')`（文件对象），subprocess.DEVNULL（丢弃输出）。

> 对于 stderr，可以直接将其输出到 subprocess.STDOUT，与结果输出合并，也可以像上面 stdout 的输出一样，单独配置。

- `timeout`：超时设置，以秒为单位。
- `encoding`：编码设置，用以将输入输出的按指定编码格式转换为字符串。
- `error`：编码异常处理，当 encoding 编解码异常时，采用 `strict`（默认，抛出异常），`ignore`（忽略），`replace`（替换）这 3 种方式处理。
- `text`：使用本地默认编码将输入输出转换为字符串，与 encoding 共用时，以 encoding 的设置为准。

# subprocess.Popen 对象

Popen 对象是 run 方法的底层，也是 subprocess 底层的进程创建和管理工具，具有比 run 方法更大的灵活性，但也更为复杂，通常不建议使用。

**subprocess.Popen 对象的构造方法是**：

```python
import subprocess  

# 以字符串作为命令传入
p1 = subprocess.Popen("ls -l")  

# 以列表作为命令传入
p2 = subprocess.Popen(['ls','-l'])  

# 上述执行后会在 Python 控制台打印输出结果
"""
total 86
drwxr-xr-x 1 --- ---     0 Sep  4 16:15 ---
drwxr-xr-x 1 --- ---     0 Sep  4 16:15 ---
......
"""

# 返回一个 Popen 对象
print(p1)
# <Popen: returncode: None args: 'ls -l'>
```

与 run 方法很类似，这是因为 Popen 对象是 run 方法底层的关系，**所以在  Popen 对象构建过程中，支持传入与 run 方法相似的一些参数来进行配置**：

- `stdin`：标准输入。
- `stdout`：标准输出，支持设置为 subprocess.PIPE（subprocess 对象），`open('output.txt', 'w')`（文件对象），subprocess.DEVNULL（丢弃输出）。
- `stderr`：标准错误输出。
- `encoding`：输出输入编码。
- `errors`：输出输入编码错误处理。

> 要想在执行命令时，不让命令输出出现在控制台，则应当将 `stdout=subprocess.PIPE`。即 `p = subprocess.Popen(['ls','-l'], stdout=subprocess.PIPE)` 。

**与 run 方法不同，Popen 对象的特点是支持异步操作，程序可以通过 `wait()` 和 `communicate()` 进行等待和通信，而不是像 run 一样调用后直接执行，控制更为灵活。**

**比如下述代码在处理好数据后，再通过通信来调用命令：**

```python
import subprocess  
  
# 启动子进程
proc = subprocess.Popen(  
    ['openssl', 'sha256'],  
    stdin=subprocess.PIPE,  
    stdout=subprocess.PIPE,  
    stderr=subprocess.PIPE,  
    text=True  
)  
  
# 发送数据并获取输出  
input_data = "Hello"  
stdout_data, stderr_data = proc.communicate(input=input_data)  
  
# 检查结果  
if proc.returncode == 0:  
    print(stdout_data)  
    # SHA2-256(stdin)=185f8...
else:  
    print(stderr_data)
```

**或者在执行一些长时间命令时，定时关闭：**

```python
import subprocess  

# 启动子进程
proc = subprocess.Popen(  
    ['ping', '127.0.0.1', '-t'],
    stdout=subprocess.PIPE,  
    stderr=subprocess.PIPE,  
    text=True,   
)  

# 读取前 10 行输出
try:  
    for _ in range(10):    
        line = proc.stdout.readline()  
        if not line:  
            break  
        print(line)  
finally:  
    # 终止进程
    proc.terminate()  
```

Popen 对象支持的常用属性和方法有：

- `Popen.args`：最终执行的命令字符串。
- `Popen.returncode`：子进程的返回码。为 0 则表示正常结束，None 或其他值表示进程尚未终结。
- `Popen.pid`：子进程 ID。
- `Popen.stdin`，`Popen.stdout`，`Popen.stderr`：标准输入，输出，错误流。这是一个类似 `open()` 方法打开的文件对象，因此支持 `read()`，`readline()` 这些文件读取方法。
- `Popen.wait(timeout=None)`：等待子进程被关闭，支持设置超时时间。
- `Popen.communicate(input=None, timeout=None)`：给子进程 stdin 发送数据，并从 stdout，stderr 读取数据，因此该方法会返回两个结果。同样支持设置超时时间。
- `Popen.terminate()`：停止子进程，通过发送停止信号安全的结束。
- `Popen.kill()`：杀死子进程，通过发送 kill 信息强制的结束。

# 扩展阅读：隐藏 Windows 的控制台

在 Windows 系统上，打包的 Python 程序在使用 subprocess 模块执行其系统指令时会额外打开一个 Windows cmd 控制台，这样程序会很难看，如何隐藏这个控制台？

在 subprocess 模块中，支持通过配置 creationflags 标志来达到隐藏控制台的功能。

creationflags 是用来控制 Windows 平台下子进程的创建方式的一个参数。在 run 方法和 Popen 对象构建时进行传入。

creationflags 常用配置项有：

- `subprocess.CREATE_NEW_CONSOLE`：创建一个新的控制台。
- `subprocess.CREATE_NO_WINDOW`：不创建窗口，静默运行（**需要配合 `stdout/stderr` 捕获使用，不然输出会丢失**）。
- `subprocess.DETACHED_PROCESS`：创建一个独立的控制台。

使用代码如下：

```python
import subprocess  
  
# run 方法，配合 capture_output 捕获数据
result = subprocess.run(["ls", "-l"], capture_output=True,
						creationflags=subprocess.CREATE_NO_WINDOW)  
print(result)

# Popen 对象，重定向 stdout 输出捕获数据
p = subprocess.Popen(["ls", "-l"], stdout=subprocess.PIPE,
					 creationflags=subprocess.CREATE_NO_WINDOW)  
print(p.stdout.read())
```

————————————

> [ subprocess --- 子进程管理 ](https://docs.python.org/zh-cn/3.13/library/subprocess.html)
