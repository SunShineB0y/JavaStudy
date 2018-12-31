# Redis #
Redis是一个基于内存的高性能的key-value数据库系统，整个系统加载在内存中进行操作，定期通过异步操作把数据存储到磁盘中。

## Redis的好处 ##

1. 读写速度快，因为数据存储在内存中
2. 支持多种类型的数据结构，如List、String、Set、SortedSet(Zset)、hash
3. 支持事务，所有的操作都是原子性的，要么成功执行，要么失败完全不执行
4. 特性丰富，可用于消息队列，缓存等，可按key设置expire过期时间，到期后自动删除。

## 使用场景 ##
1. 缓存，从缓存中读取数据更快
2. redis的list和set结构，可以让其做消息队列来使用。
## 如何保证Redis中的数据都是热点数据？##
在Redis内存数据集达到一定大小的时候，就会施行数据淘汰策略。redis提供6种数据淘汰策略，可以选择在已设置过期时间的数据里选择最近最少使用的数据进行淘汰的策略。

另外两种数据淘汰策略：随机选择一部分key释放；在最近最少使用的数据中选择一部分key进行释放。

## Redis持久化 ##
持久化指的是将数据写入硬盘

两种持久化方式：RDB(默认)、AOF

RDB持久化：每次持久化都是将数据完整写入到硬盘一次（写入rdb文件中），并不只是增量地只同步脏数据

AOF持久化：通过保存对redis服务端的写命令来记录数据库状态（命令写入aof文件中），redis服务重启时会使用AOF文件来还原数据库。


区别：

RDB持久化方式：方便备份，恢复大数据集时比AOF要快，但是会丢失部分数据

AOF持久化方式：AOF文件会过大。
## 分布式 ##

Redis支持主从的模式。原则：Master会将数据同步到slave，而slave不会将数据同步到master。Slave启动时会连接master来同步数据。


这是一个典型的分布式读写分离模型。我们可以利用master来插入数据，slave提供检索服务。这样可以有效减少单个机器的并发访问数量。

## Redis缓存击穿、失效及热点key问题 ##

[https://www.cnblogs.com/peteremperor/p/7342119.html](https://www.cnblogs.com/peteremperor/p/7342119.html)


## Redis常见面试问题 ##

[https://mp.weixin.qq.com/s/lx3ReEJ1cywac-o9Jt3Kyg](https://mp.weixin.qq.com/s/lx3ReEJ1cywac-o9Jt3Kyg)

[https://mp.weixin.qq.com/s/QeJOyNCg5z7uwNXDrLkHww](https://mp.weixin.qq.com/s/QeJOyNCg5z7uwNXDrLkHww)

## Redis进阶 ##

[https://mp.weixin.qq.com/s/pkGHXNCqGVbVofKwI2yP4A](https://mp.weixin.qq.com/s/pkGHXNCqGVbVofKwI2yP4A)