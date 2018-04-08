# MongoDB权威指南
根据权威指南这本书记录的一些相关知识

### 第1、2章 MongoDB介绍和基础知识

启动命令：
```
mongod
```

链接命令：
```
mongo
```
  或者 mongo 主机：端口号/数据库 
```
mongo -nodb
```
 或者
```
mongo XX.js
```
  或者
```
mongo --quiet
```

查看数据库：
```
db
```

选择数据库：
```
use foo
```  
类似于db.getSister("foo")
显示所有数据库：
```
show dbs
```
类似于 db.getMongo().getDBs()
显示所有链接： 
```
show collections
```
类似于db.getCollectionNames()


创建:
```
db.XX.insert(XX);
```
读取：
```
db.XX.find()
```
 读取20个或者
```
db.XX.findOne();
```
更新：
```
db.XX.update({},XX);
```
删除：
```
db.XX.remove();
```

ObjectId：4位时间戳3位机器2位PID（进程标识符）3位计数器

Js文件加载：load(XX.js)，js放在bin文件夹下
js脚本不能出现shell辅助函数，如：use foo；show dbs； show collections

conn = new Mongo(主机：端口号);db = conn.getDB("数据库名");
或db = connect(主机:端口号/数据库名);

创建 .mongorc.js文件在当前用户名目录下C:\Users\yangpengyan

prompt可以改变shell提示，如prompt = function(){ return (new Date())+"> ";}显示当前数据库

EDITOR="路径"
 
db.collectionName可以获取集合,如果collectioName包含保留字或者无效名称
可以db.getCollection("名称");或者x[y]形式，如：
1、变量的使用 for(var I in collections){db.blog[collection[i]]},其中var collection = ["a","b","c"]
2、集合怪异的可以用 vae name = "%&^" db[name].find();



