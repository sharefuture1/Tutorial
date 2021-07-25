# system

## OS的PageCache机制

PageCache是OS对文件的缓存，用于加速对文件的读写。一般来说，程序对文件进行顺序读写的速度几乎接近于内存的读写访问，这里的主要原因就是在于OS使用PageCache机制对读写访问操作进行了性能优化，将一部分的内存用作PageCache 1、对于数据文件的读取

如果一次读取文件时出现未命中（cache miss）PageCache的情况，OS从物理磁盘上访问读取文件的同时，会顺序对其他相邻块的数据文件进行预读取（ps：顺序读入紧随其后的少数几个页面）。这样，只要下次访问的文件已经被加载至PageCache时，读取操作的速度基本等于访问内存 1、对于数据文件的写入

OS会先写入至Cache内，随后通过异步的方式由pdflush内核线程将Cache内的数据刷盘至物理磁盘上 对于文件的顺序读写操作来说，读和写的区域都在OS的PageCache内，此时读写性能接近于内存。RocketMQ的大致做法是，将数据文件映射到OS的虚拟内存中（通过JDK NIO的MappedByteBuffer），写消息的时候首先写入PageCache，并通过异步刷盘的方式将消息批量的做持久化（同时也支持同步刷盘）；订阅消费消息时（对CommitLog操作是随机读取），由于PageCache的局部性热点原理且整体情况下还是从旧到新的有序读，因此大部分情况下消息还是可以直接从Page Cache（cache hit）中读取，不会产生太多的缺页（Page Fault）中断而从磁盘读取PageCache机制也不是完全无缺点的，当遇到OS进行脏页回写，内存回收，内存swap等情况时，就会引起较大的消息读写延迟 对于这些情况，RocketMQ采用了多种优化技术，比如内存预分配，文件预热，mlock系统调用等，来保证在最大可能地发挥PageCache机制优点的同时，尽可能地减少其缺点带来的消息读写延迟.

## 引用

* [https://my.oschina.net/u/3180962/blog/3064148](https://my.oschina.net/u/3180962/blog/3064148)

