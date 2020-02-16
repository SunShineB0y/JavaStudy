## MongoDB索引 ##
如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。
这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。


MongoDB中也有慢查询的概念，默认是执行超过100ms时会记录慢日志mongodb.log。

    db.getProfilingStatus() # 查看慢查询配置

### 索引 ###
创建数据库并插入数据，mongodb支持用for循环的方式插入数据

    user shijiange;
    # 插入500000数据
    for(i=1; i<=500000;i++){
      db.myuser.insert( {name:'mytest'+i, age:i} )
    }
    
- 查看当前索引
    
      db.myuser.getIndexes()  
      > db.myuser.getIndexes()
      [
              {
                      "v" : 2,             # 表示索引引擎的版本是2
                      "key" : {
                              "_id" : 1    # 默认只有_id的索引，1代表升序
                      },
                      "name" : "_id_",
                      "ns" : "shijiange.myuser"   # 表示索引所在的集合
              }
      ]
      db.myuser.totalIndexSize() # 查看索引大小
    
- 创建索引

      db.myuser.createIndex( {age:1} ) 	# 增加age的升序索引，也可用ensureIndex，这是3.0以前版本的用法
      
      db.myuser.createIndex( {age:-1} ) 	# 增加age的降序索引
      
      db.myuser.createIndex({"class":1,"name":1,"age":-1})  # 创建复合索引
      
      # 通过在创建索引时加 background:true 的选项，让创建工作在后台执行
      db.myuser.createIndex({"class":1,"name":1,"age":-1},{background:true}) 
      
      > db.myuser.find({age:99999}).explain(true)
      {
              "queryPlanner" : {
                      "plannerVersion" : 1,
                      "namespace" : "shjiange.myuser",
                      "indexFilterSet" : false,
                      "parsedQuery" : {
                              "age" : {
                                      "$eq" : 99999
                              }
                      },
                      "winningPlan" : {
                              "stage" : "FETCH",
                              "inputStage" : {
                                      "stage" : "IXSCAN",
                                      "keyPattern" : {
                                              "age" : 1
                                      },
                                      "indexName" : "age_1",
                                      "isMultiKey" : false,
                                      "multiKeyPaths" : {
                                              "age" : [ ]
                                      },
                                      "isUnique" : false,
                                      "isSparse" : false,
                                      "isPartial" : false,
                                      "indexVersion" : 2,
                                      "direction" : "forward",
                                      "indexBounds" : {
                                              "age" : [
                                                      "[99999.0, 99999.0]"
                                              ]
                                      }
                              }
                      },
                      "rejectedPlans" : [ ]
              },
              "executionStats" : {
                      "executionSuccess" : true,
                      "nReturned" : 1,
                      "executionTimeMillis" : 1,             # 查询时间1ms
                      "totalKeysExamined" : 1,
                      "totalDocsExamined" : 1,               # 表示只扫描了一行数据
                      "executionStages" : {
                              "stage" : "FETCH",
                              "nReturned" : 1,
                              "executionTimeMillisEstimate" : 0,
                              "works" : 2,
                              "advanced" : 1,
                              "needTime" : 0,
                              "needYield" : 0,
                              "saveState" : 0,
                              "restoreState" : 0,
                              "isEOF" : 1,
                              "invalidates" : 0,
                              "docsExamined" : 1,
                              "alreadyHasObj" : 0,
                              "inputStage" : {
                                      "stage" : "IXSCAN",     # 表示通过索引查询
                                      "nReturned" : 1,
                                      "executionTimeMillisEstimate" : 0,
                                      "works" : 2,
                                      "advanced" : 1,
                                      "needTime" : 0,
                                      "needYield" : 0,
                                      "saveState" : 0,
                                      "restoreState" : 0,
                                      "isEOF" : 1,
                                      "invalidates" : 0,
                                      "keyPattern" : {
                                              "age" : 1
                                      },
                                      "indexName" : "age_1",
                                      "isMultiKey" : false,
                                      "multiKeyPaths" : {
                                              "age" : [ ]
                                      },
                                      "isUnique" : false,
                                      "isSparse" : false,
                                      "isPartial" : false,
                                      "indexVersion" : 2,
                                      "direction" : "forward",
                                      "indexBounds" : {
                                              "age" : [
                                                      "[99999.0, 99999.0]"
                                              ]
                                      },
                                      "keysExamined" : 1,
                                      "seeks" : 1,
                                      "dupsTested" : 0,
                                      "dupsDropped" : 0,
                                      "seenInvalidated" : 0
                              }
                      },
                      "allPlansExecution" : [ ]
              },
              "serverInfo" : {
                      "host" : "localhost",
                      "port" : 27017,
                      "version" : "4.0.14",
                      "gitVersion" : "1622021384533dade8b3c89ed3ecd80e1142c132"
              },
              "ok" : 1
      }
      
      
    
