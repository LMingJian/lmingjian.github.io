---
title: Python 基于进程的并行 multiprocessing
date: 2025-08-20T09:12:12+08:00
author: LiangMingJian
---

# 前言

在开发过程中，如果需要并发的执行多个任务，Python 提供两种并行方式供开发者使用，分别是基于线程的并行 threading 和基于进程的并行 multiprocessing。

> 线程和进程的区别可以查看另一篇文章： [什么是多进程和多线程？什么是协程？](https://zhuanlan.zhihu.com/p/1930636923082342814)

> 本文介绍基于进程的并行 multiprocessing 模块，另一种基于线程的将在另一篇文章介绍。

> **推荐先看基于线程的并行**。

基于进程的并行 multiprocessing 是 Python 提供，通过子进程而非线程有效地绕过全局解释器锁，并可以充分利用多处理器的一个模块。

> 全局解释器锁是（Global Interpreter Lock，GIL）是 CPython 解释器的核心同步机制，其本质是一个互斥锁，它要求同一时刻内只能有一个线程执行 Python 代码。**GIL 这种机制的存在会导致在多核 CPU 的机器上，Python 程序无法使用其他核**。即使使用多线程也没办法实现真正意义的并行执行，其代码执行仍然是单核串行。

> 特别，Python 3.12 引入 nogil 不使用全局解释器锁的编译选项，Python 3.13 则默认移除全局解释器锁。

# 基本使用

在 multiprocessing 模块中，主要通过构建 Process 对象并调用 `start()` 方法来生成多线程，其使用代码结构与 Thread 一致。

`Process(group=None, target=None, name=None, args=(), kwargs={}, daemon=None)`

- group：不用管，其存在主要是与 Thread 一致。
- **target**：工作函数的函数名。
- name：进程名，默认以 Process-N 构建，也可以自定义。
- **args**：工作函数的位置参数，使用元组形式传递。
- **kwargs**：工作函数的关键字参数，使用字典形式传递。
- daemon：True/False，是否设置为守护进程（守护进程是为其他进程提供支持性服务，如垃圾回收、日志记录、资源监控等的进程。注意，守护进程在所有进程结束后强制结束，不管守护进程有没有执行结束）。

在实例化类 Process 后，便可以通过 `start()` 方法进行调用，同时使用 `join()` 方法进行等待，避免主进程提前关闭（如果不使用 `join()` 方法阻塞主进程，那么主进程有可能会在启动多进程后便直接结束，不会等待多进程的执行返回，最终导致多进程执行失败）。

特别的，`join()` 方法支持超时参数 timeout（单位秒），允许设置超时时间来在进程异常时不再阻塞主进程，继续执行主进程。

不过要注意的是，子进程在超时后不会结束，只是放在后台执行，不再阻塞主进程，请通过 `Process.is_alive()`，`Process.exitcode` 或 `multiprocessing.active_children()` 来检查进程是否终止。

与线程不同，进程提供 `terminate()`，`kill()` 方法来结束进程，这个两个方法的区别在于发送信号的不同，前者发送 SIGTERM，后者发送 SIGKILL。

> **特别注意：多进程的启动代码必须包裹在 `if __name__ == '__main__'` 内，避免子进程重复执行主进程的代码。**

```python
import multiprocessing  
import time  
  
# 模拟阻塞操作  
def crawl(date, delay=3):  
    print(f"crawl started for {date}")  
    time.sleep(delay)  
    print(f"crawl ended for {date}")  
  
if __name__ == '__main__':  
    # 建立工作进程池，并传入工作函数  
    process = []  
    for each in range(3):  
        # 实例化类 Process        
        # 使用 `args` 传入位置参数并使用 `kwargs` 传入关键字参数  
        p = multiprocessing.Process(target=crawl, 
                                    args=(each,), 
                                    kwargs={"delay": 2})  
        process.append(p)  
  
    # 设置并启动守护线程  
    daemon = multiprocessing.Process(target=crawl, 
                                     args=(100,), 
                                     daemon=True)  
    daemon.start()  
  
    # 启动每个线程  
    for p in process:  
        p.start()  
  
    # 查看当前的活跃的子进程列表长度  
    # 输出 4（守护线程 1 个 + 子线程 3 个）  
    print(len(multiprocessing.active_children()))  
  
    # 等待所有进程结束  
    for p in process:  
        p.join()  
  
    # 检查守护进程是否还没结束，手动结束  
    if daemon.is_alive():  
        print(f'{daemon.name} 还在执行')  
        daemon.kill()  
        print(f'{daemon.name} 结束了')
```

# 进程同步

multiprocessing 模块提供与 threading 模块基本一致的同步对象，使用方式也与 threading 模块类似。

multiprocessing 模块同样支持通过 `with` 语句来使用 `Lock`（互斥锁），`RLock`（重入锁），`Condition`（条件锁），`Semaphore`（信号量），`BoundedSemaphore`（有界信号量），`Event`（事件），`Barrier`（栅栏） 这些锁对象。

> 锁对象的详细介绍请查阅另一篇文章。

> **可以使用锁来确保一次只有一个进程打印到标准输出，这在多进程工作时是很有效的，这可以极大的避免多线程输出的混淆**。

```python
import multiprocessing  
import time  
    
def crawl(date, locker): 
    # 使用锁确保一次只有一个进程输出到控制台
    with locker:  
        time.sleep(1)  
        print(f"{date}")  
  
if __name__ == '__main__':  
    # 创建一个互斥锁
    lock = multiprocessing.Lock()  
  
    process = []  
    for each in range(3):  
        p = multiprocessing.Process(target=crawl, args=(each, lock))  
        process.append(p)  
  
    for p in process:  
        p.start()  
  
    for p in process:  
        p.join()
```

# 进程通信

multiprocessing 模块提供管道 Pipe 和队列 Queue 两种途径供多进程间通信。

## Pipe 管道通信

**Pipe 管道用于少量一对一进程间通信，它通过创建一对 `Connection` 连接对象 `(conn1, conn2)` 来完成通信。**

默认情况下这一对对象 `(conn1, conn2)` 是双工（双向）的，即 conn1 和 conn2 即能发送数据也能接收数据。通过设置 Pipe 管道的 duplex 参数为 False 可以使其变成单工，conn1 只能用于接收数据，conn2 只能发送数据。

**`Connection` 对象通过 `send()` 方法发送数据，通过 `recv()` 方法接收数据（recv 会阻塞进程直到接收到数据），在使用完成后，通过 `close()` 方法关闭连接。**

```python
import multiprocessing  
  
def process_a(conn):  
    conn.send("我是 A")  
    print("A 收到:", conn.recv())  
    # A 收到: 我是 B
    conn.close()  
  
def process_b(conn):  
    conn.send("我是 B")  
    print("B 收到:", conn.recv())  
    # B 收到: 我是 A
    conn.close()  
  
if __name__ == '__main__':  
    a_conn, b_conn = multiprocessing.Pipe() 
    # 单工 a_conn, b_conn = multiprocessing.Pipe(duplex=False)
  
    pa = multiprocessing.Process(target=process_a, args=(a_conn,))  
    pb = multiprocessing.Process(target=process_b, args=(b_conn,))  
  
    pa.start()  
    pb.start()  
  
    pa.join()  
    pb.join()
```

## Queue 队列通信

**Queue 队列是一种应用在复杂多对多进程间的通信方式，其通过构建一个 `Queue()` 对象来完成通信**。

相对于 Pipe 管道，Queue 队列是单向的，进程只能往队列发送数据，也只能从队列中获取数据，不支持直接的向另外一个进程发送数据。

**`Queue()` 对象支持传入一个整型用来限制队列的长度，支持通过调用 `put()` 方法和 `get()` 方法，按先进先出的规则分别写入数据和读取数据（请注意，get 读取的数据会从队列中移除）**。

注意，`put()` 和 `get()` 方法都会阻塞进程。`put()` 会等待队列有空闲，`get()` 会等待队列有数据。通过 `empty()` 和 `full()` 可以判断队列是否为空，是否满了。

```python
import multiprocessing  
import time  
  
def process_a(conn):  
    time.sleep(1)  
    # 队列不满，添加数据
    if not conn.full():  
        conn.put("我是 A")  
  
def process_b(conn):  
    time.sleep(2)  
    conn.put("我是 B")  
  
if __name__ == '__main__':  
    # 创建一个长度 5 的队列  
    queue = multiprocessing.Queue(5)  
  
    pa = multiprocessing.Process(target=process_a, args=(queue,))  
    pb = multiprocessing.Process(target=process_b, args=(queue,))  
  
    pa.start()  
    pb.start()  
  
    pa.join()  
    pb.join()  
  
    # 队列不空，读取数据
    if not queue.empty():  
        print(queue.get())  # 我是 A，A 先进先出
        print(queue.get())  # 我是 B
```

# 进程池

**multiprocessing 模块提供一个 Pool 对象用来供开发者创建一个进程池，而不用通过创建多个 Process 对象并添加到列表来管理进程**。

通过预先创建固定数量的工作进程，可以实现进程复用，避免频繁创建销毁的开销。同时支持同步或异步的提交多个任务，自动分配到空闲线程，资源最大化利用。

Pool 对象支持接收一个 processes 参数（默认 CPU 核心数量）用来表示要使用的工作进程数量；支持接收一个 maxtasksperchild 参数来限制单个进程最大的任务数。

**Pool 对象在创建后，可以通过 `apply()` 方法来创建一个任务，也可以通过 `map()` 方法一次创建多个任务并直接获取返回值列表。**

apply 方法适用于所有类型的函数，其支持通过位置参数元组 `(arg, )` 或关键字字典 `{key: value}` 来传入函数参数执行。

map 适用于只有 1 个位置参数（关键参数也可以有，但调用时使用默认值）的函数。map 支持一个函数名参数和一个可迭代对象参数，其会将可迭代对象的每一个值作为位置参数传入函数中执行，并自动将函数的返回值存储在一个列表中。

map 存在变种函数 starmap 适用于多个参数，其支持传入参数元组 `(arg1, arg2)`，然后将迭代的参数元组进行解包传递，即 `[func(arg1,arg2), func(arg3,arg4)]`。

```python
import multiprocessing  
  
def f1(x):  
    return x  
  
def f2(x, y):  
    return x, y  
  
def f3(x, y, z=5):  
    return x, y, z  
  
if __name__ == '__main__': 
    # 构建一个 4 工作线程的线程池
    with multiprocessing.Pool(processes=4) as pool:  
        # 适用于单个位置参数的函数
        res = pool.map(f1, range(5))  
        print(res)   # [0, 1, 2, 3, 4]

        # 适用于多个参数的函数
        res2 = pool.starmap(f2, [(1, 2), (2, 3), (4, 3)])  
        print(res2)  # [(1, 2), (2, 3), (4, 3)]

        # 适用于所有函数，但只能执行一次
        res3 = pool.apply(f3, (1, 2), kwds={'z': 3})  
        print(res3)  # (1, 2, 3)
```

特别的，上述的这 3 种任务创建方式都是同步的，即在执行时会阻塞主线程。multiprocessing 模块提供上述任务创建方式的异步版本 `apply_async()`，`map_async()`，`starmap_async()`。异步版本结果使用 `get()` 方法进行获取。

```python
import multiprocessing  
  
def f1(x):  
    return x  
  
def f2(x, y):  
    return x, y  
  
def f3(x, y, z=5):  
    return x, y, z  
  
if __name__ == '__main__':  
    with multiprocessing.Pool(processes=4) as pool:  
        # 异步调用函数
        res1 = pool.map_async(f1, range(5))  
        print('asynv1')  
        res2 = pool.starmap_async(f2, [(1, 2), (2, 3), (4, 3)])  
        print('asynv2')  
        res3 = pool.apply_async(f3, (1, 2), kwds={'z': 3})  
        print('asynv3')  
        # 先后打印 asynv1，asynv2，asynv3
        # 没有阻塞

        # 从结果中获取
        print(res1.get())  
        print(res2.get())  
        print(res3.get())
        # [0, 1, 2, 3, 4]，[(1, 2), (2, 3), (4, 3)]，(1, 2, 3)
```

————————————

> [ multiprocessing --- 基于进程的并行 ](https://docs.python.org/zh-cn/3/library/multiprocessing.html)
