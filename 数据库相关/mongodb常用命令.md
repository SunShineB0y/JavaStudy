## Mongodb ##

### 启动 ###
MongoDB提供了可执行程序mongod用于启动MongoDB服务器，可以通过此命令指定配置文件的方式启动MongoDB

    # /data/mongodb-4.0.14是安装路径，/data/mongodb/27017/conf是配置文件路径
    /data/mongodb-4.0.14/bin/mongod -f /data/mongodb/27017/conf/mongodb.conf

### 连接 ###
MongoDB提供一个mongo客户端，类似于mysql提供的客户端命令

    /data/mongodb-4.0.14/bin/mongo -uroot -p123456  --port 27017 admin

### 命令 ###
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

- 创建数据库 runoob

		use runoob;

- 查看当前db

		db;

- 关闭mongodb服务器
        
        # mongo客户端提供了一个正确关闭mongodb服务器的方法
        use admin
        db.shutdownServer()

- 查看所有数据库

		show dbs;
        # 或者 show databases;
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