---
title: 如何使用 Python 从 threading 获取多线程的返回值
date: 2024-12-25T16:09:12+08:00
author: LiangMingJian
---

# 需求

在 Python 中，`threading` 是用于实现多线程并发的模块，但 `threading` 有个问题，即它无法直接返回单个线程里面运行的结果，当我们需要子线程返回其处理结果时，只能通过特殊的方法实现。

# 通过继承 Thread 自定义返回方法

通过自定义继承类 `Thread`，复写 `run` 方法，并在 `run` 方法中把返回值赋值给 `result`，最后通过自定义并调用 `get_result` 方法获取每个线程的返回值。

```python
import threading  
import time  
from random import random  
  
def crawl(link, delay=3):  
    """ 模拟耗时操作 """    
    print(f"crawl started for {link}")  
    time.sleep(delay)  # 阻塞 I/O (模拟网络请求)  
    print(f"crawl ended for {link}")  
    return {link: random()}  
  
class ReturnThread(threading.Thread):  
    """ 继承 Tread """
    result = None  
  
    def __init__(self, target=None, args=(), kwargs=None):  
        super(ReturnThread, self).__init__()  
        self.target = target  
        self.args = args  
        self.kwargs = kwargs  
  
    def run(self):  
        """ 获取返回值 """
        self.result = self.target(*self.args, **self.kwargs)  
  
    def get_result(self): 
        """ 返回 """ 
        return self.result  
  
if __name__ == "__main__":  
    # 针对每个链接启动线程  
    threads = []  
    result = []  
    links = [  
        "https://python.org",  
        "https://docs.python.org",  
        "https://peps.python.org",  
    ]  
  
    for each in links:  
        # 使用 `args` 传入位置参数并使用 `kwargs` 传入关键字参数  
        t = ReturnThread(target=crawl, args=(each,), kwargs={"delay": 2})  
        threads.append(t)  
  
    # 启动每个线程  
    for t in threads:  
        t.start()  
  
    # 等待所有线程结束  
    for t in threads:  
        t.join()  
        """获取返回值"""
        result.append(t.get_result())  
        
    print(result)
```

执行结果：

```
crawl started for https://python.org
crawl started for https://docs.python.org
crawl started for https://peps.python.org
crawl ended for https://peps.python.org
crawl ended for https://python.org
crawl ended for https://docs.python.org
[{'https://python.org': 0.13342107887558985}, {'https://docs.python.org': 0.05891051098876887}, {'https://peps.python.org': 0.4964428239313935}]
```

# 通过内置队列`Queue`接收返回值

在 Python 中，使用 queue 模块可以设置用于线程间安全共享数据的队列。

设置任务队列和结果队列，子线程从任务队列中获取任务并执行，然后在执行结束后将返回值写入对应的结果队列，最后用户通过结果队列安全的获取返回值。

```python
import threading  
import time  
from random import random  
import queue  
  
def crawl(link, delay=3):  
    """ 模拟耗时操作 """    
    print(f"crawl started for {link}")  
    time.sleep(delay)  # 阻塞 I/O (模拟网络请求)  
    print(f"crawl ended for {link}")  
    return {link: random()}  
  
def worker(task_queue, result_queue, delay=3):  
    """ 从队列中获取任务，并返回值到队列中 """    
    while not task_queue.empty():  
        try:  
            link = task_queue.get_nowait()  # 获取还没有使用的任务（值）  
            result = crawl(link, delay)  # 执行耗时任务  
            result_queue.put(result)  # 将结果返回  
        except queue.Empty:  # 队列为空的时候结束执行  
            break  
  
if __name__ == "__main__":  
    # 针对每个链接启动线程  
    threads = []  
    results = []  
    tasks_queue = queue.Queue()  
    results_queue = queue.Queue()  
    links = [  
        "https://python.org",  
        "https://docs.python.org",  
        "https://peps.python.org",  
    ]  
  
    for each in links:  # 给任务队列分配  
        tasks_queue.put(each)  
  
    for each in range(len(links)):  # 创建指定数量的线程  
        # 使用 `args` 传入位置参数并使用 `kwargs` 传入关键字参数  
        t = threading.Thread(target=worker, args=(tasks_queue, results_queue), kwargs={"delay": 2})  
        threads.append(t)  
  
    # 启动每个线程  
    for t in threads:  
        t.start()  
  
    # 等待所有线程结束  
    for t in threads:  
        t.join()  
  
    # 从结果队列中获取返回值  
    while not results_queue.empty():  
        results.append(results_queue.get())  
  
    print(results)
```

执行结果：

```
crawl started for https://python.org
crawl started for https://docs.python.org
crawl started for https://peps.python.org
crawl ended for https://peps.python.org
crawl ended for https://python.org
crawl ended for https://docs.python.org
[{'https://peps.python.org': 0.9430907045591979}, {'https://docs.python.org': 0.3050054037315165}, {'https://python.org': 0.2066999261107899}]
```