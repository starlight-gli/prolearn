# MongoDB

每行即每个数据，每列即每个数据的属性（关系）

每个集合，每个集合包括一个文档。Binary JSON

## Windows安装

可以使用GUI来连接和管理

## NoSQL简介

### NoSQL 简史

> select fun, profit from real_world where relational=false;

因此，对NoSQL最普遍的解释是"非关联型的"，强调Key-Value Stores和文档数据库的优点，而不是单纯的反对RDBMS。

### CAP定理（CAP theorem）

或者叫做**布鲁尔定理**

它指出对于一个分布式计算系统来说，不可能同时满足以下三点:

- **一致性(Consistency)** (所有节点在同一时间具有相同的数据)
- **可用性(Availability)** (保证每个请求不管成功或者失败都有响应)
- **分区容错性(Partition tolerance)** (系统中任意信息的丢失或失败不会影响系统的继续运作)

### NoSQL 数据库分类

| 类型          | 部分代表                                         | 特点                                                         |
| ------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 列存储        | Hbase、Cassandra、Hypertable                     | 顾名思义，是按列存储数据的。最大的特点是方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或者某几列的查询有非常大的IO优势。 |
| 文档存储      | MongoDB、CouchDB                                 | 文档存储一般用类似**json**的格式存储，存储的内容是文档型的。这样也就有机会对某些字段建立索引，实现关系数据库的某些功能。 |
| key-value存储 | Tokyo Cabinet / TyrantBerkeley DBMemcacheDBRedis | 可以通过key快速查询到其value。一般来说，存储不管value的格式，照单全收。（Redis包含了其他功能） |
| 图存储        | Neo4J、FlockDB                                   | 图形关系的最佳存储。使用传统关系数据库来解决的话性能低下，而且设计使用不方便。 |
| 对象存储      | db4o、Versant                                    | 通过类似面向对象语言的语法操作数据库，通过对象的方式存取数据。 |
| xml数据库     | Berkeley DB XML、BaseX                           | 高效的存储XML数据，并支持XML的内部查询语法，比如XQuery,Xpath。 |

## MongoDB简介

### 1、什么是MongoDB？

文档型数据库，数据类似JSON的文档形式存储

MongoDB使用**集合**（Collections）来组织**文档**（Documents），每个文档都是由**键值对**组成的。

- **数据库（Database）**：存储数据的容器，类似于关系型数据库中的数据库。
- **集合（Collection）**：数据库中的一个集合，类似于关系型数据库中的表。
- **文档（Document）**：集合中的一个数据记录，类似于关系型数据库中的行（row），以 BSON 格式存储。

