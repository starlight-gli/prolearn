## 创建数据库

### 语法

当你使用 **use** 命令来指定一个数据库时，如果该数据库不存在，MongoDB将自动创建它。

MongoDB 创建数据库的语法格式如下：

```javascript
use DATABASE_NAME
```

```javascript
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 

```

```javascript
> db.runoob.insertOne({"name":"菜鸟教程"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
runoob  0.000GB

```

## 创建集合

```javascript
use myNewDatabase
db.createCollection("myNewCollection")
```

## 删除数据库

```
use myDatabase
db.dropDatabase()
```

## MongoDB 更新文档

在 MongoDB 中，更新文档的操作可以使用多种方法实现，常用的方法包括 **updateOne()、updateMany()、replaceOne() 和 findOneAndUpdate()**。

## MongoDB 删除文档

**使用场景：**

- **数据清理**：删除不再需要的旧数据或无效数据。
- **数据修正**：在数据修正过程中删除错误的或重复的文档。
- **自动化任务**：在自动化脚本或任务中，根据特定条件删除文档。

```javascript
db.collection.deleteOne(filter, options)
```

```jav
db.collection.deleteMany(filter, options)
```

```javascript
db.collection.findOneAndDelete(filter, options)
```

## MongoDB 查询文档

暂略

## MongoDB 索引

### 创建索引

MongoDB 使用 createIndex() 方法来创建索引。