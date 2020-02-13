# Redis #
Redis是一个基于内存的高性能的key-value数据库系统，整个系统加载在内存中进行操作，定期通过异步操作把数据存储到磁盘中。

## Redis的可执行文件 ##
redis-server是Redis服务器，redis-cli是Redis命令行客户端，
redis-benchmark是Redis提供的性能测试工具，redis-check-aof是Redis的aof文件修复工具，
redis-check-dump是Redis的rdb文件修复工具，redis-sentinel可用于起动Redis的sentinal节点

## Redis三种启动方式 ##
1. 最简启动
执行`redis-server`，这种方式使用的是redis默认的配置文件

2. 动态参数启动
执行`redis-server --port 6380`，可通过这种方式指定参数来启动

3. 配置文件启动
执行`redis-server redis.conf`，通过这种方式指定特定的配置文件，可在配置文件中自定义一些参数的值

## Redis的优点 ##

1. 读写速度快，因为数据存储在内存中
2. 支持多种类型的数据结构，如List、String、Set、SortedSet(Zset)、hash
3. 支持事务，所有的操作都是原子性的，要么成功执行，要么失败完全不执行
4. 特性丰富，可用于消息队列，缓存等，可按key设置expire过期时间，到期后自动删除。

## 使用场景 ##
1. 缓存，从缓存中读取数据更快
2. redis的list和set结构，可以让其做消息队列来使用。

## 如何保证Redis中的数据都是热点数据？
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

## Redis事务 ##
在Redis中，MULTI/EXEC/DISCARD/WATCH这四个命令是我们实现事务的基石。

1. 在事务中的所有命令都将会被串行化的顺序执行，事务执行期间，Redis不会再为其它客户端的请求提供任何服务，
从而保证了事物中的所有命令被原子的执行。

2. 和关系型数据库中的事务相比，在Redis事务中如果有某一条命令执行失败，其后的命令仍然会被继续执行。

3. MULTI命令可以开启一个事务，类似于“BEGIN TRANSACTION”。在该语句之后执行的命令都将被视为事务之内的操作，
最后我们可以通过执行EXEC/DISCARD命令来提交/回滚该事务内的所有操作。
这两个Redis命令可被视为等同于关系型数据库中的COMMIT/ROLLBACK语句。

4. 在事务开启之前，如果客户端与服务器之间出现通讯故障并导致网络断开，其后所有待执行的语句都将不会被服务器执行。
然而如果网络中断事件是发生在客户端执行EXEC命令之后，那么该事务中的所有命令都会被服务器执行。

## Redis主从复制 ##

Redis支持主从的模式。原则：Master会将数据同步到slave，而slave不会将数据同步到master。Slave启动时会连接master来同步数据。

1. 同一个master可以连接多个slave

2. Slave同样可以接受其它Slaves的连接和同步请求，可以有效的分载Master的同步压力，所以可以将Redis的Replication架构视为图结构。

3. Master Server是以非阻塞的方式为Slaves提供服务，所以在Master-Slave同步期间，客户端仍然可以提交查询或修改请求。

4. Slave Server同样是以非阻塞的方式完成数据同步，在同步期间，如果有客户端提交查询请求，Redis则返回同步之前的数据。

5. Master可以将数据保存操作交给Slaves完成，从而避免了在Master中要有独立的进程来完成此操作。

6. 当一个slave库取消主从复制时，之前同步过的数据不会被清除；当这个库再被设置为slave时，会清空数据并进行同步

这是一个典型的分布式读写分离模型。我们可以利用master来插入数据，slave提供检索服务。这样可以有效减少单个机器的并发访问数量。

#### 配置主从复制 ####
1.通过命令

    # 把6380设置成同一主机上6379的slave，在slave 上执行
    slaveof 127.0.0.1 6379   # 即 slaveof  ip  port
    # 若想取消该slave的主从复制，需要在slave执行
    slaveof no one    # 该命令不会清除之前同步过的数据

2.通过配置文件
通过配置文件这种方式需要重启redis服务器。

    slaveof <masterip> <masterport>  # 配置master
    masterauth <master-password>  # 如果master库配置了密码，需要用该配置认证master库的密码
    # 设置slave为只读，可避免slave写入数据，而master不能同步slave的数据，最终导致数据不一致
    slave-read-only yes 

#### 全量复制的开销 ####
1.主节点执行bgsave的时间
2.rdb文件网络传输时间
3.从节点清空数据的时间
4.从节点加载rdb的时间
5.从节点可能的aof重写时间


## Redis缓存击穿、失效及热点key问题 ##

[https://www.cnblogs.com/peteremperor/p/7342119.html](https://www.cnblogs.com/peteremperor/p/7342119.html)


## Redis常见面试问题 ##

[https://mp.weixin.qq.com/s/lx3ReEJ1cywac-o9Jt3Kyg](https://mp.weixin.qq.com/s/lx3ReEJ1cywac-o9Jt3Kyg)

[https://mp.weixin.qq.com/s/QeJOyNCg5z7uwNXDrLkHww](https://mp.weixin.qq.com/s/QeJOyNCg5z7uwNXDrLkHww)

## Redis进阶 ##

[https://mp.weixin.qq.com/s/pkGHXNCqGVbVofKwI2yP4A](https://mp.weixin.qq.com/s/pkGHXNCqGVbVofKwI2yP4A)
