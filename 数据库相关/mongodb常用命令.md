## Mongodb ##

### 连接 ###

### 命令 ###
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

- 创建数据库 runoob

		use runoob;

- 查看当前db

		db;

- 查看所有数据库

		show dbs;
		admin   0.000GB
		config  0.000GB
		local   0.000GB

在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。

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

