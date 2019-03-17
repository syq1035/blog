---
title: MongoDB
date: 2018-11-29 20:10:10
tags: MongoDB
categories: MongoDB
---

## 运行 MongoDB 服务器
运行 MongoDB 服务器，你必须从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件。
```
E:\MongoDB\bin\mongod --dbpath d:\data\db
```
## 连接MongoDB
```
E:\MongoDB\bin\mongo.exe
```
或者直接打开E:\MongoDB\bin下的mongo.exe文件
<!-- more -->
## 查看所有数据库
```
show dbs
```
## 创建数据库
```
>use DATABASE_NAME            //eg:  use runoob
```
如果数据库不存在，则创建数据库，否则切换到指定数据库
## 删除数据库
```
>db.dropDatabase()             //删除当前use数据库
```
## 查看已有集合
```
show collections
```
## 创建集合
```
>db.createCollection(name, options)
```
* name: 要创建的集合名称
* options: 可选参数, 指定有关内存大小及索引的选项

在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。
```
> db.mycol2.insert({"name" : "菜鸟教程"})
```
## 删除集合
```
>db.collection.drop()
```
## 插入文档
```
db.COLLECTION_NAME.insert(document)
```
在集合 col 中插入如下数据 : 
```
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
## 查看已插入文档
```
db.COLLECTION_NAME.find()
```
## 更新文档
```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
* **query **: update的查询条件，类似sql update查询内where后面的。
* **update **: update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
* **upsert **: 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
* **multi **: 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
* **writeConcern **:可选，抛出异常的级别。

更新标题(title):
```
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
```
以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
```
只更新第一条记录：
```
db.col.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );
```
全部更新：
```
db.col.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );
```
只添加第一条：
```
db.col.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );
```
全部添加加进去:
```
db.col.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );
```
全部更新：
```
db.col.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );
```
只更新第一条记录：
```
db.col.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );
```
## 删除文档
```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```
* **query **:（可选）删除的文档的条件。
* **justOne **: （可选）如果设为 true 或 1，则只删除一个文档。
* **writeConcern **:（可选）抛出异常的级别。
## 查询文档
```
db.collection.find(query, projection)
```
* **query** ：可选，使用查询操作符指定查询条件
* **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：
```
>db.col.find().pretty()
```
pretty() 方法以格式化的方式来显示所有文档。
| **操作** | **格式**                                   | **范例**                              | **RDBMS中的类似语句**   |
| :----- | :--------------------------------------- | :---------------------------------- | :---------------- |
| 等于     | {<key>:<value>}                          | db.col.find({"by":"菜鸟教程"}).pretty() | where by = '菜鸟教程' |
| 小于     | {<key>:{$lt:<value>}}   | db.col.find({"likes":{$lt:50}}).pretty() | where likes < 50                    |                   |
| 小于或等于  | {<key>:{$lte:<value>}}   | db.col.find({"likes":{$lte:50}}).pretty() | where likes <= 50                   |                   |
| 大于     | {<key>:{$gt:<value>}}   | db.col.find({"likes":{$gt:50}}).pretty() | where likes > 50                    |                   |
| 大于或等于  | {<key>:{$gte:<value>}}   | db.col.find({"likes":{$gte:50}}).pretty() | where likes >= 50                   |                   |
| 不等于    | {<key>:{$ne:<value>}}   | db.col.find({"likes":{$ne:50}}).pretty() | where likes != 50                   |                   |

### AND 条件
MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，即常规 SQL 的 AND 条件。
语法格式如下：
```
>db.col.find({key1:value1, key2:value2}).pretty()
```
### OR 条件
```
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```
### $type 操作符
基于BSON类型来检索集合中匹配的数据类型，并返回结果。
```
db.col.find({"title" : {$type : 2}})
或
db.col.find({"title" : {$type : 'string'}})
```
### Limit() 方法
如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。
```
>db.COLLECTION_NAME.find().limit(NUMBER)
```
### Skip() 方法
我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，
skip方法同样接受一个数字参数作为跳过的记录条数。
```
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```
## 排序sort() 方法
在 MongoDB 中使用 sort() 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而 -1 是用于降序排列。
```
>db.COLLECTION_NAME.find().sort({KEY:1})
```
eg.
```
sort({'createTime':-1})
```
## 关联
```
populate(path, [select], [model], [match], [options]）
```
path：指定要查询的表
select(可选)：指定要查询的字段，-表示不显示
model(可选)：类型：Model，可选，指定关联字段的 model，如果没有指定就会使用Schema的ref。
match(可选)：类型：Object，可选，指定附加的查询条件。
options(可选)：类型：Object，可选，指定附加的其他查询选项，如排序以及条数限制等等。

```
var mongoose = require('mongoose');
var Schema   = mongoose.Schema;

var UserSchema = new Schema({
    name  : { type: String, unique: true },
});
var CommentSchema = new Schema({
    commenter : { type: Schema.Types.ObjectId, ref: 'User' },
    content   : String
});
```

```
Comment.find({_id:Id})
    .populate({path:'User',select:'name -_id',options:{limit:5}})
    .exec(function(err,catetories){
        if (err) {
            console.log(err);
        }
 })
```

