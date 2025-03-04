# java-io-nio

## IO简介

```text
IO是Java中的一种输入和输出的功能，Java中对这种操作叫做对流的操作。
流代表的是任何有能力产出数据的数据源对象或者是有能力接受数据的接收端对象。
流的本质是数据传输，流不只是对文件可进行读写，还可以对内存、网络、程序操作。
```

### 学习计划

* [Java IO、NIO原理](https://www.jianshu.com/p/63d1c4476f45)
* [讲的最好的同步/异步/阻塞/非阻塞/BIO/NIO/AIO的文章](http://note.youdao.com/noteshare?id=e00d8caf0973471780b8ca880e1c5bf9&sub=wcp1578879502523399)
* [零拷贝](http://note.youdao.com/noteshare?id=13101142e0a628da85eca4a52df1596b&sub=wcp1579073093657123)
* [如何学习Java的NIO](http://note.youdao.com/noteshare?id=5ea48ae4fd97f7a7bb4bd9d036ba4d11)
* [一篇文章读懂阻塞，非阻塞，同步，异步](https://www.jianshu.com/p/b8203d46895c)
  * 结合代码来理解，这篇文章提到了底层的原理，但是不够深入
  * [https://blog.csdn.net/qq\_41936805/article/details/94675873](https://blog.csdn.net/qq_41936805/article/details/94675873) 帮助理解多路复用
  * [http://www.imooc.com/article/255865](http://www.imooc.com/article/255865) 有代码可以看下
* [操作系统IO处理过程](https://blog.csdn.net/hutongling/article/details/69944456)
* [Java IO层次体系结构](https://blog.csdn.net/qq_21870555/article/details/82999195)
* [IO多路复用](https://www.jianshu.com/p/397449cadc9a)

### 学习笔记

NIO是同步的IO，是因为程序需要IO操作时，必须获得了IO权限后亲自进行IO操作才能进行下一步操作。AIO是对NIO的改进（所以AIO又叫NIO.2），它是基于Proactor模型的。每个socket连接在事件分离器注册 IO完成事件 和 IO完成事件处理器。程序需要进行IO时，向分离器发出IO请求并把所用的Buffer区域告知分离器，分离器通知操作系统进行IO操作，操作系统自己不断尝试获取IO权限并进行IO操作（数据保存在Buffer区），操作完成后通知分离器；分离器检测到 IO完成事件，则激活 IO完成事件处理器，处理器会通知程序说“IO已完成”，程序知道后就直接从Buffer区进行数据的读写。

也就是说：AIO是发出IO请求后，由操作系统自己去获取IO权限并进行IO操作；NIO则是发出IO请求后，由线程不断尝试获取IO权限，获取到后通知应用程序自己进行IO操作。 同步/异步：数据如果尚未就绪，是否需要等待数据结果。 阻塞/非阻塞：进程/线程需要操作的数据如果尚未就绪，是否妨碍了当前进程/线程的后续操作。应用程序的调用是否立即返回！ NIO与BIO最大的区别是 BIO是面向流的，而NIO是面向Buffer的。

Buffer是一块连续的内存块,是 NIO 数据读或写的中转地。 为什么说NIO是基于缓冲区的IO方式呢？因为，当一个链接建立完成后，IO的数据未必会马上到达，为了当数据到达时能够正确完成IO操作，在BIO（阻塞IO）中，等待IO的线程必须被阻塞，以全天候地执行IO操作。为了解决这种IO方式低效的问题，引入了缓冲区的概念，当数据到达时，可以预先被写入缓冲区，再由缓冲区交给线程，因此线程无需阻塞地等待IO。

缓冲区实际上是一个容器对象，更直接的说，其实就是一个数组，在NIO 库中，所有数据都是用缓冲区处理的。在读取数据时，它是直接读到缓冲区中的； 在写入数据时，它也是写入到缓冲区中的；任何时候访问NIO 中的数据，都是将它放到缓冲区中。而在面向流I/O 系统中，所有数据都是直接写入或者直接将数据读取到Stream 对象中。在NIO 中，所有的缓冲区类型都继承于抽象类Buffer，最常用的就是ByteBuffer

## Java IO中常用的类

```text
整个Java IO包中最重要的就是5个类和一个接口。
5个类指：
    * File：用于文件或者目录的描述信息，例如生成新的目录，修改文件名，删除文件，判断文件，过滤文件等
    * OutputStream：抽象类，基于字节的输出操作，是所有输出流的父类。
    * InputStream：抽象类，基于字节的输入操作，是所有输入流的父类。
    * Writer：抽象类，基于字符的输出操作。
    * Reader：抽象类，基于字符的输入操作。
一个接口指：Serializable
另外一个特殊的类：RandomAccessFile：随机文件操作，可以从文件任意位置进行存取（输入输出）操作。
```

`IO接口和类的结构图可参考技术栈图`

### RandomAccessFile

```text
我们在对文件的操作过程中，除了使用字节流和字符流的方式之外，我们还可以使用RandomAcessFile这个工具类来实现。
RandomAccessFile可以实现对文件的读 和 写，但是他并不是继承于以上4中基本虚拟类。
而且在对文件的操作中，RandomAccessFile有一个巨大的优势，他可以支持文件的随机访问，程序快可以直接跳转到文件的任意地方来读写数据。所以如果需要访问文件的部分内容，而不是把文件从头读到尾，使用RandomAccessFile将是更好的选择。
RandomAccessFile的方法虽然多，但它有一个最大的局限，就是只能读写文件，不能读写其他IO节点。
RandomAccessFile的一个重要使用场景就是网络请求中的多线程下载及断点续传。
```

## 字符与字节

Java中有输入和输出两种IO流，每种输入输出流又分为字节流和字符流两大类。

* 关于字节：每个字节（byte）有8bit组成
* 关于字符：一个字符代表一个英文字母或一个汉字

### 字符与字节的关系

Java采用unicode编码，2个字节表示1个字符

## 总结

* 先进先出，最先写入输出流的数据最先被输入流读取到
* 顺序读取，不能随机访问数据（RandomAccessFile除外）
* 只读只写，每个流只能是输入流或输出流的一种
* 每次进行IO操作，要手动close，因为IO资源并不属于内存资源，并不会被GC回收
* 对于输出操作，flush\(\)会刷新输出流，强制缓冲区中的输出字节被写出; close\(\)关闭输出流，释放和这个流相关的系统资源，调用close\(\)会自动flush
* 流结束的判断：方法read\(\)的返回值为-1时；readLine\(\)的返回值为null时
* 节流没有缓冲区，是直接输出的，而字符流是输出到缓冲区的。因此在输出时，字节流不调用colse\(\)方法时，信息已经输出了，而字符流只有在调用close\(\)方法关闭缓冲区时，信息才输出。要想字符流在未关闭时输出信息，则需要手动调用flush\(\)方法
* 字节流与字符流区别
  * 字节流以字节（8bit）为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节
  * 字节流能处理所有类型的数据（如图片、avi等），而字符流只能处理字符类型的数据
  * 只要是处理纯文本数据，就优先考虑使用字符流。除此之外都使用字节流

## Java IO与IO的区别和比较

## NIO

传统的 Socket 阻塞模式直接导致每个 Socket 都必须绑定一个线程来操作数据，参与通信的任意一方如果处理数据的速度较慢，则都会直接拖累另一方，导致另一方的线程不得不浪费大量的时间在 I/O 等待上，所以，每个 Socket 要绑定一个单独的线程正是传统Socket 阻塞模式的根本“缺陷”。之所以这里加了“缺陷”两个字，是因为这种模式在一些特定场合下效果是最好的，比如只有少量的 TCP 连接通信，双方都非常快速地传输数据，此时这种模式的性能最高。

现在我们可以开始分析“非阻塞”模式了，它就是要解决 I/O 线程与 Socket 解耦的问题，因此，它引入了事件机制来达到解耦的目的。我们可以认为 NIO 底层中存在一个 I/O 调度线程，它不断扫描每个 Socket 的缓冲区，当发现写入缓冲区为空（或者不满）的时候，它会产生一个Socket 可写事件，此时程序就可以把数据写入 Socket 里，如果一次写不完，则等待下次可写事件的通知；而当发现读取缓冲区里有数据的时候，它会产生一个 Socket 可读事件，程序收到这个通知事件时，就可以从 Socket 读取数据了。

内核空间、用户空间、计算机体系结构、计算机组成原理、…… 确实有点儿深奥。

我的新书《代码之谜》会有专门的章节讲解相关知识，现在写个简短的科普文：

就速度来说 CPU &gt; 内存 &gt; 硬盘

```text
I- 就是从硬盘到内存
O- 就是从内存到硬盘
```

第一种方式：我从硬盘读取数据，然后程序一直等，数据读完后，继续操作。这种方式是最简单的，叫阻塞IO。

第二种方式：我从硬盘读取数据，然后程序继续向下执行，等数据读取完后，通知当前程序（对硬件来说叫中断，对程序来说叫回调），然后此程序可以立即处理数据，也可以执行完当前操作在读取数据。

在一起的 Java IO 中，都是阻塞式 IO，NIO 引入了非阻塞式 IO。

还有一种就是同步 IO 和异步 IO。经常说的一个术语就是“异步非阻塞”，好象异步和非阻塞是同一回事，这大概是一个误区吧。

至于 Java NIO 的 Selector，在旧的 Java IO 系统中，是基于 Stream 的，即“流”，流式 IO。

当程序从硬盘往内存读取数据的时候，操作系统使用了 2 个“小伎俩”来提高性能，那就是预读，如果我读取了第一扇区的第三磁道的内容，那么你很有可能也会使用第二磁道和第四磁道的内容，所以操作系统会把附近磁道的内容提前读取出来，放在内存中，即缓存。

（PS：以上过程简化了）

通过上面可以看到，操作系统是按块 Block从硬盘拿数据，就如同一个大脸盆，一下子就放入了一盆水。但是，当 Java 使用的时候，旧的 IO 确实基于 流 Stream的，也就是虽然操作系统给我了一脸盆水，但是我得用吸管慢慢喝。

于是，NIO 横空出世。

#### 总结

Java中的IO/NIO：多路复用 IO 模型 1、多路复用 IO 模型是目前使用得比较多的模型。Java NIO 实际上就是多路复用 IO。在多路复用 IO 模型中，会有一个线程不断去轮询多个 socket 的状态，只有当 socket 真正有读写事件时，才真正调用实际的 IO 读写操作。因为在多路复用 IO 模型中，只需要使用一个线程就可以管理多个socket，系统不需要建立新的进程或者线程，也不必维护这些线程和进程，并且只有在真正有 socket 读写事件进行时，才会使用 IO 资源，所以它大大减少了资源占用。在 Java NIO 中，是通过 selector.select\(\)去查询每个通道是否有到达事件，如果没有事件，则一直阻塞在那里，因此这种方式会导致用户线程的阻塞。多路复用 IO 模式，通过一个线程就可以管理多个 socket，只有当 socket 真正有读写事件发生才会占用资源来进行实际的读写操作。因此，多路复用 IO 比较适合连接数比较多的情况。 2、另外多路复用 IO 为何比非阻塞 IO 模型的效率高是因为在非阻塞 IO 中，不断地询问 socket 状态时通过用户线程去进行的，而在多路复用 IO 中，轮询每个 socket 状态是内核在进行的，这个效率要比用户线程要高的多。 3、不过要注意的是，多路复用 IO 模型是通过轮询的方式来检测是否有事件到达，并且对到达的事件逐一进行响应。因此对于多路复用 IO 模型来说， 一旦事件响应体很大，那么就会导致后续的事件迟迟得不到处理，并且会影响新的事件轮询。 I/O复用是多路复用，这里的多路是指N个连接，每一个连接对应一个channel，或者说多路就是多个channel。复用，是指多个连接复用了一个线程或者少量线程\(在Tomcat中是Math.min\(2,Runtime.getRuntime\(\).availableProcessors\(\)\)\)。

## Reference

* [Java IO详解](https://www.jianshu.com/p/aea76bc0e6d1)
* [深入理解Java中的IO](https://www.cnblogs.com/ylspace/p/8128112.html)
* [Java IO与IO的区别和比较](https://blog.51cto.com/825272560/2059144)

