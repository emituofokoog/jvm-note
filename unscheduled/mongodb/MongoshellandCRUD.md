# Mongo shell and CRUD
#### mongo shell

进入mongo shell
> mongo

显示当前使用对数据库
> db

列出所有可用的数据库
> show dbs
切换数据库
> use <database>

你可以切换到一个不存在的数据库。当你第一次向数据库存储数据，如通过创建一个集合，MongoDB将自动创建数据库。例如，当执行 insert() 操作期间，数据库``myNewDatabase`` 和 collection ``myCollection``都将被创建。

```
use myNewDatabase
db.myCollection.insert( { x: 1 } );
```

#### MongoDB CRUD操作

CRUD操作就是 创建，读取，更新，删除 文档 documents。
##### 创建文档
创建或插入操作即向 集合 collection 添加新的 文档 documents。如果插入时集合不存在，插入操作会创建该集合"collection"。

MongoDB中提供了以下方法来插入文档到一个集合：
- db.collection.insert()
- db.collection.insertOne()      //New in version 3.2
- db.collection.insertMany()     //New in version 3.2

在MongoDB中，插入操作作用于单个 集合collection 。MongoDB中所有的写操作在单个 集合 document 的层级上是 原子性。

示例请看[插入文档](http://www.mongoing.com/docs/tutorial/insert-documents.html)

##### 读操作
读操作获取 集合 collection 中的 文档 documents ;例如查询一个集合中的文档。MongoDB提供了如下方法从集合中读取文档:
- db.collection.find()
你可以指定 [条件或者过滤器](http://www.mongoing.com/docs/tutorial/query-documents.html#read-operations-query-argument) 找到指定的文档.

##### 更新操作
更新操作修改 集合 collection 中已经存在的 文档 documents。MongoDB提供了以下方法去更新集合中的文档:

- db.collection.update()
- db.collection.updateOne()       //New in version 3.2
- db.collection.updateMany()      //New in version 3.2
- db.collection.replaceOne()      //New in version 3.2

在MongoDB中,更新操作作用于单个集合。MongoDB中所有的写操作在单个 文档 document 层级上是 原子性的.

你可以指定条件或过滤器来找到要更新的文档。这些[过滤器](http://www.mongoing.com/docs/core/document.html#document-query-filter) 的使用与读操作一样的语法

##### 删除操作

删除是从一个集合中删除文档的操作。MongoDB提供下列方法从集合删除文档。
- db.collection.remove()
- db.collection.deleteOne() New in version 3.2
- db.collection.deleteMany() New in version 3.2

在MongoDB中。删除作作用于单个集合。MongoDB中所有的写操作在单个 文档 document 层级上是 原子性的。



