---
title: Python 基于线程的并行 threading
date: 2024-12-25T16:10:35+08:00
author: LiangMingJian
---

# 前言

在开发过程中，如果需要并发的执行多个任务，Python 提供两种并行方式供开发者使用，分别是基于线程的并行 threading 和基于进程的并行 multiprocessing。

> 线程和进程的区别可以查看另一篇文章：[ 什么是多进程和多线程？什么是协程？](https://zhuanlan.zhihu.com/p/1930636923082342814)

> 本文介绍基于线程的并行 threading 模块，另一种基于进程的将在另一篇文章介绍。

基于线程的并行 threading 是 Python 提供用以在单个进程内并发运行多个线程的一个模块。它允许创建和管理线程，以便能够平行地执行多个任务，并共享内存空间。

# 基本使用

threading 模块的使用，最重要的步骤是实例化类 Thread，并传入工作函数与参数。

`Thread(group=None, target=None, name=None, args=(), kwargs={}, daemon=None)`

- group：保留参数，不用管，未来类 ThreadGroup 的扩展。
- **target**：工作函数的函数名。
- name：线程名，默认以 Thread-N 构建，也可以自定义。
- **args**：工作函数的位置参数，使用元组形式传递。
- **kwargs**：工作函数的关键字参数，使用字典形式传递。
- daemon：True/False，是否设置为守护线程（守护线程是为其他线程提供支持性服务，如垃圾回收、日志记录、资源监控等的线程。注意，**守护线程在所有线程结束后强制结束，不管守护线程有没有执行结束**）。

在实例化类 Thread 后，便可以通过列表保存线程池，接着对线程池内的每一个**实例通过 `start()` 方法进行调用，同时使用 `join()` 方法进行等待**，避免主线程提前关闭（如果不使用 `join()` 方法阻塞主线程，那么主程序有可能会在启动多线程后便直接结束，不会等待多线程的执行返回，最终导致多线程执行失败）。

特别的，`join()` 方法支持超时参数 timeout（单位秒），允许设置超时时间来在线程异常时不再阻塞主线程，继续执行主线程。

不过要注意的是，**子线程在超时后不会结束，只是放在后台执行，不再阻塞主线程**，通过 `thread.is_alive()` 或 `threading.active_count()` 还是可以看见线程在运行的（Python 的 threading 模块不提供直接结束线程的方法，因此请尽量保证线程可以正常的执行并返回值，避免设置超时时间）。

示例的代码如下：

```python
import threading  
import time  
  
# 模拟阻塞操作  
def crawl(date, delay=3):  
    print(f"crawl started for {date}")  
    time.sleep(delay)  
    print(f"crawl ended for {date}")  
  
# 建立工作线程池，并传入工作函数  
threads = []  
for each in range(3):  
    # 实例化类 Thread    
    # 使用 `args` 传入位置参数并使用 `kwargs` 传入关键字参数  
    t = threading.Thread(target=crawl, args=(each,), kwargs={"delay": 2})  
    threads.append(t)  
  
# 设置并启动守护线程 
daemon = threading.Thread(target=crawl, args=(100,), daemon=True)  
daemon.start()  
  
# 启动每个线程
for t in threads:  
    t.start()  
  
# 检查子线程是否还没结束  
for t in threads:  
    if t.is_alive():  
        print(f'{t.name} 还在执行')  

# 查看当前的线程计数
# 输出 5（主线程 1 个 + 守护线程 1 个 + 子线程 3 个）
print(threading.active_count())  
  
# 等待所有线程结束  
for t in threads:  
    t.join()
```

# 线程局部数据

类 `threading.local()` 为基于线程的并发提供一个设置线程局部数据的功能，**通过 `threading.local()` 设置的变量，在子线程内的所有调用修改都不会影响到主线程，主线程的所有调用修改也不会影响子线程，即每次线程里面的 local 变量都是新的**。

```python
import threading  
import time  

# 创建一个线程局部数据
mydata = threading.local()  
# 在主线程中设置一个变量 number
mydata.number = 100  
  
# 模拟阻塞操作  
def crawl(date, delay=3): 
    # 在子线程内设置一个变量 number 和 name
    mydata.name = '子线程'  
    mydata.number = date  
    print(f'{mydata.name}： {mydata.number}')  # 子线程： 1
    time.sleep(delay)  
  
t = threading.Thread(target=crawl, args=(1,), kwargs={"delay": 2})  
t.start()  
t.join()  

print(mydata.number)  # 100
print(mydata.name)  # no attribute 'name'
```

特别的，可以通过继承 `threading.local()` 实现一些默认值支持。

```python
import threading  
import time  
  
class MyLocal(threading.local):  
    name = '线程'  
  
mydata = MyLocal()  
mydata.number = 100  
  
# 模拟阻塞操作  
def crawl(date, delay=3):  
    mydata.number = date  
    print(f'{mydata.name}： {mydata.number}')  # 线程： 1 
    time.sleep(delay)  
  
t = threading.Thread(target=crawl, args=(1,), kwargs={"delay": 2})  
t.start()  
t.join()  
  
print(mydata.number)  # 100  
print(mydata.name)  # 线程
```

# 线程同步

## 线程锁

在 threading 模块中，提供 3 种类对象来实现线程同步，这些类对象有 `Lock`，`RLock` 和 `Condition`。

**Lock 是最简单的互斥锁，其要求同一时间只有一个线程可以访问资源**。使用时可直接通过 `with` 语句进行自动获取和释放（如需手动管理，则使用`locked()` 检查，`acquire()` 获取，`release()` 释放等方法）。

```python
import threading  
import time  
  
lock = threading.Lock()  
data = 0  
  
def increment():  
    time.sleep(3)  
    global data  
    # 自动获取和释放锁  
    with lock:  
        data += 1
     
threads = []  
for each in range(3):  
    t = threading.Thread(target=increment)  
    threads.append(t)  
  
for t in threads:  
    t.start()  
  
for t in threads:  
    t.join()  

print(data)  # 输出结果 3
```

**RLock 是适用于递归函数或嵌套函数调用时使用的锁，其要求在锁定状态下，某些线程拥有锁；在非锁定状态下，没有线程拥有它**。使用时同样可以通过 `with` 语句进行自动获取和释放。

```python
import threading  
  
rlock = threading.RLock()  
data = 0  

# 一个递归函数，需要重复使用锁
def recursive(n):  
    global data  
    with rlock:  
        if n > 0:  
            data += 1  
            recursive(n - 1)  
  
threads = []  
for each in range(3):  
    t = threading.Thread(target=recursive, args=(2, ))  
    threads.append(t)  
  
for t in threads:  
    t.start()  
  
for t in threads:  
    t.join()  
  
print(data)  # 输出结果 6
```

**Condition 是结合了条件判断的一个锁，其要求线程在特定条件下等待或通知其他线程继续操作**。与 Lock 和 RLock 不同，Condition 除了要通过 `with` 语句使用外，还需要手动通过 `condition.wait()` 和 `condition.notify(n=1)`（默认唤醒一个等待这个条件的线程，最多随机唤醒 n 个等待的线程），`condition.notify_all()` （唤醒所有）来等待和通知其他线程执行。

```python
import threading  
import time  
  
condition = threading.Condition()  
data = []  
  
def producer():  
    global data  
    with condition:  
        time.sleep(3)  
        data.append(1)  
        # 通知等待这个锁的随机 1 个线程继续执行  
        condition.notify()  
  
def consumer():  
    global data  
    with condition:  
        # 当列表为空时，释放锁，然后挂起等待 
        while not data:  
            condition.wait()  
        data.append(2)
  
# 启动线程  
t1 = threading.Thread(target=producer)  
t2 = threading.Thread(target=consumer)  
  
t1.start()  
t2.start()  
  
t1.join()  
t2.join()  
  
print(data)  # 输出：[1, 2]
```

## 线程信号量（数量限制）

**此外，针对线程同步，threading 模块还提供类 `Semaphore` 和 `BoundedSemaphore` 用于控制同时访问共享资源的线程数量**。

`Semaphore` 和 `BoundedSemaphore` 的控制是通过锁的释放计数进行的，每次释放都会使内部的计数器加 1，当有锁被获取时，则计数器减 1。两者的区别在于 `BoundedSemaphore` 对资源限制更为严格，当锁释放超过设定值时，就会抛出异常。

**Semaphore 的使用**：

```python
import threading  
import time  
  
semaphore = threading.Semaphore(3)  # 允许最多 3 个线程同时访问  
  
def task():  
    with semaphore:  
        print(threading.current_thread().name, "执行任务")  
        time.sleep(3)  
        # 先打印 3 个线程，最后再打印剩下的 2 个线程
  
threads = [threading.Thread(target=task) for _ in range(5)]  
  
for t in threads:  
    t.start()  
  
for t in threads:  
    t.join()
```

**BoundedSemaphore 的使用**：

```python
import threading  

# 计数器最多是 3 
semaphore = threading.BoundedSemaphore(3)  # 允许最多 3 个线程同时访问
# 同时释放 4 个锁，导致计数器加 4，超过限制
semaphore.release(4)  # ValueError: Semaphore released too many times
```

## 线程栅栏（同步等待）

**特别的，若要让线程在达到某个条件前都进行等待，然后在条件达到后一起执行，则可以使用 Barrier 对象。**

Barrier 对象支持传入参数 `parties`（目标线程数），`action`（等待完成后的回调），`timeout`（超时时间）来进行实例化。

实例化后的对象可以通过方法 `wait()` 来进行等待，当等待的线程数等于 `parties` 时，所有线程开始执行。

存在方法 `reset()` 和  `abort()` 重置等待和中断登录，但会导致已完成等待的线程或后续调用 `wait()` 操作的线程出现报错，请尽量避免使用。

```python
import threading  
  
def ready_action():  
    print("所有线程已准备")  
  
barrier = threading.Barrier(3, ready_action)  
  
def worker(name):  
    print(f"{name}已准备")  
    barrier.wait()  
    print(f"{name}开始操作")  
  
threads = [  
    threading.Thread(target=worker, args=("A",)),  
    threading.Thread(target=worker, args=("B",)),  
    threading.Thread(target=worker, args=("C",))  
]  
  
for t in threads:  
    t.start()  
      
for t in threads:  
    t.join()
```

# 线程通信

**threading 模块通过 Even 对象来实现线程通信。Even 即事件，也可以说是事件信号，一个线程通过发送事件信号给其他线程来完成通信。**

一个 Even 对象通过调用 `set()` 方法设置/发送信号为 True，通过 `wait()` 方法等待/接收信号为真，通过 `clear()` 清理信号为 False。

```python
import threading  
import time  
  
even = threading.Event()  
  
def producer():  
    print("设置信号...")  
    time.sleep(3)  
    even.set()   # 发送信号  
  
def consumer():  
    print("等待信号...")  
    even.wait()  # 阻塞等待  
    print("收到数据")  
  
t1 = threading.Thread(target=producer)  
t2 = threading.Thread(target=consumer)  
  
t1.start()  
t2.start() 

t1.join()  
t2.join()
```

# 拓展

threading 模块提供一个定时器，用以在给定时间后执行相应方法函数。

```python
import threading  
  
def hello():  
    print("hello, world")  

# 3 秒后执行 hello 方法
t = threading.Timer(3, hello)  
t.start()
```

————————————

> [ threading --- 基于线程的并行 ](https://docs.python.org/zh-cn/3/library/threading.html)
