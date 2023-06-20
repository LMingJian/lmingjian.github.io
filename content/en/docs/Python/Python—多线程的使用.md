---
title: Python 多线程代码的实现
date: 2020-12-13
author: LM
---

## 1.单线程

单线程执行任务时需要进行排序，比如下面代码中的听音乐和看电影两件事。

```python
# coding=utf-8
from time import ctime,sleep

def music(func):
    for i in range(2):
        print(f"I was listening to {func}! {ctime()}")
        sleep(1)

def move(func):
    for i in range(2):
        print(f"I was at the {func}! {ctime()}")
        sleep(5)

if __name__ == '__main__':
    music('爱情买卖')
    move('阿凡达')
    
    print(f"all over {ctime()}")
```

在执行时，先执行听音乐这件事，通过`for`循环来控制音乐播放两次，每首音乐播放需要1秒钟。接着执行看电影，每一场电影需要5秒钟，同样通过`for`循环看两遍。最后输出结束时间。

```bash
# 运行结果：
I was listening to 爱情买卖! Thu Apr 17 11:48:59 2014
I was listening to 爱情买卖! Thu Apr 17 11:49:00 2014
I was at the 阿凡达! Thu Apr 17 11:49:01 2014
I was at the 阿凡达! Thu Apr 17 11:49:06 2014
all over Thu Apr 17 11:49:11 2014
```

## 2.多线程

在多线程中，我们同时执行多个任务。`Python`中可以使用`threading` 来实现多线程，示例代码如下：

```python
#coding=utf-8
import threading
from time import ctime,sleep

def music(func):
    for i in range(2):
        print(f"I was listening to {func}! {ctime()}")
        sleep(1)

def move(func):
    for i in range(2):
        print(f"I was at the {func}! {ctime()}")
        sleep(5)

if __name__ == '__main__':
    # 使用 threading.Thread() 方法创建线程 t1，并在这个方法中调用 music，使用 args 对 music 进行传参。
    # 把创建好的线程 t1 装到所创建的 threads 数组中。
    threads = []
    t1 = threading.Thread(target=music, args=('爱情买卖',))
    threads.append(t1)
    t2 = threading.Thread(target=move, args=('阿凡达',))
    threads.append(t2)
    # 最后通过 for 循环遍历线程数组，开启线程。
    # setDaemon(True) 将线程声明为守护线程，必须在 start() 方法调用之前设置。
    # 如果不设置守护线程，那么在子线程启动后，父线程也继续执行下去，当父线程执行完最后一条语句后，不会等待子线程便直接退出，同时一并结束子线程束。
    for t in threads:
        t.setDaemon(True)
        t.start()
    # 对线程使用 join() 方法，用于等待线程终止。
    # join() 的作用是，在子线程完成运行之前，这个子线程的父线程将一直被阻塞。
    for t in threads:
        t.join()
    print(f"all over {ctime()}")
```

在执行时，由于使用了多线程，所以听音乐和看电影这两个任务会同时进行。可以从下面的运行结果看出。

```bash
# 运行结果：
I was listening to 爱情买卖. Thu Apr 17 12:51:45 2014 
I was at the 阿凡达! Thu Apr 17 12:51:45 2014
all over Thu Apr 17 12:51:45 2014
```

{{< details "参考文件" >}} 
1：[ python 多线程  @虫师 ](https://www.cnblogs.com/fnng/p/3670789.html)
{{< /details >}}