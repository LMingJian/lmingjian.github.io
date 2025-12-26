---
title: Qt 如何进行多线程编程
date: 2025-12-25T13:47:41+08:00
author: LiangMingJian
---

# 需求

在 Qt 编程时，主窗口所在的主线程运行着用户设计的所有界面。当用户在主窗口点击某个按钮，希望执行某些耗时操作，如网络请求，文件读取，数据搜索时，如果是单一线程运行，则主窗口的整个界面会被**卡住**（因为线程正在运行其他耗时操作，导致无法更新界面），此时对用户的观感造成极大的影响。此时，使用多线程并发的运行程序就显得极为必要了。

在 Qt 中，**多线程可以通过使用 QThread 和 QThreadPool 来实现**，这两个组件分别适应少量多线程与大量多线程的使用环境。

# QThread

QThread 是 Qt 框架中的线程管理类，用于创建和控制**单个子线程**，每次调用一个 QThread 类，都代表启动了一个子线程。

QThread 类有两种使用方法，分别是重写 `run()` 方法和使用 `moveToThread()` 将工作对象移到子线程，这两种方法的区别在于：

- `run()` 方法**需要继承** QThread 并重写 `run()`，子线程在重写的 `run()` 方法执行完成后就会销毁，**不允许线程的复用**。
- `moveToThread()` 方法**不需要继承** QThread，也不用重写 `run()`，它将工作对象移动到子线程单独运行，通过信号机制，可以多次，重复的调用工作对象中的方法，子线程不会在某一个方法执行完成后销毁，**允许线程的复用**。

正因 `moveToThread()` 方法的支持复用，因此在实际使用时，一般推荐使用 `moveToThread()` 完成多线程的使用。

**在使用 `moveToThread()` 实现方式时，工作对象可以按以下结构进行设计**：

```python
# 构建工作线程，没什么特殊要求，都建议继承 QObject
class Worker(QObject):  
    # 定义工作线程的完成信号  
    finished1 = Signal()  
    # 如果工作线程需要返回数据，则标注数据类型  
    finished2 = Signal(int)  
    finished3 = Signal(str, int)  
  
    @Slot()  # 指示这个函数是槽函数（信号的执行函数）（显示指定槽函数能让系统自动的为其进行性能优化）  
    def do_work1(self):  
        pass  # 耗时操作  
        self.finished1.emit()  # 发送执行结束的信号  
  
    @Slot(int)  # 如果这个槽函数带形参，则要在这里进行标注  
    def do_work3(self, i: int):  
        pass  # 耗时操作  
        self.finished3.emit('End', i)  # 发送带参数的结束信号
```

**上述工作对象在使用 `moveToThread()` 方式时，可以按以下步骤进行调用**：

```python
# 主窗口类  
class MainWindow(QMainWindow):  
    # 定义工作对象的启动信号  
    start1 = Signal()  
    # 如果工作对象要有参数，则定义一个带参数的信号  
    start2 = Signal(int)  
  
    # 类初始化  
    def __init__(self):  
        # UI 初始化  
        super(MainWindow, self).__init__()  
        self.ui = Ui_MainWindow()  
        self.ui.setupUi(self)  
  
        # 工作对象初始化  
        self.worker = Worker()  
        # 工作线程初始化  
        self.worker_thread = QThread()  
        # 将工作对象移动到工作线程  
        self.worker.moveToThread(self.worker_thread)  
        # 启动工作线程（这个时候，工作对象就实例化了，我们可以通过信号来调用对象里面的耗时方法）  
        self.worker_thread.start()
```

工作线程在启动后，被移动到工作线程里面的工作对象也就在主线程中脱离了，此时如果调用工作对象中的所有方法，这些方法都会独立的在子线程中运行，不会对主线程造成影响，此时，主线程里面的主窗口和工作线程里的工作对象通过信号进行唯一的联系。

**正常来说，我们需要对下述的这些信号进行绑定**：

```python
...
	def __init__(self):
		...
		# 将主窗口的工作对象启动信号绑定到工作对象的对应耗时方法
		self.start1.connect(self.worker.do_work1)  
		self.start2.connect(self.worker.do_work2)
		# 将工作对象的完成信号绑定到主窗口中的完成槽函数
		self.worker.finished1.connect(self.finished1)
		self.worker.finished3.connect(self.finished3)
		
	@Slot()  
	def finished1(self):  
	    pass  
	      
	@Slot(str, int)  
	def finished3(self, s: str, i: int):  
	    pass
```

**在完成上述的代码编写后，我们就可以在后续使用时通过 start1，start2 这两个信号的发送来调用工作对象中的耗时方法**：

```python
self.start1.emit()  
self.start2.emit(0)
```

需要注意，因为 `moveToThread()` 线程支持复用，因此在不用的时候需要手动的结束。不过正常的，结束主线程时，子线程也会正常结束，如果没什么特殊的要求，可以不用管。

如果确实需要结束工作线程，则建议遵循以下的步骤，安全的结束线程：

