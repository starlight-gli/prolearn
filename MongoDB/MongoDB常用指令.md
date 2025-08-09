## **MongoDB 常用命令速查表**

### 1️⃣ 数据库操作

```js
show dbs                          // 查看所有数据库
use my_database                   // 切换（或创建）数据库
db                                // 查看当前数据库名
db.dropDatabase()                 // 删除当前数据库
```

------

### 2️⃣ 集合（Collection）操作

```js
show collections                  // 查看当前数据库所有集合
db.createCollection("users")      // 创建集合
db.users.drop()                    // 删除集合
```

------

### 3️⃣ 插入数据

```js
// 插入单条
db.users.insertOne({ name: "Alice", age: 25 })

// 插入多条
db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 28 }
])
```

------

### 4️⃣ 查询数据

```js
db.users.find()                    // 查询所有
db.users.find().pretty()           // 美化输出
db.users.findOne({ name: "Alice" }) // 查询单条

// 条件查询
db.users.find({ age: { $gt: 25 } }) // age > 25
db.users.find({ age: { $gte: 20, $lte: 30 } }) // 范围查询
db.users.find({ name: { $regex: "^A" } }) // 模糊匹配（以 A 开头）

// 排序 & 分页
db.users.find().sort({ age: -1 })        // 按 age 降序
db.users.find().skip(5).limit(10)        // 跳过5条，取10条
```

------

### 5️⃣ 更新数据

```js
// 更新单条
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
)

// 更新多条
db.users.updateMany(
  { age: { $gt: 25 } },
  { $inc: { age: 1 } } // 年龄 +1
)
```

------

### 6️⃣ 删除数据

```js
db.users.deleteOne({ name: "Charlie" }) // 删除单条
db.users.deleteMany({ age: { $lt: 28 } }) // 删除多条
db.users.deleteMany({}) // 删除所有
```

------

### 7️⃣ 索引操作

```js
db.users.createIndex({ name: 1 })       // 单字段索引
db.users.createIndex({ age: 1, name: -1 }) // 复合索引
db.users.createIndex({ email: 1 }, { unique: true }) // 唯一索引
db.users.getIndexes()                   // 查看索引
db.users.dropIndex("name_1")             // 删除索引
```

------

### 8️⃣ 聚合（Aggregation）常用

```js
// 按 status 分组统计数量
db.orders.aggregate([
  { $group: { _id: "$status", total: { $sum: 1 } } }
])

// 筛选 + 排序 + 投影
db.users.aggregate([
  { $match: { age: { $gt: 25 } } },
  { $sort: { age: -1 } },
  { $project: { name: 1, age: 1, _id: 0 } }
])
```

------

### 9️⃣ 工具命令

```js
db.stats()        // 查看数据库统计信息
db.serverStatus() // 查看服务器运行状态
```

------

📌 **使用提示**：

- `db` 代表当前数据库
- `db.<collection>` 表示选择某个集合
- 命令区分大小写（集合名不区分，字段名区分）