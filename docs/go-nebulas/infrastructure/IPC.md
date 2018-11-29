# IPC
IPC为NEB和NBRE交互的信使。

### 功能
IPC利用共享内存为NEB和NBRE进行通信，提高性能。
在IPC内部存在server和client两个子线程。其中server监听NEB的请求并，client监听NBRE的结果返回，同时它们之间又进行通信交互。

### 框架
IPC的框架图如下所示。

![](https://github.com/nebulasio/nebdocs/blob/zh-CN/docs/resources/IPC.png)


1. NEB提出一个功能运行申请，server接收该申请并发送到client；
2. client将申请发送到NBRE；
3. NBRE运行对应程序并返回结果给client，client将结果传送到server；
4. server将结果返回到NEB。
