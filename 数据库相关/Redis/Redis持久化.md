## Redis持久化 ##

Redis所有数据保存在内存中，对数据的更新将异步的保存到磁盘上。Redis的持久化方式有两种，即rdb持久化和aof持久化。

### RDB持久化 ###
RDB持久化是通过命令将redis中的数据完整的生成一个快照，并保存到硬盘中（即rdb文件，rdb是一个二进制文件）。当需要对Redis进行恢复，如重启时，redis会加载rdb文件进行数据恢复。

rdb文件也是一个复制的媒介，当进行主从复制的时候也会用到rdb持久化方式。

### RDB持久化的触发机制 ###

1. save 
    save是一条同步命令，在redis执行save命令时会阻塞其它命令的执行。redis客户端每次执行save命令就会生成一个rdb文件，替换老的rdb文件，当数据量比较大时，会阻塞其他客户端的命令。

2. bgsave
    bgsave即background save是一条异步命令，redis客户端会调用Linux的fork()函数创建一个子进程去执行这个命令，当子进程生成rdb文件成功后会通知主进程。fork()函数会执行的比较快，大多数情况下不会阻塞redis主进程。bgsave方式由于需要fork，所以会额外消耗内存。

3. 自动
    可在redis配置文件中配置save命令并指定参数来打开redis的自动RDB持久化。可自定义参数值。这种rdb持久化的方式也是通过bgsave的方式。注意：save自动配置每满足任一条件就会进行持久化。

        # redis配置文件中save命令的默认配置 
        save 900 1  # 900s内有1个key发生改变则进行持久化
        save 300 10  #300s内有10个key发生改变则进行持久化
        save 60 10000  #60s内有10000个key发生改变则进行持久化

        dbfilename dump.dmb  #rdb文件名，默认是dump.rdb
        dir ./ #指定rdb文件的路径，默认是当前路径
        stop-writes-on-bgsave-error  yes  #当bgsave发生错误时是否停止写入
        rdbcompression yes  #rdb文件是否采用压缩的格式，默认yes
        rdbchecksum yes  #对rdb数据进行校验，默认yes

*一些不容忽略的触发机制*

- 全量复制 
  主从复制的时候候会全量复制数据，也会生成rdb文件

- debug reload
  debug级别的重启，不需要将内存中的数据清空的重启，也会触发rdb文件生成

- shutdown 
  可指定参数save或nosave，也会生成rdb文件

### RDB持久化的缺点 ###
1. 耗时、耗性能（fork会消耗内存，数据写到硬盘会有IO消耗）
2. 不可控、可能丢失数据（会丢失未及时持久化的数据）

### AOF持久化 ###
AOF持久化是把Redis客户端的每条写命令记录到aof文件中，每写入一条命令都会记录到aof文件中。 redis宕机后可通过aof文件对数据进行恢复。Reis执行写命令是将写命令写入到缓冲区中，缓冲区再根据一些策略刷到磁盘中。

### AOF持久化的三种策略 ###

    Redis用appendfsync来配置aof持久化策略
    appendfsync always  # 每条命令都会从缓冲区fsync到硬盘中
    appendfsync everysec  # 每秒种进行一次刷新，把命令从缓冲区fsync到硬盘中
    appendfsync no  # 由操作系统决定fsync

always不会丢失数据，但IO开销比较大；everysec每秒进行一次fsync，会丢失一秒钟的数据；no 方式则不可管、不可控

### AOF重写 ###
指把过期的、没有用的、重复的以及一些可以优化的命令进行化简，从而可以减小磁盘占用量，加快恢复速度

-  AOF重写的两种实现方式
        1. bgrewriteaof   类似于rdb持久化的bgsave方式，也会fork出子进程
        2. aof重写配置
    
        auto-aof-rewrite-min-size   # aof文件重写需要的尺寸
        auto-aof-rewrite-persentage   # aof文件增长率
        aof_current_size   # aof当前尺寸（单位：字节）
        aof_base_size   # aof上次启动和重写的尺寸（单位：字节）

- aof重写自动触发时机 （同时满足）
        1. aof_current_size   >  auto-aof-rewrite-min-size
        2. (aof_current_size  -   aof_base_size) / aof_base_size  >  auto-aof-rewrite-persentage

### AOF持久化配置 ###
    appendonly yes  # 打开aof持久化，默认是no
    appendfilename "appedonly-${port}.aof"  # aof文件名
    appendfsync everysec  # aof持久化的策略
    dir ./  # 指定aof文件的位置
    no-appendfsync-on-rewirte yes  # 在aof重写时，是否做aof的append操作
    auto-aof-rewrite-persentage  100
    auto-aof-rewrite-min-size  64mb

### RDB和AOF的对比
  启动优先级：RDB低，AOF高
体积：RDB小，AOF大
恢复速度：RDB快，AOF慢
数据安全性：RDB会丢数据，AOF根据策略决定
轻重：RDB重，AOF轻

#### RDB最佳策略 ####
1. 建议关掉（其实是无法真正的关掉，因为主从复制必然通过生成rdb文件的方式来进行）
2. 集中管理
3. 从节点打开，为的是保存一份rdb文件

#### AOF最佳策略 ####
1. 打开
2. AOF重写集中管理
3. 选择everysec策略