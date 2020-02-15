## MongoDB介绍 ##
MongoDB是个非关系型数据库，但操作跟关系型数据最类似。它是面向文档存储的非关系型数据库，
数据以json的格式进行存储。MongoDB可用来永久存储，也可用来缓存数据，MongoDB提供了副本集和分片集群的功能。

需要注意的是：MongoDB的版本偶数版本为稳定版，奇数版本为开发版。

### 启动 ###
MongoDB提供了可执行程序mongod用于启动MongoDB服务器，可以通过此命令指定配置文件的方式启动MongoDB

    # /data/mongodb-4.0.14是安装路径，/data/mongodb/27019/conf是配置文件路径
    /data/mongodb-4.0.14/bin/mongod -f /data/mongodb/27019/conf/mongodb.conf

### 连接 ###
MongoDB提供一个mongo客户端，类似于mysql提供的客户端命令

    /data/mongodb-4.0.14/bin/mongo -uroot -p123456  --port 27019 admin

### 命令 ###
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

- 创建数据库 runoob

		use runoob; # 使用use即可，无需显示创建

- 查看当前db

		db;

- 关闭mongodb服务器
        
        # mongo客户端提供了一个正确关闭mongodb服务器的方法
        use admin
        db.shutdownServer()

- 查看所有数据库

      show dbs;
      show databases;
      # admin/config/local 是mongodb自带的三个库
      admin   0.000GB
	    config  0.000GB
      local   0.000GB

在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。

- 删除数据库

        # 删除数据库mydatabase
        use mydatabase
        db.dropDatabase()

- 插入数据

		db.runoob.insert({"name":"菜鸟教程"})
		db.runoob.insert({"age":"18"})
		db.runoob.insert({"age":"18"},{"name":"菜鸟教程"})

此时向 runoob 数据库插入了一些数据,再查看所有db

	show dbs;
	admin   0.000GB
	config  0.000GB
	local   0.000GB
	runoob  0.000GB

如上，可以看到我们的runoob。
		
- 查询数据

      db.runoob.find() # 查询所有数据
      db.runoob.find().pretty() # 用易读的方式展示数据
      db.runoob.find().limit(2) # 限制查询数量
      db.runoob.find().skip(2) # 跳过前两条数据
      db.runoob.find({"age":"18"}) # 带条件的查询

    - 可以用skip和limit组合实现分页查询
    
          db.runoob.find().skip(0).limit(2)
          db.runoob.find().skip(2).limit(2)
          db.runoob.find().skip(4).limit(2)
          
    - 使用sort进行排序
    
          db.runoob.find().sort({"age":1}) # 按age升序
          db.runoob.find().sort({"age":-1}) # 按age降序
          
    - 数字比较查询
    
          db.runoob.find({"age":{$lt:30}}) # 查询age小于30的数据
          $gt	#大于
          $lt	#小于
          $gte	#大于或等于
          $lte	#小于或等于
    
    - 多种查询条件组合
    
          db.runoob.find({"name":"张三"})
          db.runoob.find({"name":"李四"})
          db.runoob.find({$or:[{"name":"张三"},{"name":"李四"}]}) # 或者
          db.runoob.find({$and:[{"name":"张三"},{"name":"李四"}]}) # 并且
          db.runoob.find({$and:[{"name":"张三"},{"age":"20"}]}) 
          
    - mongodb正则查询，支持普通正则和扩展正则
    
          db.runoob.find({"name":{$regex:"zhangsan[1-9]"}}) # 普通正则过滤
          db.runoob.find({"name":{$regex:"(zhangsan)"}}) # 支持分组正则
		
- 查看集合

      show collections; # 或者 show tables 
      
- 删除数据

      use mydatabase
      db.runoob.remove("age":"18") # 删除数据使用remove
      db.runoob.remove({}) #删除集合的所有数据，需要{}
      db.runoob.drop() # 删除集合
      
- 更新数据

      use mydatabase
      db.runoob.update({"age":"18"},{$set:{"age":"20"}})

