## Redis持久化 ##

Redis所有数据保存在内存中，对数据的更新将异步的保存到磁盘上。Redis的持久化方式有两种，即rdb持久化和aof持久化，其中rdb持久化是通过

### RDB持久化 ###
RDB持久化是通过命令将redis中的数据完整的生成一个快照，并保存到硬盘中（即rdb文件，rdb是一个二进制文件）。当需要对Redis进行恢复，如重启时，redis会加载rdb文件进行数据恢复。

rdb文件也是一个复制的媒介，当进行主从复制的时候也会用到rdb持久化方式。

### RDB持久化的触发机制 ###

1. save 
    save是一条同步命令，在redis执行save命令时会阻塞其它命令的执行。redis客户端每次执行save命令就会生成一个rdb文件，替换老的rdb文件，当数据量比较大时，会阻塞其他客户端的命令。

2. bgsave
    bgsave即background save是一条异步命令，redis客户端会调用Linux的fork()函数创建一个子进程去执行这个命令，当子进程生成rdb文件成功后会通知主进程。fork()函数会执行的比较快，大多数情况下不会阻塞redis主进程。bgsave方式由于需要fork，所以会额外消耗内存。

3. 自动