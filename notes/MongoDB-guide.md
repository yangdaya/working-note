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
  或者  
```  
mongo 主机：端口号/数据库
```  
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
类似于
``` 
db.getSister("foo")
``` 
显示所有数据库：
```
show dbs
```
类似于
``` 
db.getMongo().getDBs()
``` 
显示所有链接： 
```
show collections
```
类似于
``` 
db.getCollectionNames()
``` 


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

Js文件加载：
``` 
load(XX.js)
``` 
js放在bin文件夹下
js脚本不能出现shell辅助函数，如：use foo；show dbs； show collections
```
conn = new Mongo(主机：端口号);
db = conn.getDB("数据库名");
```
或
``` 
db = connect(主机:端口号/数据库名);
``` 

创建 .mongorc.js文件在当前用户名目录下C:\Users\yangpengyan

prompt可以改变shell提示，如
```
prompt = function(){ return (new Date())+"> ";}
```
显示当前数据库

EDITOR="路径"
 
db.collectionName可以获取集合,如果collectioName包含保留字或者无效名称
可以db.getCollection("名称");或者x[y]形式，如：
1、变量的使用
```
for(var I in collections){db.blog[collection[i]]}
```
其中var collection = ["a","b","c"]

2、集合怪异的可以用 
```
vae name = "%&^"
db[name].find();
```
### 第三章 创建、更新和删除文档

插入并保存文档
```
db.foo.insert({文档});
```
插入insert最大的文档为16mb，如果查询一个稳定大小，可以用Object.bsonsize(文档名),单位为字节
批量增加，mongoimport命令行工具导入原始数据

查询文档
```
db.foo.find()
```
 或者
```
db.foo.find({}) 
```
或者
```
db.foo.findOne()
```
或者 
```
db.foo.findOne({})
```

删除文档
```
db.foo.remove({条件});
```
可加入文档作为条件，如{"username":"aa"}
drop,不能指定条件,删除整个集合

更新文档
```
db.foo.update({条件},{文档});
```

使用修改器
$inc可以增加指定的值，用法：
```
{"$inc":{"键":增加的个数}}
```
如果字段不存在则创建。只能用于整型、长整型或双精度浮点型
```
db.foo.update({"username":"aa3"},{"$inc":{"bb":1}})
```
$set指定一个字段的值，如果字段不存在则创建，对于内嵌文档在使用$set更新时，使用"."连接的方式。
$unset删除键
```
{"$unset":{"要删除的字段":1}})
```
不论对目标键使用1、0、-1或者具体的字符串等都是可以删除该目标键

数组修改器
$push向已有的数组末尾加入一个元素（字段或者集合），要是没有就创建一个新的数组，不过滤重复的数据，要求键值类型必须是数组db.foo.update({"username":"aa4"},{"$push":{"bb":1}})
$each 可以通过push增加多个值
$slice 可以与push和each组合使用，保证数组不会超出设定好的最大长度，必须是负整数，保留最后的几个数据，可以$sort对数组中的所有对象进行排序，-1倒序,1正序
$lt,$lte,$gt,$gte,$ne分别表示<,<=,>,>=,!=
$addToSet可以避免插入重复，可与$each组合起来，添加多个不同的值
$pop从数组的头或者尾删除数组中的元素。1或者0从数组的尾部删除，-1从数组的头部删除
$pull从数组中删除满足条件的元素
数组的定位修改器，$.数字或者直接用$,如果为多个文档满足条件，则只更新第一个文档


修改器速度db.coll.stats()查看填充因子（addingFactor）
	1、最初文档直接没有多余的空间
	2、如果一个文档因为体积变大而不得不进行移动，它原先占用的空间就闲置了，而且填充因子会增加
	3、之后插入的新文档都会拥有填充因子制定大小的增长空间，如果在之后的插入中不再发生文档移动，填充因子会逐渐变小
空白空间太多，系统会给提示，需要进行压缩
如果在进行插入和删除时会进行大量的移动或者经常打乱数据，可以用userPowerOf2Sizes选项提高磁盘复用率，但是不适合经常插入或者原地更新的集合上使用。因为分配的空间为2的幂
	db.runCommand({"collMod":collectionName,"usePowerOf2Sizes":true})


upsert，update的第三个参数为true表示这个是upsert
$setOnInsert文档插入时设置字段的值
$save在文档不存在的时候插入，存在的时候更新，只有一个参数文档
批量更新多个文档，update的第四个参数为true表示更新所有匹配的文档
getLastError表示最后一次操作的相关信息.db.runCommand({getLastError:1})返回的n表示多少个文档更新了，
	"updatedExisting" : true,说明对已有的文档进行更新
	
返回被更新的文档，findAndModify

写入安全机制


