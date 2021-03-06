## MongoDB副本集 ##
MongoDB副本集是一组维护相同数据集的mongod实例，通过在不同数据库服务器上提供多个数据副本，
复制可提供一定级别的容错功能。副本集类似有自动故障恢复功能的主从集群。

通俗地讲，就是用多台机器进行同一数据的异步同步，从而使多台机器拥有同一数据的多个副本，并且
当主库宕掉时在不需要用户干预的情况下自动切换其他备份服务器做主库，而且还可以利用副本服务器
做只读服务器，实现读写分离、提高负载。副本集提供冗余和高可用性，是所有生产部署的基础。

### 副本集与主从复制的区别 ###
副本集与主从集群最大的区别就是副本集没有固定的“主节点”；整个集群会选举出一个“主节点”，当主
节点挂掉后，又在剩下的节点中重新选取一个“主节点”，副本集总有一个活跃节点（主、primary）和
多个备份节点（从、secondary）。

### MongoDB单实例模式所具有的缺陷 ###
1. 数据会有丢失的风险
2. 无法做高可用性
3. mongodb副本集能够预防数据丢失，多台mongodb数据一致
4. mongodb副本集能够在有问题的时候自动切换

### 副本集的三个角色 ###
- 主要成员（Primary）：主要接收所有写操作。就是主节点。
- 副本成员（Peplicate）：从主节点通过复制操作以维护相同的数据集，即备份数据，不可写操作，但
可以读操作（但需要配置）。是默认的一种从节点类型。
- 仲裁者（Arbiter）：不保留任何数据的副本集，只具有投票选举作用。当然也可以将仲裁服务器维护
为副本集的一部分，即副本集成员同时也可以是仲裁者。也是一种从节点类型。


### 副本集的创建 ###
同一个副本集的所有角色要在配置文件中添加相同的配置，即指定相同的副本集名称

    replication:
      # 副本集名称 fubenji
      replSetName: fubenji

### 初始化副本集 ###
使用默认的配置来初始化副本集

    # 初始化副本集
    rs.initiate()
    
    # 提示
    1. "ok"的值为1，说明创建成功
    2.命令行提示符发生变化，变成了一个从节点角色，此时默认不能读写。稍等片刻，回车，变成主节点
    
    # 查看副本集的配置内容
    rs.conf(configuration)
    
    提示： rs.config()是该方法的别名，configuration可选，如果没有配置，则默认主节点配置
    
    # 添加副本从节点
    rs.add("192.168.1.82:27018")
    
    # 添加仲裁节点
    rs.add("192.168.1.82:27019",true) 或者 rs.addArb("192.168.1.82:27019")
    
    # 查看副本集状态
    rs.status()
    
设置读操作权限

    # 登录副本从节点
    rs.slaveOk() 或 rs.slaveOk(true)
    
    提示：该命令是db.getMongo().setSlaveOk()的简化命令
    
    # 收回副本节点的读权限
    rs.slaveOk(false)
    
### 主节点的选举原则 ###
MongoDB在副本集中，会自动进行主节点的选举，主节点选举的触发条件

1. 主节点故障

2. 主节点网络不可达（默认心跳间隔为10s）

3. 人工干预（re.stepDown(600)）

选举规则是根据票数来决定谁获胜

1. 票数最高，且获得了“大多数”成员的投票支持的节点获胜。
   
   大多数的定义为：若投票成员数量为N，则大多数为N/2 + 1。当复制集内存活成员数量不足大多数时，
   整个复制集将无法选举出主节点，复制集将无法提供写服务，处于只读状态
 
2. 若票数相同，且都获得了大多数成员的投票支持，数据新的节点获胜。数据的新旧是通过日志oplog来
对比的。


在获得票数的时候，优先级（priority）参数影响重大，可以通过设置该参数来设置额外票数。优先级即
权重，取值0-1000，相当于可额外增加0-1000的票数。该参数指定较高的值可使成员更有资格成为主要节
点，默认情况下，优先级的值是1。
