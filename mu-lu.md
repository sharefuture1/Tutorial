# 目录

## Summary

#### 目录

| 英语 | Java | Spring生态 | 中间件 | 数据库 | 服务器 | 网络/系统 | 架构设计 | 内功 | 程序人生 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 英语 | Basic JVM Web  | Spring Spring Boot Spring Cloud | Keepalived ActiveMQ RabbitMQ RocketMQ MQTT Netty Mybatis Redis Log | MySQL Postgresql Mongodb | Tomcat Nginx | 网络 Linux | 原则 安全 高可用 扩展性 伸缩性 性能  | 数据结构 算法 设计模式 | 软文 规范 工具 面试集锦 实战训练 |

#### 英语

* [计算机行业常用英语积累](tool/english.md)

#### Java

* `JavaCore`:  [Java基础](java/basic/java-basic.md)

    [JDBC基础](https://github.com/zhonghuasheng/JAVA/blob/master/jdbc/src/main/java/com/zhonghuasheng/jdbc/learn01/BasicSteps.java)  [Java集合](java/basic/java-collection.md)  [Java多线程系列](java/basic/java-thread.md)  [JUC系列](java/basic/java-thread-juc.md)  [Java IO基础](java/basic/java-io-nio.md)

* `Java -VM`:  [Java虚拟机系列](java/jvm/深入理解Java虚拟机.md) [JVM虚拟机监控及性能调优系列](java/jvm/JVM虚拟机监控及性能调优.md)
* `Java-Web`:  [Servlet基础](java/javaweb/servlet.md) [JSP基础](java/javaweb/jsp.md)
* `Spring X`:  [Spring4基础知识系列](java/spring/spring.md#Spring) [SpringMVC基础知识系列](java/spring/spring.md#SpringMVC) [SpringBoot基础知识系列](java/spring/spring.md#SpringBoot)

#### 中间件

* `负载均衡`:  [Keepalived系列](plugins/keepalived.md)
* `消息通信`:  [消息通信基础](http://note.youdao.com/noteshare?id=30a11e46aaef3f00d2ecfb84692ca294&sub=wcp157828038663078) [MQ概述](plugins/mq/mq.md) [ActiveMQ系列](plugins/activemq.md) [RabbitMQ系列](plugins/rabbitmq.md)  [RocketMQ系列](plugins/rocketmq.md)  [Netty系列](plugins/netty.md)  [IOT通信](plugins/mqtt.md)
* `数据访问`:  [MyBatis](plugins/mybatis.md)  [MyBatis-Plus](plugins/mybatis-plus.md)
* `数据缓存`:  [Redis系列](plugins/redis.md)
* `搜索引擎`:  [Elasticsearch](plugins/elasticsearch.md)
* `日志模块`： [Log4j2](plugins/log.md)

#### 数据库

* `关系型数据库`:  [数据库理论基础](plugins/database/database.md) [MySQL](plugins/mysql.md) [Postgresql](plugins/postgresql.md)
* `非关系型数据库`:  [Mongodb学习笔记](plugins/mongodb.md)

#### 服务器

* [Tomcat服务器](plugins/tomcat.md) [Nginx反向代理服务器搭建](plugins/nginx.md) [Linux系统常用命令](tool/shell/linux.md)

#### 架构设计

* `设计原则`:  [系统设计注意事项](system/architecture/系统设计注意事项.md)
* `系统安全`:  [系统架构安全设计](system/architecture/系统架构安全设计.md)
* `高可用性`:  [系统架构高可用设计](system/architecture/系统架构高可用设计.md)
* `高扩展性`:  [系统架构扩展性设计](system/architecture/系统架构扩展性设计.md)
* `高伸缩性`:  [系统架构伸缩性设计](system/architecture/系统架构伸缩性设计.md)
* `系统性能`:  [系统架构性能设计](system/architecture/系统架构性能设计.md)
* `其他事项`:  [系统架构设计其他注意事项](system/architecture/系统架构设计其他注意事项.md)

#### 内功

* `设计模式`: [23种设计模式](system/algorithm/设计模式.md)
* `数据结构`: [数据结构系列](system/algorithm/数据结构.md)
* `算法`: [算法系列](system/algorithm/algorithm.md)
* `操作系统`: [操作系统](tool/shell/linux.md)

#### 网络

* `常见网络问题`: [常见网络问题系列](system/network/network.md)

#### 程序人生

> `软文`
>
> * [工作经历 - 记录自己的成长](tool/coding-life.md#记录自己的成长)
> * [最好的建议](tool/coding-life.md/#最好的建议)
> * [正视自己的价值](tool/coding-life.md/#正视自己的价值)
> * [新工程师要干的五件事情](tool/coding-life.md/#新工程师要干的五件事情)
> * [为什么CTO,技术总监，架构师不写代码都这么牛逼](http://note.youdao.com/noteshare?id=f4eeda7da9b73adf4294f984a5e7cbe5&sub=945D4467238947AAB5C030B17D5AC01E)
> * [技术/管理](https://www.cnblogs.com/yexiaochai/p/14805941.html#top)
>
> `规范`
>
> * [雅虎前端34条军规](http://note.youdao.com/noteshare?id=b59d0da4f7bb2b7ba5f73129d85b1ba1)
> * [Google Java Coding Style](https://google.github.io/styleguide/javaguide.html)
> * [阿里巴巴代码规范](https://github.com/alibaba/p3c/blob/master/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%EF%BC%88%E8%AF%A6%E5%B0%BD%E7%89%88%EF%BC%89.pdf)
> * [给函数取一个好的名字](http://note.youdao.com/noteshare?id=74f3c5fae9fc26473e7046a700cdad12&sub=wcp1581864078132689)
> * [Java命名规范参考](http://note.youdao.com/noteshare?id=c0ca7331624eb2f19b06f623a1b832ae&sub=2F7223EB9D9E4072B60A1FB578BF0AFA)
>
> `工具`
>
> * \[Java诊断工具\]
>   * [阿里JAVA诊断工具Arthas](tool/tools.md)
> * [API测试工具](tool/api-testing-tool.md)
> * [流量统计，网站分析](tool/common-tools.md)
> * [日志管理工具](tool/cronolog.md)
> * [Git](tool/git.md)
> * [Intellij](tool/intellij.md)
> * [Maven](tool/maven.md)
> * [VSCode](tool/vscode-settings.md)
> * CloudFlare免费的CDS服务
> * LDAP搭建和使用
> * [常见部署方式](tool/deployment.md)
>
> \`\`



