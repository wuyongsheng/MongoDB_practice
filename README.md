>  最近在工作中越来越多地使用到MongoDB数据库，接触MongoDB的时间也不算长，感觉MongoDB作为非关系型数据库，在语法上与关系型数据库中的 Mysql 有些类似。这几天看了下菜鸟教程上 MongoDB的章节，觉得写得很好很全面。结合菜鸟教程以及自己对MongoDB的使用，把 MongoDB 的语法以及使用 Python 操作 MongoDB 做一个总结。

### MongoDB 介绍

引用百度百科对 MongoDB 的介绍：

MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。

MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。它支持的数据结构非常松散，是类似json的bson格式，因此可以存储比较复杂的数据类型。

Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。

### MongoDB 的安装

可以直接在 MongoDB 的官网上下载 MongoDB 的安装包进行安装，地址：www.mongodb.com/download-center#community 
安装的过程很简单，参照网上的方法进行安装就可以了，我安装的是最新的 v4.0.5

-  连接MongoDB

我们可以在命令窗口中运行 mongo.exe 命令即可连接上 MongoDB，执行如下命令：
```
C:\MongoDB\bin>mongo.exe
```
执行以上命令后，会自动进入到 MongoDB Shell（它是 MongoDB 自带的交互式Javascript shell,用来对 MongoDB 进行操作和管理的交互式环境）。

同时，它默认会链接到 test 文档（数据库），使用 db 命令即可查看当前所处的数据库，如下图所示：

