# MongoDB - 连接

### 启动 MongoDB 服务

在你只需要在 MongoDB 安装目录的 bin 目录下执行 **mongodb** 即可。

### **简化版 MongoDB 连接字符串说明**

#### **基础格式**（类比网址）

text

```
mongodb://用户名:密码@服务器地址:端口/默认数据库?参数
```

**就像访问网站**：`https://admin:123456@example.com:80/dashboard?lang=zh`



标准 URI 连接语法：

```
mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]
```

- `mongodb://`：协议头，表示使用 MongoDB。

- `[username:password@]`：（可选）认证信息，包括用户名和密码。

- `host1[:port1][,...hostN[:portN]]`：服务器地址和端口，可以是一个或多个 MongoDB 服务器的地址和端口。

- `/[defaultauthdb]`：（可选）默认认证数据库。

- `[?options]`：（可选）连接选项。

  标准的连接格式包含了多个选项(options)，如下所示：

  - `authSource`：指定认证数据库。

  - `replicaSet`：指定副本集的名称。

  - `ssl`：启用 SSL 连接（true 或 false）。

  - `readPreference`：指定读偏好，如 `primary`, `primaryPreferred`, `secondary`, `secondaryPreferred`, `nearest`。

  - `connectTimeoutMS`：指定连接超时时间（毫秒）。

  - `socketTimeoutMS`：指定套接字超时时间（毫秒）。

连接到本地 MongoDB 实例（默认端口 27017）：

```
mongodb://localhost
```

连接到本地 MongoDB 实例，指定数据库：

```
mongodb://localhost/mydatabase
```

使用用户名和密码连接到本地 MongoDB 实例：

```
mongodb://username:password@localhost/mydatabase
```

连接到远程 MongoDB 实例：

```
mongodb://remotehost:27017
```

连接到副本集（Replica Set）：

```
mongodb://host1:27017,host2:27017,host3:27017/mydatabase?replicaSet=myReplicaSet
```

使用 SSL 连接到 MongoDB：

```
mongodb://username:password@localhost:27017/mydatabase?ssl=true
```

使用多个选项连接：

```
mongodb://username:password@localhost:27017/mydatabase?authSource=admin&ssl=true
```

### Python (PyMongo)

## 实例

```python
from pymongo import MongoClient

client = MongoClient('mongodb://user:password@localhost:27017/mydatabase?authSource=admin')
db = client['mydatabase']
```

