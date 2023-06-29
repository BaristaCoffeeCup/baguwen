
## Java中的IO流如何分类？
| 基类 | 操作单元 | 数据流向 | 常见类 |
| :--: | :--: | :--: | :--: |
| InputStream | 字节流 8位 | 输入流 | FileInputStream
| OutputStream | 字节流 8位 | 输出流 | FileOutputStream
| Reader | 字符流 16位 | 输入流 | FileReader
| Writer | 字符流 16位 | 输出流 | FileWriter

![字节流](../八股文/图例/字节流.png)
![字符流](../八股文/图例/字符流.png)
![数据操作分类](../八股文/图例/数据操作分类.png)

<br></br>


## Java的I/O模型分类
- 同步阻塞IO（Blocking IO）：也称为传统的IO模型，它在IO操作过程中会阻塞线程，直到数据完全读取或写入才返回结果。

- 同步非阻塞IO（Non-blocking IO）：使用Java NIO（New IO）库实现的IO模型。在进行IO操作时，线程不会被阻塞，而是立即返回。但是线程仍需要轮询来检查IO操作是否完成，这可能导致CPU资源浪费。

- 多路复用IO（Multiplexing IO）：通过使用Java NIO库中的Selector选择器，一个线程可以同时监听多个通道的IO事件。当某个通道有IO事件发生时，线程会被唤醒处理该事件，其他无事件的通道则会继续监听。

- 信号驱动IO（Signal-driven IO）：使用Java NIO库中的信号机制，让操作系统在IO事件到达时发送信号给应用程序，然后应用程序处理相应的信号。

- 异步IO（Asynchronous IO）：使用Java NIO库中的CompletableFuture或者回调函数等方式实现异步IO。应用程序发起IO操作后，可以继续执行其他任务，当IO操作完成时，会通过回调函数或者Future对象得到通知。

