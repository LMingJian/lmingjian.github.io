---
title: Python 多线程代码的返回值
date: 2020-12-14
author: LM
---

## 1.需求

`python` 多线程一般使用 `threading` 模块，但 `threading` 模块有个问题，无法直接返回线程里面运行的结果，那如果需要线程返回值那如何处理呢。

## 2.实现：通过自定义线程类接受返回值

通过自定义线程类，继承`Thread`类，并复写`run`方法，在`run`方法中写入执行函数的方式，并把返回值赋值给`result`，然后通过调用`get_result`获取每个进程的返回值。

```python
# 多线程类
class MyThread(Thread):
    result1 = None
    result2 = None

    def __init__(self, func, args=()):
        super(MyThread, self).__init__()
        self.func = func
        self.args = args

    def run(self):
        self.result1, self.result2 = self.func(*self.args)
        # 在执行函数的同时，把结果赋值给result,
        # 然后通过get_result函数获取返回的结果

    def get_result(self):
        # noinspection PyBroadException
        try:
            return self.result1, self.result2
        except Exception:
            return None


# 多线程执行函数
def Mss(info, window):
    result, furl = api_test(info, baseurl)
    sleep(0.5)
    # 等待，否则由于子进程过快，主进程未开启进度条，而执行进度条关闭命令导致矛盾
    window.close()
    return result, furl

t1 = MyThread(Mss, args=(info, myDia5))
t1.setDaemon(True)
t1.start()
result, furl = t1.get_result()
```

## 3.实现：通过内置队列`Queue`接收返回值

通过`python`内置的队列`Queue`接收子进程的返回值，然后再从中取出。

```python
import threading 
import Queue 
def is_even(value, q): 
    if value % 2 == 0: 
        q.put(True) 
    else: 
        q.put(False) 
        
def multithreading(): 
    q = Queue.Queue() 
    threads = [] 
    results = [] 
    
for i in range(10): 
    t = threading.Thread(target=is_even, args=(i, q)) 
    t.start() 
    threads.append(t) 
    
for thread in threads: 
    thread.join() # 等待子线程结束后，再往后面执行 
    
for _ in range(10): 
    results.append(q.get()) 
    print(results) 
    
    multithreading() 
```