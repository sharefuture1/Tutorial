# database

## 学习计划

* [数据库主键怎么选择](mysql/shu-ju-ku-zhu-jian-zen-mo-xuan-ze.md)
* [数据库中的索引](mysql/mysql-suo-yin.md)
* [数据库性能优化](database.md#数据库性能优化)

### 影响数据库性能的因素

* 磁盘数据库，内存数据库，分布式内存数据库
* Apache Ignite
* 事务的特性
  * 原子性（Atomicity）：（同生共死）一个事务必须被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说不可能只执行其中的一部分操作。（银行转账的例子）数据库是如何保证原子性的呢？
  * 一致性（Consistency）：每个事务都必须保留数据库的完整性约束（已声明的一致性规则）。事务将数据库从一种正确的状态装换到另外一种正确的状态。这个是最难理解的，什么是正确的状态？
  * 隔离性（Isolation）：隔离性要求一个事务对数据库中数据的修改，在未提交完成前对其它事务是不可见的。四种事务隔离级别：
    * 未提交读：事务可以读取未提交的数据，称之为脏读（Read Uncommited）
    * 已提交读：大多数数据库（SQL Server/Postgresql）默认隔离级别，mysql不是这个。一个事务开始时只能看到已提交事务做的修改，或者说一个事务在开始提交到结束是不可见的（Read Commited，也叫不可重复读）
    * 可重复读：在同一个事务内，多次读取的结果是一致的，什么意思呢，就相当于在这个你的这个事务在开启那刻切出来了一个数据库的镜像，不管在这个事务执行中这个数据库发生了什么，或者有数据的修改，你都感知不到。举个例子，你在事务A中多次查询t表中的结果，然后在事务B中插入了一条新数据到t中，再次在事务A中查询，返回的结果和最开始得到的结果是一样的（Repeatable Read，mysql默认的级别）
    * 可串行化（Serializable）：最高的隔离级别，在读取的每一行上都加锁，会造成大量的锁争用和超时，很少用，除非严格的数据一致性，或者并发很少的情况下使用。
    * 隔离级别由低到高，并发性由高到低。InnoDB的默认隔离级别是可重复读，而不是已提交多
  * 持久性（Durability）：一旦事务提交，则其所做的修改就会永久保存到数据库中，此时即使系统崩溃，已提交的修改数据也不会丢失
  * 事务的ACID不是一个平级的关系
    * 只有满足一致性，事务的执行结果才是正确的；在无并发的情况下，事务串行执行，隔离性一定能够满足。此时只要能满足原子性，就一定能满足一致性。在并发的情况下，多个事务并发执行，事务不仅要满足原子性，还要满足隔离性，这样才能保证一致性；事务满足持久化是为了能应对数据库崩溃的情况。

      > 事务一致性详解
  * 首先说下事务的产生原因，其实是为了当应用程序访问数据库的时候,事务能够简化我们的编程模型,不需要我们去考虑各种各样的潜在错误和并发问题.可以想一下当我们使用事务时,要么提交,要么回滚,我们不会去考虑网络异常了,服务器宕机了,同时更改一个数据怎么办对吧?因此事务本质上是为了应用层服务的.而不是伴随着数据库系统天生就有的.
  * ACID里的AID都是数据库的特征,也就是依赖数据库的具体实现.而唯独这个C,实际上它依赖于应用层,也就是依赖于开发者.这里的一致性是指系统从一个正确的状态,迁移到另一个正确的状态.什么叫正确的状态呢?就是当前的状态满足预定的约束就叫做正确的状态.而事务具备ACID里C的特性是说通过事务的AID来保证我们的一致性.
  * 这里我们举个大家都在说的财务系统的例子.
    * A要向B支付100元,而A的账户中只有90元,并且我们给定账户余额这一列的约束是,不能小于0.那么很明显这条事务执行会失败,因为90-100=-10,小于我们给定的约束了.这个例子里,支付之前我们数据库里的数据都是符合约束的,但是如果事务执行成功了,我们的数据库数据就破坏约束了,因此事务不能成功,这里我们说事务提供了一致性的保证.
    * 然后我们再看个例子A要向B支付100元,而A的账户中只有90元,我们的账户余额列没有任何约束.但是我们业务上不允许账户余额小于0.因此支付完成后我们会检查A的账户余额,发现余额小于0了,于是我们进行了事务的回滚.这个例子里,如果事务执行成功,虽然没有破坏数据库的约束,但是破坏了我们应用层的约束.而事务的回滚保证了我们的约束,因此也可以说事务提供了一致性保证\(ps:事实上,是我们应用层利用事务回滚保证了我们的约束不被破坏\).
    * 最后我们再看个例子A要向B支付100元,而A的账户中只有90元,我们的账户余额列没有任何约束.然后支付成功了.这里,如果按照很多人的理解,事务不是保证一致性么?直观上账户余额为什么能为负呢.但这里事务执行前和执行后,我们的系统没有任何的约束被破坏.一直都是保持正确的状态.所以,综上.你可以理解一致性就是:应用系统从一个正确的状态到另一个正确的状态.而ACID就是说事务能够通过AID来保证这个C的过程.C是目的,AID都是手段.
* 什么影响了数据库性能？ 1. 服务器硬件：CPU/网络/磁盘
  * CPU
    * 如何选择CPU？计算密集型/存储密集型（亚马逊上选服务器的时候选），CPU的核数，频率。MySQL的版本，老版本对多核支持不太好。32位还是64位的CPU，现在默认的是64位。64位CPU上运行32位架构的操作系统。
    * 对于并发比较高的场景CPU的数量比频率重要；对于CPU密集型场景和复杂SQL则频率越高越好。两者都是的话，CPU核数多且频率高越贵哦
  * 内存：选择服务器所支持最高的频率
    * MyLSam: 索引缓存到内存中，数据通过操作系统来缓存
    * InnoDB: 同时在缓存中存储索引和数据，提高效率
  * 磁盘：容量/传输速度/访问时间/主轴转速（7000/1.5W）/物理尺寸
    * 传统磁盘：便宜，效率低，读写慢。RAID0可以把一些小的磁盘构建磁盘阵列，但是数据可能会丢失；RAID1可以对磁盘做镜像，对数据安全性高，磁盘利用率低；RAID5通过分布式奇偶校验块把数据分散到多个磁盘上，如果任何一个盘数据丢失，都可以从奇偶校验块重建，如果都丢失是无法恢复的；RAID10分片镜像
    * SSD：相比机械硬盘有更好的随机读写性能；固态存储吞吐量大，更好支持并发；使用SATA接口（6G/S）
    * PCI-E SSD需要特殊的接口和独立的驱动，闪存技术
    * SAN: Storage Area Network 通过光纤接入服务器。适合做数据库备份
    * NAS：Network-Attached Storage 使用网络连接，使用NFS或SMB协议来访问
  * 网络接口性能对数据库的影响：延迟、带宽（也叫吞吐量）、网络的质量（抖动、丢包）；根据需求使用万兆交换机；尽可能多网络隔离，不暴露数据库在外网上
    1. 操作系统
  * windows对schema大小写不敏感，linux对大小写敏感。建议数据库/表都小写，通过配置参数来强制要求小写也可以。
  * centos优化
    * 内核相关参数 /etc/sysctl.conf
      * net.core.somaxconn=65535 每个端口监听的个数
      * net.core.netdev\_max\_backlog=65535
      * net.ipv4.tcp\_max\_syn\_backlog=65535
      * net.ipv4.tcp\_fin\_timeout=10 TCP等待时间加速回收速度 net.ipv4.tcp\_tw\_reuse=1 net.ipv4.tcp\_tw\_recycle=1
      * net.ipv4.tcp\_keepalive\_time=120秒
    * 增加资源限制
      * /etc/security/limit.conf 打开文件数的限制，添加到limit.conf文件的末尾就可以了
        * soft nofile 65535
        * hard nofile 65535
    * 磁盘调度策略
    * 文件系统对性能的影响
      * Linux文件系统有EXT3/EXT4/XFS都有日志功能，传说XFS性能高
      * Windows文件系统有FAT/NTFS
    * 数据库存储引擎
  * MyISAM：不支持事务，表级锁
  * InoDB: 事务存储引擎，完美支持行级锁，事务ACID特性
    1. 数据库参数配置
    2. 数据库表结构设计和SQL语句对性能影响
  * 慢查询

### 数据库性能优化

1. 数据表的设计
2. SQL语句的设计
3. 数据库设计

## 学习笔记

* [分库分表 如何做到永不迁移数据和避免热点](https://github.com/zhonghuasheng/Tutorial/wiki/%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-%E5%A6%82%E4%BD%95%E5%81%9A%E5%88%B0%E6%B0%B8%E4%B8%8D%E8%BF%81%E7%A7%BB%E6%95%B0%E6%8D%AE%E5%92%8C%E9%81%BF%E5%85%8D%E7%83%AD%E7%82%B9)

## 百问

1. Select count\(1\) 和count\(col\),count\(\*\)区别
2. 千万级别的数据，如何更改表字段长度？

   今天遇到一个场景，有一个表创建时间比较长了，现在有3亿的数据，其中有个字段A是varchar\(255\)，现在呢有个新需求，要求A中能存500个长度的数据。直接改A表字段，肯定不行，预计这个表修改需要执行1个多小时。我试了下，弄张新表B，然后修改A字段长度，然后同步A的数据到B，估计也得需要1个小时。你不可能在线上直接改啊，采用的方案是另起一张新表一个id字段指向原表的id，然后加个A字段