```python
...
# 先结束工作线程
self.worker_thread.quit()
# 再删除工作对象
self.worker.deleteLater()
# 最后再删除工作线程
self.thread.finished.connect(self.thread.deleteLater)
```

> 拓展阅读1：如果想要使用 `run()` 方法，则可以参照下面的代码，继承 QThread 类进行重写。

```python
class WorkerThread(QThread): 
	finished = Signal() 
	
	def run(self): 
		pass # 耗时操作
		self.finished.emit()

class MainWindow(QMainWindow):
	def __inin__(self):
		super().__init__() 
		# 初始化子线程
		self.thread = WorkerThread()
		# 运行子线程
		self.thread.start()
```

> 拓展阅读2：QThread 除了支持通过 `start()` 方法启动外，还支持 `quit()`（结束线程运行），`terminate()`（强制结束运行），`wait()`（阻塞当前线程），`isRunning()`（判断线程是否运行），`isFinished()`（判断线程是否结束）等方法来管理线程。

# QThreadPool

QThreadPool 是 Qt 用来管理和调度**多个线程**执行的一个类，与 QThread 一次管理一个线程不同，QThreadPool 可以一次自动管理多个线程，适合用来处理需要多个复杂线程的程序。

QThreadPool 的使用需要先实例化 QThreadPool 来构建一个线程池，然后通过继承 ‌QRunnable 来构建一个可用的工作对象，最后通过 `QThreadPool.start(QRunnable)` 的方法，将工作对象移动到线程池来进行工作。

需要注意，QThreadPool 里面的工作对象 QRunnable 其实是类似于 QThread 中 `run()` 方法重写的实现，在通过 `start()` 方法调用启动后，其便会自动执行，在工作结束后自动的从线程中移除，**自动删除已经结束的任务**。

> QThreadPool 会在 QRunnable 的 `run()` 方法结束返回后，自动删除该任务，通过 `setAutoDelete(True)` 可以修改自动删除任务的默认行为。

因此，我们一般可以将 QRunnable 编写为一个**接收函数**的对象。这样，在每次调用 `start()` 时，我们都可以通过传入不同的函数，来调用不同的方法，完成不同的任务，减少代码量。

**对此，QRunnable 的实现代码如下**：

```python
# 继承 QRunnable 实现一个工作对象类
class Worker(QRunnable):
	# 构建信号
    finished = Signal()
    result = Signal(object)
    error = Signal(tuple)
    
    # 支持传入一个 fn 函数参数
    # *args 表示支持任意非关键字参数, **kwargs 表示支持任意关键字参数
    def __init__(self, fn, *args, **kwargs):
        super().__init__()
        self.fn = fn
        self.args = args
        self.kwargs = kwargs
        # 设置一个互斥锁，保护共享资源
        self.mutex = QMutex()

	# 重写 run 执行方法
    def run(self):
        try:
	        # 启用互斥
            with QMutexLocker(self.mutex):
	            # 执行传入的 fn 函数，参数复制 *args 和 **kwargs
                result = self.fn(*self.args, **self.kwargs)
            # 发送结果
            self.result.emit(result)
        except Exception as e:
	        # 发送异常
            self.error.emit((type(e), str(e), traceback.format_exc()))
        finally:
            # 发送结束
            self.finished.emit()
        return
```

**在构建好 QRunnable 对象后，便可以实例化线程池。线程池的实例方法有两种，依据需要可以任意选择**：

```python
# 这种方法应用全局，可以跨函数，类，模块
threadpool = QThreadPool.globalInstance()

# 这种方法应用局部，不可以跨函数，类，模块
self.threadpool = QThreadPool()
```

上述的两种实例使用方法都一样的，只是作用范围不同而已，一般建议直接使用全局，避免资源浪费。

> QThreadPool 支持通过 `setMaxThreadCount(10)` 的方法限定启动的线程最大数量，避免溢出。

最后，在完成上述配置后，我们便可以通过 `start()` 方法启动任务，开始多线程工作的运行。

```python
...
# 工作任务
def do_something(self, i):
	pass  # 耗时操作
	return i  # 返回执行结果

# 工作回调
def resultEven(self, i):
	print(i)
	return

# 分配工作任务
def run_work(self):
	for i in range(5):
		# 实例化工作对象
		worker = Worker(do_something, i)
		# 绑定结束信号的槽函数
		worker.result.connect(self.resultEven)
		# 启动工作对象
		threadpool.start(worker)
	return
```

特别注意，QThreadPool 不提供方法强制的结束 QRunnable 的任务，因此如何想取消任务，一般建议编写信号槽来实现。

QThreadPool 提供 `clear()` 方法，清空当前任务队列（移除所有未执行的任务，不再接收新的任务，但已有正在执行的任务会等待结束，无法取消）。

QThreadPool 可用通过 `activeThreadCount()` 获取当前活跃的线程数。

————————————

在少量使用多线程时，使用 QThread。

在大量使用多线程时，使用 QThreadPool。