![mongodb连接.jpg](https://upload-images.jianshu.io/upload_images/12273007-acf9fd4c36ec418b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### MongoDB 概念解析

下表将帮助您更容易理解Mongo中的一些概念：

SQL术语/概念	 | MongoDB术语/概念	 | 解释/说明
---|---|---
database | database | 数据库
table | collection | 数据库表/集合
row | document | 数据记录行/文档
column | field | 数据字段/域
index | index | 索引
table joins	 |  | 表连接,MongoDB不支持
primary key | primary key | 主键,MongoDB自动将_id字段设置为主键

###  MongoDB的可视化工具

上文的截图中是通过在 Windows 的 DOS 窗口执行安装目录下的 mongo.exe 命令连接到 MongoDB 的，也可以通过一些可视化工具连接到 MongoDB ，我是用过三款 MongoDB 的可视化工具：robo 3t、nosqlbooster 、adminmongo，其中 adminmongo 是一款 web 工具，使用的时候要启动 web 服务，不太推荐。下面介绍robo 3t和nosqlbooster：

-  nosqlbooster

个人感觉 nosqlbooster 是这三款可视化工具中功能最强大的，启动nosqlbooster后，在Basic选项卡中填入下图所示参数（name可以自定义），其他的默认，点击左下方的 Test Connection 按钮测试连接是否通过，通过之后，点击 Save & Connect 即可连接成功
![nosqlbooter.jpg](https://upload-images.jianshu.io/upload_images/12273007-d4ff8b27e0d9f720.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

连接成功后，按照如下图所示即可对指定数据库进行查询（类似于 mysql的 Navicat工具）：

![nosqlbooter查询.jpg](https://upload-images.jianshu.io/upload_images/12273007-5bcd34fb9d4ddc47.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  robo 3t

robo 3t 连接MongoDB的方式类似于 nosqlbooster，连接成功之后，按下图的方式即可对MongoDB数据库进行查询操作：

![3t查询.jpg](https://upload-images.jianshu.io/upload_images/12273007-84c1ea28f872b653.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-  这两款工具的比较

个人感觉这两款工具各有优劣：nosqlbooter 功能强大，但如果电脑的配置不好，使用起来可能会比较卡顿（我在我的腾讯云主机上使用时会比较卡），robo 3t提供的功能不多，但在配置比较差的机器上也不会卡顿。

###  MongoDB 创建数据库

-  语法

MongoDB 创建数据库的语法格式如下：
```
use DATABASE_NAME
```
如果数据库不存在，则创建数据库，否则切换到指定数据库。

-  实例

以下实例我们创建了数据库 wys:

```
> use wys
switched to db wys
> db
wys
>
```

如果你想查看所有数据库，可以使用 show dbs 命令：
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
>
```
可以看到，我们刚创建的数据库 wys 并不在数据库的列表中， 要显示它，我们需要向 wys 数据库插入一些数据。
```
> db.wys.insert({"name":"wysh"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
wys     0.000GB
>
```
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

注意: 在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建

###  MongoDB 删除数据库
- 语法

MongoDB 删除数据库的语法格式如下：
```
db.dropDatabase()
```
- 实例

以下实例我们删除了数据库 wys。

首先，查看所有数据库：
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
wys     0.000GB
>
```
接下来我们切换到数据库 wys：
```
> use wys
switched to db wys
>
```
执行删除命令：

```
> db.dropDatabase()
{ "dropped" : "wys", "ok" : 1 }
>
```
最后，我们再通过 show dbs 命令数据库是否删除成功：

```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
>
```
###  MongoDB 创建集合
MongoDB 中使用 createCollection() 方法来创建集合。

- 语法格式：

```
db.createCollection(name, options)
```
- 参数说明：

1. name: 要创建的集合名称
2. options: 可选参数, 指定有关内存大小及索引的选项

-  实例
在 test 数据库中创建 wys 集合：
```
> use test
switched to db test
> db.createCollection("wysh")
{ "ok" : 1 }
>
```
如果要查看已有集合，可以使用 show collections 命令：

```
> show collections
col
nihao
wysh
>
```
在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。

```
> db.wysh1.insert({"name":"zhangsan"})
WriteResult({ "nInserted" : 1 })
> show collections
col
nihao
wysh
wysh1
>
```
###  MongoDB 删除集合
MongoDB 中使用 drop() 方法来删除集合。

-  语法格式：

```
db.collection.drop()
```
-  返回值

如果成功删除选定集合，则 drop() 方法返回 true，否则返回 false。

-  实例
在数据库 test 中，我们可以先通过 show collections 命令查看已存在的集合

```
> use test
switched to db test
> show collections
col
nihao
wysh
wysh1
>
```
接着删除集合 wysh1 :
```
> db.wysh1.drop()
true
> show collections
col
nihao
wysh
>
```
###  MongoDB 插入文档

文档的数据结构和JSON基本一样。

所有存储在集合中的数据都是BSON格式。

BSON是一种类json的一种二进制形式的存储格式,简称Binary JSON。

- 插入文档

MongoDB 使用 insert() 或 save() 方法向集合中插入文档，语法如下：
```
db.COLLECTION_NAME.insert(document)
```
- 实例

以下文档可以存储在 MongoDB 的 wysh 数据库 的 wy 集合中：
```
> use wysh
switched to db wysh
> db
wysh
> db.wy.insert({"name":"wysh","age":55,"sex":"male","country":"China"})
WriteResult({ "nInserted" : 1 })
```
以上实例中 wy 是我们的集合名，如果该集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。

-  查看已插入文档：
```
> db.wy.find()
{ "_id" : ObjectId("5c3f485b3f1190b4d7b9906c"), "name" : "wysh", "age" : 55, "se
x" : "male", "country" : "China" }
>
```
-  3.2 版本后还有以下几种语法可用于插入文档:

```
> db.wy.insertOne({"aaa":"333"})
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5c3f4aae3f1190b4d7b9906d")
}
>
> db.wy.insertMany([{"bbb":"444"},{"ccc":"444"}])
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5c3f4b293f1190b4d7b9906e"),
                ObjectId("5c3f4b293f1190b4d7b9906f")
        ]
}
> db.wy.find()
{ "_id" : ObjectId("5c3f485b3f1190b4d7b9906c"), "name" : "wysh", "age" : 55, "se
x" : "male", "country" : "China" }
{ "_id" : ObjectId("5c3f4aae3f1190b4d7b9906d"), "aaa" : "333" }
{ "_id" : ObjectId("5c3f4b293f1190b4d7b9906e"), "bbb" : "444" }
{ "_id" : ObjectId("5c3f4b293f1190b4d7b9906f"), "ccc" : "444" }
>
```

###  MongoDB 更新文档
MongoDB 使用 update() 和 save() 方法来更新集合中的文档。接下来让我们详细来看下两个函数的应用及其区别。

-  update() 方法

update() 方法用于更新已存在的文档。语法格式如下：
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
参数说明：

- query :

update的查询条件，类似sql
update查询内where后面的。

- update : 

update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的

- upsert : 

可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。

- multi : 

可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。

-  writeConcern :

可选，抛出异常的级别。

- 实例

我们在集合 wy 中插入如下数据：
```
db.wy.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: 'wysh',
    url: 'http://www.wysh.site',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
接着我们通过 update() 方法来更新标题(title):

```
db.wy.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})

Updated 1 existing record(s) in 106ms
```
```
> db.wy.find().pretty()
{
        "_id" : ObjectId("5c3f485b3f1190b4d7b9906c"),
        "name" : "wysh",
        "age" : 55,
        "sex" : "male",
        "country" : "China"
}
{ "_id" : ObjectId("5c3f4aae3f1190b4d7b9906d"), "aaa" : "333" }
{ "_id" : ObjectId("5c3f4b293f1190b4d7b9906e"), "bbb" : "444" }
{ "_id" : ObjectId("5c3f4b293f1190b4d7b9906f"), "ccc" : "444" }
{
        "_id" : ObjectId("5c3f51dec305b104a14e4618"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```
可以看到标题(title)由原来的 "MongoDB 教程" 更新为了 "MongoDB"。
以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
```
-  save() 方法

save() 方法通过传入的文档来替换已有文档。语法格式如下：
```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```
参数说明：
- document : 文档数据。

- writeConcern :可选，抛出异常的级别。
- 实例

以下实例中我们替换了 _id 为 56064f89ade2f21f36b03136 的文档数据：
```
db.col.save({
    "_id" : ObjectId("5c3f4b293f1190b4d7b9906f"),
    "title" : "MongoDB",
    "description" : "MongoDB 是一个 Nosql 数据库",
    "by" : "wysh.site",
    "url" : "http://www.wysh.site.com",
    "tags" : [
            "mongodb",
            "NoSQL"
    ],
    "likes" : 110
})
```
替换成功后，我们可以通过 find() 命令来查看替换后的数据

```
> db.wy.find().pretty()
{
        "_id" : ObjectId("5c3f485b3f1190b4d7b9906c"),
        "name" : "wysh",
        "age" : 55,
        "sex" : "male",
        "country" : "China"
}
{ "_id" : ObjectId("5c3f4aae3f1190b4d7b9906d"), "aaa" : "333" }
{ "_id" : ObjectId("5c3f4b293f1190b4d7b9906e"), "bbb" : "444" }
{
        "_id" : ObjectId("5c3f4b293f1190b4d7b9906f"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh.site",
        "url" : "http://www.wysh.site.com",
        "tags" : [
                "mongodb",
                "NoSQL"
        ],
        "likes" : 110
}
{
        "_id" : ObjectId("5c3f51dec305b104a14e4618"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```
###  MongoDB 删除文档
MongoDB remove()函数是用来移除集合中的数据。

MongoDB数据更新可以使用update()函数。在执行remove()函数前先执行find()命令来判断执行的条件是否正确，这是一个比较好的习惯。

-  语法

remove() 方法的基本语法格式如下所示：
```
db.collection.remove(
   <query>,
   <justOne>
)
```
参数说明：

- query :

（可选）删除的文档的条件。
- justOne : 

（可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- writeConcern :

（可选）抛出异常的级别。
- 实例

以下文档我们执行两次插入操作：
```
db.wy.insert({
    title: 'MongoDB test', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: 'wysh',
    url: 'http://www.wysh.site.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 101010
})
```
使用 find() 函数查询数据：
```
> db.wy.find().pretty()
{
        "_id" : ObjectId("5c3f576bc305b104a14e4619"),
        "title" : "MongoDB test",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 101010
}
{
        "_id" : ObjectId("5c3f576cc305b104a14e461a"),
        "title" : "MongoDB test",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 101010
}
>
```
接下来我们移除 title 为 'MongoDB 教程' 的文档：
```
db.wy.remove({'title':'MongoDB test'})
Removed 2 record(s) in 2ms
```
如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：
```
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```
如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：
```
>db.wy.remove({})
>db.wy.find()
>
```
### MongoDB 查询文档
MongoDB 查询文档使用 find() 方法。

find() 方法以非结构化的方式来显示所有文档。

- 语法

MongoDB 查询数据的语法格式如下：
```
db.collection.find(query, projection)
```
- query ：

可选，使用查询操作符指定查询条件
- projection ：

可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：
```
>db.wy.find().pretty()
```
pretty() 方法以格式化的方式来显示所有文档。

-  实例

以下实例我们查询了集合 wy 中的数据：
```
> db.wy.find().pretty()
{
        "_id" : ObjectId("5c3f576bc305b104a14e4619"),
        "title" : "MongoDB test",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 101010
}
{
        "_id" : ObjectId("5c3f576cc305b104a14e461a"),
        "title" : "MongoDB test",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "wysh",
        "url" : "http://www.wysh.site.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 101010
}
```
除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。