- 删除索引

      db.myuser.dropIndexes()         # 删除myuser集合的所有索引
      db.myuser.dropIndex( {age:1} ) 	# 删除age的升序索引
      
      # 通过索引名称的方式删除索引
      > db.myuser.dropIndex("name_1_age_-1")
      { "nIndexesWas" : 2, "ok" : 1 }
      
      # 索引删除后的查询计划
      > db.myuser.find({age:99999}).explain(true)
      {
              "queryPlanner" : {
                      "plannerVersion" : 1,
                      "namespace" : "shjiange.myuser",
                      "indexFilterSet" : false,
                      "parsedQuery" : {
                              "age" : {
                                      "$eq" : 99999
                              }
                      },
                      "winningPlan" : {
                              "stage" : "COLLSCAN",
                              "filter" : {
                                      "age" : {
                                              "$eq" : 99999
                                      }
                              },
                              "direction" : "forward"
                      },
                      "rejectedPlans" : [ ]
              },
              "executionStats" : {
                      "executionSuccess" : true,
                      "nReturned" : 1,
                      "executionTimeMillis" : 227,      # 执行时间227ms，默认情况下这条查询命令会记录到慢日志
                      "totalKeysExamined" : 0,
                      "totalDocsExamined" : 500000,     # 总共扫描的行数
                      "executionStages" : {
                              "stage" : "COLLSCAN",     # 表示全表扫描
                              "filter" : {
                                      "age" : {
                                              "$eq" : 99999
                                      }
                              },
                              "nReturned" : 1,
                              "executionTimeMillisEstimate" : 5,
                              "works" : 500002,
                              "advanced" : 1,
                              "needTime" : 500000,
                              "needYield" : 0,
                              "saveState" : 3906,
                              "restoreState" : 3906,
                              "isEOF" : 1,
                              "invalidates" : 0,
                              "direction" : "forward",
                              "docsExamined" : 500000
                      },
                      "allPlansExecution" : [ ]
              },
              "serverInfo" : {
                      "host" : "localhost",
                      "port" : 27017,
                      "version" : "4.0.14",
                      "gitVersion" : "1622021384533dade8b3c89ed3ecd80e1142c132"
              },
              "ok" : 1
      }


使用正则的话，索引无效果

    db.myuser.find( {"name":"mytest1"} )
    db.myuser.ensureIndex( {name:1} )			#添加索引
    db.myuser.find( {"name":"mytest6"} )
    db.myuser.find( {"name":/99999/} )
    db.myuser.find( {"name":/99999/} ).explain(true)        #使用正则，全表扫描，也是慢
    
mongodb建立唯一索引，唯一索引对应的值不能重复

    use shijiange
    db.myuser.insert( {userid:1} )
    db.myuser.insert( {userid:1} )                    # 没有创建唯一索引时可以插入相同的数据，只是_id不同
    db.myuser.remove({}) 		                  # 清空数据
    db.myuser.ensureIndex( {userid:1},{unique:true} ) 	# 创建唯一索引
    db.myuser.insert( {userid:1} )
    db.myuser.insert( {userid:2} )
    db.myuser.insert( {userid:1} )                    # 因为是唯一索引，所以会报错

