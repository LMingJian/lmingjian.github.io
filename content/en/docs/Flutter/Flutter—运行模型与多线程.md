---
title: Flutter 运行模型与多线程
date: 2021-08-25
author: LM
---

## 1.Dart 是一种单线程语言

Dart 是一种单线程语言，意味着同一时刻程序只能执行一个操作，其他操作在这个操作完成后执行，只要这个操作还在执行，它就不会被其他 Dart 代码中断。依赖于 Dart 的 Flutter 也是如此。

```dart
void myBigLoop(){
    for (int i = 0; i < 1000000; i++){
        _doSomethingSynchronously();
    }
}
```

在上述例子中，`myBigLoop()` 方法在执行完成前永远不会被中断，在整个方法执行期间应用将会被阻塞。

## 2.运行模型

在这里需要关注的是 Dart 的代码序列器（事件循环）。

当你启动一个 Flutter 或 Dart 应用时，应用将创建并启动一个新的线程进程 Isolate，这个线程将是整个应用的主线程。

在主线程启动后，应用会在此初始化 2 个 FIFO（先进先出）队列，`MicroTask`和 `Event`队列，在上述操作执行完成后，才会执行`main()`方法，并启动事件循环。

事件循环是一种由一个内部时钟控制的无限循环，在每个时钟周期内，如果没有其他 Dart 代码执行，则执行以下操作：

```dart
void eventLoop(){
    while (microTaskQueue.isNotEmpty){
        fetchFirstMicroTaskFromQueue();
        executeThisMicroTask();
        return;
    }

    if (eventQueue.isNotEmpty){
        fetchFirstEventFromQueue();
        executeThisEventRelatedCode();
    }
}
```

可以注意到，这个操作的作用是从`MicroTask`和 `Event`队列提取出事件到循环中执行，直到两个队列中所有事件执行完成。

### a.MicroTask 队列

`MicroTask`队列用于非常简短且需要异步执行的内部动作，这些动作需要在其他事件完成之后并在将执行权送还给`Event`队列之前运行。

### b.Event 队列

大多数需要使用异步的动作都使用`Event`队列进行处理，如外部事件 I/O，绘图等。值得注意的是，Future 操作也通过`Event`队列处理。

## 3.Future

Future 是一个异步执行并且在未来的某一个时刻完成（或失败）的任务。Future 并非并行执行，而是遵循事件循环处理事件的顺序规则执行。当你实例化一个 Future 时，应用会执行以下操作：

- 该 Future 的一个实例被创建并记录在由 Dart 管理的内部数组中；
- 需要由此 Future 执行的代码直接推送到 Event 队列中去；
- 该 Future 实例会返回一个状态（= incomplete）；
- 如果存在下一个同步代码，执行它（非 Future 的执行代码）;

只要事件循环从 Event 循环中获取它，被 Future 引用的代码将像其他任何 Event 一样执行。当该代码将被执行并将完成（或失败）时，`then()` 或 `catchError()` 方法将直接被触发。

```dart
void main(){
    print('Before the Future');
    Future((){
        print('Running the Future');
    }).then((_){
        print('Future is complete');
    });
    print('After the Future');
}
```

上述代码执行输出

```
Before the Future
After the Future
Running the Future
Future is complete
```

应用的执行执行流程如下：

1. `print("Before the Future")`
2. 将 `(){print("Running the Future");}` 添加到 `Event` 队列；
3. `print("After the Future")`
4. 事件循环获取（在第二步引用的）代码并执行它
5. 当代码执行时，它会查找 `then()` 语句并执行它

## 4.Async

当你使用 async 关键字作为方法声明的后缀时，Dart 会将其理解为：

- 该方法的返回值是一个 Future；
- 它同步执行该方法的代码直到第一个 await 关键字，然后它暂停该方法其他部分的执行；
- 一旦由 await 关键字引用的 Future 执行完成，下一行代码将立即执行。

这是 Dart 异步代码的同步实现。注意到的是，async 并非并行执行，也是遵循事件循环处理事件的顺序规则执行。

## 5.多线程（伪）

如何在 Flutter 中如何并行运行代码，这里我们可以使用 Isolate。

### a.Isolate 是什么？

Isolate 是 Dart 中的 线程。然而，它与常规「线程」的实现存在较大差异，Isolate 在 Flutter 中并不共享内存，不同 Isolate 之间通过消息进行通信，实际上 Isolate 更新进程。

### b.每个 Isolate 都有自己的事件循环

每个 Isolate 都拥有自己的事件循环及队列。这意味着在一个 Isolate 中运行的代码与另外一个 Isolate 不会存在任何关联。

### c.如何启动 Isolate

#### c1.创建并握手

