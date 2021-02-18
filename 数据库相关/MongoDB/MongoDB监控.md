## MongoDB监控 ##

MongoDB中提供了mongostat 和 mongotop 两个命令来监控MongoDB的运行情况。

### mongostat命令 ###

mongostat是mongodb自带的状态检测工具，在命令行下使用。它会间隔固定时间获取mongodb的当前运行状态，并输出。
如果你发现数据库突然变慢或者有其他问题的话，你第一手的操作就考虑采用mongostat来查看mongo的状态。

mongostat命令可实时（1秒钟刷新一次）显示Mongodb数据库的运行情况，可视为性能监视器。


    [root@localhost ~]# /data/mongodb-4.0.14/bin/mongostat
    insert query update delete getmore command dirty used flushes vsize  res qrw arw net_in net_out conn                time
        *0    *0     *0     *0       0     2|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   158b   66.1k    1 Feb 16 11:14:12.082
        *0    *0     *0     *0       0     2|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   158b   66.1k    1 Feb 16 11:14:13.081
        *0    *0     *0     *0       0     1|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   157b   65.9k    1 Feb 16 11:14:14.083
        *0    *0     *0     *0       0     2|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   158b   66.2k    1 Feb 16 11:14:15.081
        *0    *0     *0     *0       0     1|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   157b   66.0k    1 Feb 16 11:14:16.081
        *0    *0     *0     *0       0     1|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   157b   66.0k    1 Feb 16 11:14:17.081
        *0    *0     *0     *0       0     2|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   158b   66.1k    1 Feb 16 11:14:18.081
        *0    *0     *0     *0       0     1|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   157b   66.0k    1 Feb 16 11:14:19.081
        *0    *0     *0     *0       0     2|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   158b   66.1k    1 Feb 16 11:14:20.081
        *0    *0     *0     *0       0     1|0  0.0% 0.0%       0 1.08G 177M 0|0 1|0   157b   66.0k    1 Feb 16 11:14:21.082
        
#### 参数解释 ####

- insert/query/update/delete  
  每秒执行增删改查的次数

- getmore  
  mongodb在进行查询时，每次并不是返回所有的数据，比如要一次查询一百万条，
  每次只会返回一定量的数据，当每次find的时候，getmore用来获取以后的数据
  
- command  
  每秒执行的命令数
  
- dirty/used  
  dirty指脏数据字节的缓存百分比，used指正在使用中的缓存百分比

- flushes  
  WiredTiger引擎时，指检查点(checkpoint)的触发次数在一个轮询间隔期间；
  MMAPv1 引擎时，指每秒执行fsync将数据写入硬盘的次数

- vsize/res  
  虚拟/物理内存使用量

- qrw  
  qr，qw分别指客户端等待从mongodb实例读，写数据的队列长度。
  在写入或读取数据时，并不是来个请求就处理，而是放到队列中，qr和qw越高则mongodb当前性能越差

- arw  
  ar指执行读操作的活跃客户端数量；aw指执行写操作的活跃客户端数量

- net_in/net_out
  mongodb实例的网络进/出流量，单位是byte

- conn
  当前连接数
  
- time
  当前时间
  
  
### mongotop命令 ###

mongotop也是mongodb下的一个内置工具，mongotop提供了一个方法，用来跟踪一个MongoDB的实例，查看哪些大量的时间花费在读取和写入数据。
默认情况下，该命令每秒返回一次，可通过增加参数来修改返回数据的时间间隔，如mongotop 10 表示每10秒返回一组数据


    [root@localhost ~]# /data/mongodb-4.0.14/bin/mongotop 10
    2020-02-16T11:29:08.504+0800    connected to: 127.0.0.1
    
                        ns    total    read    write    2020-02-16T11:29:18+08:00
        admin.system.roles      0ms     0ms      0ms                             
        admin.system.users      0ms     0ms      0ms                             
      admin.system.version      0ms     0ms      0ms                             
    config.system.sessions      0ms     0ms      0ms                             
       config.transactions      0ms     0ms      0ms                             
            local.oplog.rs      0ms     0ms      0ms                             
         local.startup_log      0ms     0ms      0ms                             
      local.system.replset      0ms     0ms      0ms                             
       shijiange.shijiange      0ms     0ms      0ms                             
    
                        ns    total    read    write    2020-02-16T11:29:28+08:00
        admin.system.roles      0ms     0ms      0ms                             
        admin.system.users      0ms     0ms      0ms                             
      admin.system.version      0ms     0ms      0ms                             
    config.system.sessions      0ms     0ms      0ms                             
       config.transactions      0ms     0ms      0ms                             
            local.oplog.rs      0ms     0ms      0ms                             
         local.startup_log      0ms     0ms      0ms                             
      local.system.replset      0ms     0ms      0ms                             
       shijiange.shijiange      0ms     0ms      0ms                             


#### 参数解释 ####

- ns  
  表示数据库命名空间，并结合了数据库名称和集合
  
- total  
  表示在当前数据库命名空间执行读和写操作所花费的时间总和。
  
- read  
  表示在当前数据库命名空间执行读操作所花费的时间


- write  
  表示在当前数据库命名空间执行写操作所花费的时间
  
  
mongotop 可以在数据查询数据卡、慢、查询不出来的情况下使用。total 保持为0最好，
有时冒出个100ms-200ms问题不大，一般大于500ms时可以考虑给当前表做索引优化，结合慢查询日志找出mongod执行慢的原因。


### db.serverStatus() ###
mongodb中db.serverStatus()可用于查看所有的监控信息