![img](https://cdn.jsdelivr.net/gh/starlight-gli/image-host/img/crud-annotated-document.png)

### 2、执行基本操作：

连接成功后，可以执行各种 MongoDB 数据库操作。例如：

#### 2.1 数据库操作

| 命令                | 说明               | 示例                |
| :------------------ | :----------------- | :------------------ |
| `show dbs`          | 显示所有数据库     | `show dbs`          |
| `use <db>`          | 切换/创建数据库    | `use mydb`          |
| `db.dropDatabase()` | 删除当前数据库     | `db.dropDatabase()` |
| `db.stats()`        | 查看当前数据库状态 | `db.stats()`        |

------

#### 2.2 集合操作

| 命令                        | 说明             | 示例                           |
| :-------------------------- | :--------------- | :----------------------------- |
| `show collections`          | 显示所有集合     | `show collections`             |
| `db.createCollection(name)` | 创建集合         | `db.createCollection("users")` |
| `db.<collection>.drop()`    | 删除集合         | `db.users.drop()`              |
| `db.<collection>.stats()`   | 查看集合统计信息 | `db.users.stats()`             |

------

#### 2.3 文档CRUD操作

##### 2.3.1  插入文档

| 命令           | 说明         | 示例                                                    |
| :------------- | :----------- | :------------------------------------------------------ |
| `insertOne()`  | 插入单条文档 | `db.users.insertOne({name: "张三", age: 25})`           |
| `insertMany()` | 批量插入文档 | `db.users.insertMany([{name: "李四"}, {name: "王五"}])` |

##### 2.3.2  查询文档

| 命令               | 说明           | 示例                                        |
| :----------------- | :------------- | :------------------------------------------ |
| `find()`           | 查询所有文档   | `db.users.find()`                           |
| `findOne()`        | 查询第一条文档 | `db.users.findOne({age: 25})`               |
| `find().pretty()`  | 格式化输出结果 | `db.users.find().pretty()`                  |
| `countDocuments()` | 统计文档数量   | `db.users.countDocuments({age: {$gt: 20}})` |

* 常用查询操作符号：

```javascript
// 比较操作符
db.users.find({age: {$gt: 25}})  // 大于
db.users.find({age: {$lt: 30}})  // 小于
db.users.find({age: {$ne: 25}})  // 不等于

// 逻辑操作符
db.users.find({$or: [{age: 25}, {name: "张三"}]})

// 正则匹配
db.users.find({name: {$regex: /张/}})
```

##### 2.3.3 更新文档

| 命令           | 说明         | 示例                                                         |
| :------------- | :----------- | :----------------------------------------------------------- |
| `updateOne()`  | 更新单条文档 | `db.users.updateOne({name: "张三"}, {$set: {age: 26}})`      |
| `updateMany()` | 批量更新文档 | `db.users.updateMany({is_active: true}, {$set: {status: "verified"}})` |
| `replaceOne()` | 替换整个文档 | `db.users.replaceOne({name: "张三"}, {name: "张三", age: 27})` |

* 更新操作符：

  - `$set`: 设置字段值

  - `$unset`: 删除字段

  - `$inc`: 字段值增减

  - `$push`: 向数组追加元素

##### 2.2.4 删除文档

| 命令           | 说明         | 示例                                        |
| :------------- | :----------- | :------------------------------------------ |
| `deleteOne()`  | 删除单条文档 | `db.users.deleteOne({name: "张三"})`        |
| `deleteMany()` | 批量删除文档 | `db.users.deleteMany({status: "inactive"})` |

#### 2.3 索引操作

| 命令            | 说明         | 示例                               |
| :-------------- | :----------- | :--------------------------------- |
| `createIndex()` | 创建索引     | `db.users.createIndex({email: 1})` |
| `getIndexes()`  | 查看所有索引 | `db.users.getIndexes()`            |
| `dropIndex()`   | 删除索引     | `db.users.dropIndex("email_1")`    |

* 索引类型：

  - `1`: 升序索引

  - `-1`: 降序索引

  - `"text"`: 文本索引

#### 2.4 快速参考表（常用操作符）

| 操作符    | 说明       | 示例                       |
| :-------- | :--------- | :------------------------- |
| `$eq`     | 等于       | `{age: {$eq: 25}}`         |
| `$ne`     | 不等于     | `{age: {$ne: 25}}`         |
| `$in`     | 在数组中   | `{age: {$in: [25, 30]}}`   |
| `$nin`    | 不在数组中 | `{age: {$nin: [25, 30]}}`  |
| `$exists` | 字段存在   | `{email: {$exists: true}}` |
| `$regex`  | 正则匹配   | `{name: {$regex: /张/}}`   |

### 3、MongoDB概念解析

在 MongoDB 中基本的概念是**文档**、**集合**、**数据库**。

| SQL 术语/概念 | MongoDB 术语/概念 | 解释/说明                           |
| :------------ | :---------------- | :---------------------------------- |
| database      | database          | 数据库                              |
| table         | collection        | 数据库表/集合                       |
| row           | document          | 数据记录行/文档                     |
| column        | field             | 数据字段/域                         |
| index         | index             | 索引                                |
| table joins   |                   | 表连接,MongoDB不支持                |
| primary key   | primary key       | 主键,MongoDB自动将_id字段设置为主键 |

* **完整术语列表：**
  - **文档（Document）**：MongoDB 的基本数据单元， JSON-like 的结构，可以包含多种数据类型。
  - **集合（Collection）**：集合是一组文档的容器。
  - **数据库（Database）**：包含一个或多个集合的 MongoDB 实例。
  - **BSON**：Binary JSON 的缩写，是 MongoDB 用来存储和传输文档的二进制形式的 JSON。
  - **索引（Index）**：用于优化查询性能的数据结构，可以基于集合中的一个或多个字段创建索引。
  - **分片（Sharding）**：一种分布数据到多个服务器（称为分片）的方法，用于处理大数据集和高吞吐量应用。
  - **副本集（Replica Set）**：一组维护相同数据集的 MongoDB 服务器，提供数据的冗余备份和高可用性。
  - **主节点（Primary）**：副本集中负责处理所有写入操作的服务器。
  - **从节点（Secondary）**：副本集中的服务器，用于读取数据和在主节点故障时接管为主节点。
  - **MongoDB Shell**：MongoDB 提供的命令行界面，用于与 MongoDB 实例交互。
  - **聚合框架（Aggregation Framework）**：用于执行复杂的数据处理和聚合操作的一系列操作。
  - **Map-Reduce**：一种编程模型，用于处理大量数据集的并行计算。
  - **GridFS**：用于存储和检索大于 BSON 文档大小限制的文件的规范。
  - **ObjectId**：MongoDB 为每个文档自动生成的唯一标识符。
  - **CRUD 操作**：创建（Create）、读取（Read）、更新（Update）、删除（Delete）操作。
  - **事务（Transactions）**：从 MongoDB 4.0 开始支持，允许一组操作作为一个原子单元执行。
  - **操作符（Operators）**：用于查询和更新文档的特殊字段。
  - **连接（Join）**：MongoDB 允许在查询中使用 `$lookup` 操作符来实现类似 SQL 的连接操作。
  - **TTL（Time-To-Live）**：可以为集合中的某些字段设置 TTL，以自动删除旧数据。
  - **存储引擎（Storage Engine）**：MongoDB 用于数据存储和管理的底层技术，如 WiredTiger 和 MongoDB 的旧存储引擎 MMAPv1。
  - **MongoDB Compass**：MongoDB 的图形界面工具，用于可视化和管理 MongoDB 数据。
  - **MongoDB Atlas**：MongoDB 提供的云服务，允许在云中托管 MongoDB 数据库。

#### 3.1 文档(Document)

文档是一组键值(key-value)对(即 BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。

一个简单的文档例子如下：

```json
{"site":"www.runoob.com", "name":"菜鸟教程"}
```

下表列出了 RDBMS 与 MongoDB 对应的术语：

| RDBMS  | MongoDB                           |
| :----- | :-------------------------------- |
| 数据库 | 数据库                            |
| 表格   | 集合                              |
| 行     | 文档                              |
| 列     | 字段                              |
| 表联合 | 嵌入文档                          |
| 主键   | 主键 (MongoDB 提供了 key 为 _id ) |

#### 3.2 集合

集合就是 MongoDB 文档组

#### 3.3 元数据

## 元数据

数据库的信息是存储在集合中。它们使用了系统的命名空间：

```
dbname.system.*
```

在MongoDB数据库中名字空间 <dbname>.system.* 是包含多种系统信息的特殊集合(Collection)，如下:

| 集合命名空间             | 描述                                      |
| :----------------------- | :---------------------------------------- |
| dbname.system.namespaces | 列出所有名字空间。                        |
| dbname.system.indexes    | 列出所有索引。                            |
| dbname.system.profile    | 包含数据库概要(profile)信息。             |
| dbname.system.users      | 列出所有可访问数据库的用户。              |
| dbname.local.sources     | 包含复制对端（slave）的服务器信息和状态。 |

对于修改系统集合中的对象有如下限制。

在{{system.indexes}}插入数据，可以创建索引。但除此之外该表信息是不可变的(特殊的drop index命令将自动更新相关信息)。

{{system.users}}是可修改的。 {{system.profile}}是可删除的。

#### 3.4 MongoDB数据类型

| 数据类型           | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| String             | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
| Integer            | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。 |
| Boolean            | 布尔值。用于存储布尔值（真/假）。                            |
| Double             | 双精度浮点值。用于存储浮点值。                               |
| Min/Max keys       | 将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。 |
| Array              | 用于将数组或列表或多个值存储为一个键。                       |
| Timestamp          | 时间戳。记录文档修改或添加的具体时间。                       |
| Object             | 用于内嵌文档。                                               |
| Null               | 用于创建空值。                                               |
| Symbol             | 符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。 |
| Date               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
| Object ID          | 对象 ID。用于创建文档的 ID。                                 |
| Binary Data        | 二进制数据。用于存储二进制数据。                             |
| Code               | 代码类型。用于在文档中存储 JavaScript 代码。                 |
| Regular expression | 正则表达式类型。用于存储正则表达式。                         |

##### ObjectID

ObjectId 类似唯一主键，可以很快的去生成和排序，包含 12 bytes，含义是：

- 前 4 个字节表示创建 **unix** 时间戳,格林尼治时间 **UTC** 时间，比北京时间晚了 8 个小时
- 接下来的 3 个字节是机器标识码
- 紧接的两个字节由进程 id 组成 PID
- 最后三个字节是随机数

MongoDB 中存储的文档必须有一个 _id 键。这个键的值可以是任何类型的，默认是个 ObjectId 对象

由于 ObjectId 中保存了创建的时间戳，所以你不需要为你的文档保存时间戳字段，你可以通过 getTimestamp 函数来获取文档的创建时间:

```javascript
> var newObject = ObjectId()
> newObject.getTimestamp()
ISODate("2017-11-25T07:21:10Z")
```

ObjectId 转为字符串

```javascript
> newObject.str
5a1919e63df83ce79df8b38f
```