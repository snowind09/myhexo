---
title: I/O模型学习小记
tags:
  - Linux
  - 网络编程
categories: 技术
date: 2016-04-03 23:44:40
---
## 基础概念
* 通过I/O模型学习**同步/异步、阻塞/非阻塞**基础概念，参考资料如下：
[《Unix网络编程》](https://book.douban.com/subject/1500149/)
[《网络编程释疑之：同步，异步，阻塞，非阻塞》](http://yaocoder.blog.51cto.com/2668309/1308899)
[《Java NIO：浅析I/O模型》](http://www.cnblogs.com/dolphin0520/p/3916526.html)
[《高性能IO模型浅析》](http://www.cnblogs.com/fanzhidongyzby/p/4098546.html)

<!--more-->

* 基础概念学习小结
 * 系统I/O操作涉及用户进程和内核进程两个通信主体，有四种**常用**I/O模型：阻塞I/O，非阻塞I/O，I/O复用，异步I/O
  * 从用户进程主体角度来看，**同步I/O**请求操作分为两个阶段
     * A：等待内核准备数据，即数据完全到达内核空间
     * B：等待内核将数据从内核空间拷贝至用户空间
  * **阻塞和非阻塞I/O**的区别发生在A阶段，如用户进程始终等待A阶段完成，则为阻塞I/O，否则为非阻塞I/O，而在B阶段，都需要用户进程进行数据读写操作
  * 从用户进程主体角度来看，**异步I/O**请求操作**不感知**内核主体的操作阶段，发送I/O请求后立即返回，内核会自动完成A、B阶段工作，并**通知**用户进程进行数据处理，此机制需要**操作系统**底层的支持，如下图所示
![](http://7xshxx.com2.z0.glb.clouddn.com/IO_diff.jpg)

## 多路复用模型(IO Multiplexing)
  * 多路复用I/O模型是**同步阻塞**的，这里是从应用进程的角度来说
  * 多路复用I/O**阻塞**在select、epoll这样的系统调用上，而不是阻塞在I/O系统调用recvfrom上，因此也被称为**非阻塞I/O**(Non-blocking I/O)
![](http://7xshxx.com2.z0.glb.clouddn.com/duolufuyong.jpg?imageView/2/w/700/)
  * 对于单个I/O操作，多路复用并不比阻塞I/O有优势，反而增加了一次系统调用
  * 多路复用I/O的优势体现在同时处理多个文件描述符输入输出操作、多个套接字操作的时候，有任何一个准备就绪，select就可以返回（从并发的角度看，也有人认为这是“异步”，但从单个用户线程的角度看，仍处于等待数据或者拷贝数据的状态）
  * 多路复用I/O中，轮询每个socket状态是内核在进行的，这个效率要比用户进程要高的多，因此性能强于非阻塞I/O在用户进程中的轮询
  * 多路复用的几种系统调用之间的区别，参考[《select、poll、epoll之间的区别总结》](http://www.cnblogs.com/Anker/p/3265058.html)


## 容易发生混淆的坑
很多人将多路复用模型(IO Multiplexing)称之为异步阻塞I/O，这种方式将引起误解，为此，澄清如下：
> Synchronous I/O versus Asynchronous I/O

>  POSIX defines these two terms as follows:

>  A synchronous I/O operation causes the requesting process to be blocked until that I/O operation completes.
An asynchronous I/O operation does not cause the requesting process to be blocked.

> Using these definitions, the first four I/O models (blocking, nonblocking, I/O multiplexing, and signal-driven I/O) are all **synchronous** because the actual I/O operation (recvfrom) blocks the process. Only the asynchronous I/O model matches the asynchronous I/O definition.