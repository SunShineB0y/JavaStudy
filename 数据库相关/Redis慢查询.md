## Redis慢查询 ##

慢查询是一个先进先出的队列，且长度固定，保存在内存中（redis重启该队列就会消失），
  当一个命令被执行且超过慢查询的阈值时，这条命令就会被加入到慢查询队列中。

#### Redis客户端请求的生命周期 ####

1.客户端发送命令到Redis

2.命令在Redis排队

3.Redis执行命令

4.Redis返回执行结果到客户端

*说明：*

*1.慢查询发生在第三阶段（也就是说执行的时候慢才是慢，并不包括命令传输、排队这些情况，如 keys * 命令）；*

*2.Redis客户端超时不一定是慢查询，慢查询可能是引起客户端超时的一个因素*


#### 慢查询参数 ####
Redis慢查询有两个参数，分别代表慢查询队列的长度和慢查询的阈值，且这两个参数支持动态配置。

- slowlog-max-len 慢查询队列的长度，默认值128

- slowlog-log-slower-than   慢查询的阈值（单位：微秒）  默认值10000

      slowlog-log-slower-than=0	   # 表示记录所有命令

      slowlog-log-slower-than<0	   # 表示不记录任何命令

#### 配置方法 ####
1. 修改配置文件并重启

2. 动态配置

       config set slowlog-max-len 1000
       config set slowlog-log-slower-than 1000
       config rewrite   # 使配置永久生效
#### 慢查询命令 ####
    slowlog  get [n]    # 获取慢查询队列，用n可获取指定条数
    slowlog  len        # 获取慢查询队列的长度
    slowlog  reset      # 清空慢查询队列
#### 运维经验  ####
1. slowlog-max-len   不要设置过小，通常1000左右

2. slowlog-log-slower-than  不要设置过大（单位：微秒），默认10ms，通常设置1ms，因为当redis命令超过1ms时可能就会对性能有一定影响了

3. 定期持久化慢查询
