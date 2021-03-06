- `jvm运行时数据库区`
> 程序计数器、方法栈/本地方法栈、方法区（运行时常量池）、堆

- `垃圾回收算法`
> 标记-清除、标记整理、复制算法、分代收集

- `jvm内存模型(java memory model JMM)`
> [相关参考](https://blog.csdn.net/javazejian/article/details/72772461)
> JMM描述的是一组规则，通过这组规则控制程序中各个变量在共享数据区域和私有数据区域的访问方式
> JMM是围绕着程序执行的原子性、有序性、可见性展开的

- `内存溢出和内存泄漏的区别`
> `内存溢出`out of memory，是指程序在为自身申请内存时，没有足够的内存空间供自己使用，出现out of memory
> `内存泄露`memory leak，是指程序在申请内存后，无法释放已经申请到的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光。

> memory leak会最终会导致out of memory！内存泄漏是指你向系统申请分配内存进行使用(new)，可是使用完了以后却不归还(delete)，结果你申请到的那块内存你自己也不能再访问（也许你把它的地址给弄丢了），而系统也不能再次将它分配给需要的程序。一个盘子用尽各种方法只能装4个果子，你装了5个，结果掉倒地上不能吃了。这就是溢出！比方说栈，栈满时再做进栈必定产生空间溢出，叫上溢，栈空时再做退栈也产生空间溢出，称为下溢。就是分配的内存不足以放下数据项序列,称为内存溢出.

- `select、poll、epoll之间的区别? 底层的数据结构是什么？`
> [相关参考](https://www.jianshu.com/p/de0121046a1f)

- `mysql数据库默认存储引擎，有什么优点`
> InnoDB
> InnoDB是MySQL存储引擎中唯一支持事务的。
> 同时引入了行级锁定、读取过程不产生锁以及外键约束。
> 基于磁盘存储，并将记录按页的方式进行管理
> 只有InnoDB支持外键约束。

- `优化数据库的方法，从sql到缓存到cpu到操作系统`
> [相关参考](http://blog.51cto.com/yangshufan/2168952)

- `什么情景下做分表，什么情景下做分库`
> [相关参考](https://blog.csdn.net/sunhuiliang85/article/details/78418004)

- `linkedList与arrayList区别 适用场景`
> arrayList 数据结构为数组，linkedList 数据结构为双向链表
> 适用场景：数组查询某个值快，链表添加、删除快

- `array list是如何扩容的`
> [相关参考](https://juejin.im/post/59dd70966fb9a0452845741e)
> 进行逻辑校验，capacity不够时扩容1.5倍。

- `volatile 关键字的作用？Java 内存模型？`
> 保证被volatile修饰的共享变量对所有线程总是可见的，也就是当一个线程修改了一个被volatile修饰共享变量的值，新值总数可以被其他线程立即得知。
> 禁止指令重排序优化。

- `java lock的实现，公平锁、非公平锁`

- `悲观锁和乐观锁，应用中的案例，mysql当中怎么实现，java中的实现`
> [相关参考](https://www.jianshu.com/p/f5ff017db62a)

- `Java 内存分配策略`
> - 对象优先在Eden区分配
> - 大对象直接进入老年代
> - 长期存活的对象将进入老年代
> - 动态对象年龄判定
> - 空间分配担保

- `多个线程同时请求内存，如何分配`

- `Redis 底层用到了哪些数据结构？`
> [相关参考](https://www.cnblogs.com/jaycekon/p/6227442.html)

- `使用 Redis 的 set 来做过什么？Redis 使用过程中遇到什么问题？搭建过 Redis 集群吗？`

- `如何分析“慢查询”日志进行 SQL/索引 优化？`
- `MySQL 索引结构解释一下？（B+ 树）MySQL Hash 索引适用情况？`
- `面如何保证数据库与redis缓存一致的Redis 的并发竞争问题是什么？如何解决这个问题？了解 Redis 事务的 CAS 方案吗？如何保证 Redis 高并发、高可用？Redis 的主从复制原理，以及Redis 的哨兵原理？如果让你写一个消息队列，该如何进行架构设计啊？说一下你的思路。MySQL数据库主从同步怎么实现`