由于Isolate 不共享任何内存并通过消息进行交互，因此，我们需要在调用者与 isolate 间建立通信。每个 Isolate 都暴露一个将消息传递给 Isolate 的被称为 SendPort 的端口，调用者和 isolate 需要互相知道彼此的端口才能进行通信。需要注意的是约束 isolate 的入口必须是顶级函数或静态方法。

```dart
// 新的 isolate 端口
// 该端口将在未来使用
// 用来给 isolate 发送消息
SendPort newIsolateSendPort;
// 新 Isolate 实例
Isolate newIsolate;

// 启动一个新的 isolate
// 然后开始第一次握手
void callerCreateIsolate() async {
    // 本地临时 ReceivePort
    // 用于检索新的 isolate 的 SendPort
    ReceivePort receivePort = ReceivePort();

    // 初始化新的 isolate
    newIsolate = await Isolate.spawn(
        callbackFunction,
        receivePort.sendPort,
    );

    // 检索要用于进一步通信的端口
    newIsolateSendPort = await receivePort.first;
}

// 新 isolate 的入口
static void callbackFunction(SendPort callerSendPort){
    // 一个 SendPort 实例，用来接收来自调用者的消息
    ReceivePort newIsolateReceivePort = ReceivePort();

    // 向调用者提供此 isolate 的 SendPort 引用
    callerSendPort.send(newIsolateReceivePort.sendPort);

    //
    // 进一步流程
    //
}
```

#### c2.向 Isolate 提交消息

```dart
// 向新 isolate 发送消息并接收回复的方法
// 在该例中，我将使用字符串进行通信操作
// （发送和接收的数据）
Future<String> sendReceive(String messageToBeSent) async {
    // 创建一个临时端口来接收回复
    ReceivePort port = ReceivePort();

    // 发送消息到 Isolate，并且
    // 通知该 isolate 哪个端口是用来提供回复的
    newIsolateSendPort.send(
        CrossIsolatesMessage<String>(
            sender: port.sendPort,
            message: messageToBeSent,
        )
    );

    // 等待回复并返回
    return port.first;
}

// 扩展回调函数来处理接输入报文
static void callbackFunction(SendPort callerSendPort){
    // 初始化一个 SendPort 来接收来自调用者的消息
    ReceivePort newIsolateReceivePort = ReceivePort();

    // 向调用者提供该 isolate 的 SendPort 引用
    callerSendPort.send(newIsolateReceivePort.sendPort);

    // 监听输入报文、处理并提供回复的
    // Isolate 主程序
    newIsolateReceivePort.listen((dynamic message){
        CrossIsolatesMessage incomingMessage = message as CrossIsolatesMessage;

        // 处理消息
        String newMessage = "complemented string " + incomingMessage.message;

        // 发送处理的结果
        incomingMessage.sender.send(newMessage);
    });
}

// 帮助类
class CrossIsolatesMessage<T> {
    final SendPort sender;
    final T message;

    CrossIsolatesMessage({
        @required this.sender,
        this.message,
    });
}
```

#### c3.销毁这个新的 Isolate 实例

```dart
// 释放一个 isolate 的例程
void dispose(){
    newIsolate?.kill(priority: Isolate.immediate);
    newIsolate = null;
}
```

#### c4.简单示例

```dart
//请求的目的端口
static SendPort server_TargetPort = null;

//客户端发起连接,拿到服务端的消息接收端口
void Connect() async
{
    ReceivePort client_receivePort = ReceivePort();
    //client_receivePort.sendPort 是指client_receivePort用于接收消息的端口
    await Isolate.spawn(Server_onReceivedMsg, [client_receivePort.sendPort,"hello world"]);
   //自行设计的约定，第一个消息为服务端消息接收端口
    server_TargetPort = await client_receivePort.first;
}
//发送消息
void SendToServer(String msg)
{
    ReceivePort port = ReceivePort();
    server_TargetPort.send([port.sendPort,msg])
    var resp = await port.first;
    print(resp.toString());
}

//服务端接收消息
void Server_onReceivedMsg(List args) async
{
	//第一个参数为客户端拥用于接收消息的端口
    SendPort sendPort = args[0];
    //第二个参数为"hello world"
    print(arg[1].toString());

	//创建服务端端口
    ReceivePort server_receivePort = ReceivePort();
    //把服务端接收消息的端口发送给客户端
    sendPort.send(server_receivePort.sendPort);
    
	//循环接收消息
    await for (List data in server_receivePort)
    {
        SendPort replayTo = data[0];
        String msg = data[1];
        replayTo.send("success : " + msg);
    }
 }

void test()
{
	Connect();
	SendToServer("abc");
	SendToServer("efg");
}
```

{{< details "参考文件" >}} 
1：[ Flutter 异步编程：Future、Isolate 和事件循环 ](https://www.jianshu.com/p/0aefa62372c6)
{{< /details >}}