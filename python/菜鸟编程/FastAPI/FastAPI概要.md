# FastAPI概要

这段代码展示了一个简单的 FastAPI 应用，包含了路由定义、路径参数、查询参数和请求体处理。下面我来详细解析每一部分：

## 实例讲解

```Python
from typing import Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    price: float
    is_offer: Union[bool, None] = None


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}


@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```





### 1. 导入模块

python

```Python
from typing import Union
from fastapi import FastAPI
from pydantic import BaseModel
```

- `typing.Union`: 用于定义可以是**多种类型的参数**
- `fastapi.FastAPI`: FastAPI 的核心类，用于**创建应用实例**
- `pydantic.BaseModel`: 用于**创建数据模型**（用于请求/响应数据验证）

### 2. 创建 FastAPI 实例

python

```Python
app = FastAPI()
```

- 创建 FastAPI 应用的核心对象
- 所有路由和设置都将挂载到这个对象上

### 3. 定义数据模型 (Pydantic Model)

```Python
class Item(BaseModel):
    name: str
    price: float
    is_offer: Union[bool, None] = None
```

- 创建了一个 `Item` 模型用于处理请求体数据
- 字段说明：
  - `name`: 必填的字符串字段
  - `price`: 必填的浮点数字段
  - `is_offer`: 可选的布尔字段（默认为 `None`）

### 4. 定义根路由 (GET /)

```Python
@app.get("/")
def read_root():
    return {"Hello": "World"}
```

- `@app.get("/")`: 注册一个处理 GET 请求的路由，路径为根路径 `/`
- 当访问 `http://127.0.0.1:8000/` 时返回 JSON 响应 `{"Hello": "World"}`

### 5. 带路径参数和查询参数的路由 (GET /items/{item_id})

python

```Python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```

- 路径参数：`{item_id}` 是动态路径参数
  - FastAPI 会自动将值转换为 `int` 类型（如果无效则返回错误）
- 查询参数：`q` 是可选的字符串查询参数
  - 访问示例：`/items/42?q=searchterm`
- 响应示例：`{"item_id": 42, "q": "searchterm"}`

### 6. 带请求体的路由 (PUT /items/{item_id})

python

```Python
@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```

- HTTP 方法：`PUT` 用于更新资源
- 路径参数：`item_id` (整数)
- 请求体参数：`item` (使用前面定义的 `Item` 模型)
  - FastAPI 会自动验证请求体是否符合 `Item` 模型定义
  - 自动生成文档和交互式测试界面
- 响应示例：`{"item_name": "NewItem", "item_id": 42}`

### 关键特性总结

1. **自动类型转换**：
   - 路径参数 `item_id` 自动转换为整数
   - 无效类型会返回清晰的错误信息
2. **自动文档生成**：
   - 访问 `/docs` 可以看到所有路由的交互式文档
   - 包含参数说明、类型要求和测试功能
3. **数据验证**：
   - 使用 Pydantic 模型验证请求体
   - 例如 `Item` 要求 `name` 为字符串，`price` 为浮点数
4. **依赖注入**：
   - 函数参数自动从不同位置获取：
     - 路径参数：`item_id`
     - 查询参数：`q`
     - 请求体：`item`
5. **默认值处理**：
   - `q: Union[str, None] = None` 表示查询参数可选
   - `is_offer: Union[bool, None] = None` 表示请求体字段可选

### 使用示例

1. 启动应用：

bash

```Python
uvicorn main:app --reload
```

1. 测试端点：

- `GET /` → `{"Hello": "World"}`

- `GET /items/5?q=test` → `{"item_id":5,"q":"test"}`

- `PUT /items/5` 携带 JSON 体：

  json

  ```json
  {
    "name": "New Item",
    "price": 9.99,
    "is_offer": true
  }
  ```

  返回：`{"item_name":"New Item","item_id":5}`

这个简单示例展示了 FastAPI 的核心功能：路由定义、参数处理、数据验证和自动文档生成，是构建 RESTful API 的绝佳起点。

## FastAPI基本路由

### 根路径路由

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```



### 路径参数

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

- `@app.get("/items/{item_id}")`：定义了一个路由路径，其中 `{item_id}` 是路径参数，对应于函数参数 `item_id`。
- `def read_item(item_id: int, q: str = None)`：路由处理函数接受一个整数类型的路径参数 `item_id` 和一个可选的字符串类型查询参数 `q`。

## FastAPI 请求和响应

在 FastAPI 中，**请求**（Request）和**响应**（Response）是与客户端交互的核心。

HTTP相关内容可以参考：[HTTP请求方法](https://www.runoob.com/http/http-tutorial.html)

### 请求数据

#### 查询参数

#### 路径参数

#### 请求体



### 响应数据

#### 返回JSON数据

路由处理函数返回一个字典，该字典将被 FastAPI 自动转换为 JSON 格式，并作为响应发送给客户端

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/")
def read_item(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

#### 返回`Pydantic`模型

路由处理函数返回一个`Pydantic`模型实例，`FastAPI` 将自动将其转换为` JSON` 格式，并作为响应发送给客户端。

```python
from pydantic import BaseModel
from fastapi import FastAPI

app = FastAPI()
class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
def create_item(item: Item):
    return item
```

### 请求头和Cookie

使用 Header 和 Cookie 类型注解获取请求头和 Cookie 数据。

```python
from fastapi import Header, Cookie
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/")
def read_item(user_agent: str = Header(None), session_token: str = Cookie(None)):
    return {"User-Agent": user_agent, "Session-Token": session_token}
```

### 重定向和状态码

以上代码在浏览器访问 **http://127.0.0.1:8000/redirect/** 会自动跳转到 **http://127.0.0.1:8000/items/** 页面

使用 `HTTPException` 抛出异常，返回自定义的状态码和详细信息。

以下实例在 **item_id** 为 **42** 会返回 **404** 状态码：

### 自定义响应头

```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int):
    content = {"item_id": item_id}
    headers = {"X-Custom-Header": "custom-header-value"}
    return JSONResponse(content=content, headers=headers)
```

## `FastAPI Pydantic` 模型

### 定义`Pydantic`模型

`Pydantic` 是一个用于数据验证和序列化的 Python 模型库。

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None
```

### 使用`Pydantic`模型

#### 请求体验证

在 `FastAPI` 中，可以将 `Pydantic` 模型用作请求体（Request Body），以自动验证和解析客户端发送的数据。

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
def create_item(item: Item):
    return item
```

#### 查询参数验证

`Pydantic` 模型还可以用于验证查询参数、路径参数等。

```python
from fastapi import FastAPI, Query
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.get("/items/")
def read_item(item: Item, q: str = Query(..., max_length=10)):
    return {"item": item, "q": q}
```

##### 关键点解析

1. **`Query` 参数验证**

   ```python
   q: str = Query(..., max_length=10)
   ```

   - `Query` 是 `FastAPI` 提供的特殊类，用于对查询参数进行额外验证和元数据设置
   - `...` (Ellipsis) 表示这个参数是**必需的**（不能省略）
   - `max_length=10` 设置最大长度限制（不能超过10个字符）
   - 效果：`q` 是必须提供的查询参数，且长度不能超过10个字符

2. **`Item` 模型作为参数**

   ```python
   item: Item
   ```

   - 这里 `Item` 是一个 `Pydantic` 模型
   - **关键点**：在 GET 请求中使用模型参数是**特殊用法**
   - `FastAPI` 会尝试从：
     - 查询参数（URL 参数）
     - 请求体（虽然 GET 通常没有请求体）
     - 表单数据
   - 来解析数据并填充模型

3. **`Item` 模型定义**

   python

   ```python
   class Item(BaseModel):
       name: str  # 必需字段
       description: str = None  # 可选字段，默认None
       price: float  # 必需字段
       tax: float = None  # 可选字段
   ```

   - 定义了数据结构和验证规则
   - 字段类型提示确保自动类型转换和验证

##### 工作原理

这个端点 `/items/` 期望接收两种参数：

1. **查询参数 `q`**
   - 必须提供
   - 字符串类型
   - 长度 ≤ 10 字符
2. **模型参数 `item`**
   - 需要提供组成 Item 模型的字段
   - 可以通过多种方式提供：
     - 作为查询参数：`/items/?q=test&name=Book&price=29.99`
     - 作为表单数据
     - 作为 `JSON` 请求体（虽然 GET 请求通常没有请求体）

##### 请求示例

##### 有效请求 1（通过查询参数）：

```http
GET /items/?q=short&name=Book&price=29.99
```

```json
{
  "item": {
    "name": "Book",
    "description": null,
    "price": 29.99,
    "tax": null
  },
  "q": "short"
}
```

##### 有效请求 2（通过表单或请求体）：

text

```
POST /items/?q=short
Content-Type: application/x-www-form-urlencoded

name=Book&price=29.99
```

（虽然使用 GET 方法，但 FastAPI 允许这种特殊用法）

##### 无效请求 1（缺少 q 参数）：

text

```
GET /items/?name=Book&price=29.99
```

响应：422 错误（缺少必需的 q 参数）

##### 无效请求 2（q 参数太长）：

text

```
GET /items/?q=thisiswaytoolong&name=Book&price=29.99
```

响应：422 错误（q 长度超过10字符）

##### 无效请求 3（缺少 Item 必需字段）：

text

```
GET /items/?q=test&name=Book
```

响应：422 错误（缺少必需的 price 字段）

##### 重要注意事项

1. **GET 请求中的模型参数**：

   - 这种用法是非标准的 RESTful 实践
   - 通常 GET 请求只使用查询参数，复杂数据应该用 POST
   - FastAPI 支持这种用法主要是为了灵活性

2. **实际应用建议**：

   - 对于复杂数据，更推荐使用 POST 方法

   - 修改为 POST 会更符合常规：

     python

     ```
     @app.post("/items/")
     def create_item(item: Item, q: str = Query(..., max_length=10)):
         ...
     ```

3. **Query 的高级用法**：

   - 添加描述：`Query(..., description="搜索关键词")`
   - 添加别名：`Query(..., alias="search-term")`
   - 正则验证：`Query(..., regex="^[a-z]+$")`
   - 多值参数：`q: list[str] = Query([])`

##### 总结

这段代码演示了：

1. 使用 `Query` 进行查询参数的增强验证
2. 在 GET 请求中使用 Pydantic 模型接收结构化数据
3. FastAPI 自动参数解析和验证的强大功能
4. 请求数据的多种传递方式

虽然技术上可行，但在实际开发中，对于复杂数据更推荐使用 POST/PUT 方法配合请求体，这样更符合 HTTP 规范。GET 请求应主要用于简单查询参数的数据获取。



这些 HTTP 响应头是由 FastAPI 和 Uvicorn 服务器自动生成的，用于描述服务器响应的重要元数据。让我详细解释每个头部的来源和作用：

##### HTTP 响应头详解

1. **`content-length: 79`**
   - **来源**：Uvicorn 服务器自动计算
   - **作用**：表示响应体的字节大小（这里是 79 字节）
   - **重要性**：客户端（如浏览器）根据这个值准确读取响应内容
2. **`content-type: application/json`**
   - **来源**：FastAPI 框架自动设置
   - **作用**：声明响应内容的格式是 JSON
   - **为什么重要**：告诉客户端如何解析响应数据（这里是 JSON 格式）
3. **`date: Sun,03 Aug 2025 13:57:39 GMT`**
   - **来源**：Uvicorn 服务器自动生成
   - **作用**：服务器生成响应的时间（GMT 时区）
   - **重要性**：用于缓存控制和时间敏感操作
4. **`server: uvicorn`**
   - **来源**：Uvicorn 服务器自动添加
   - **作用**：标识服务器软件是 Uvicorn
   - **为什么出现**：这是 Web 服务器的标准标识方式

![deepseek_mermaid_20250803_01b740](https://cdn.jsdelivr.net/gh/starlight-gli/image-host/img/deepseek_mermaid_20250803_01b740.png)

##### 这些头部是如何产生的？

1. **FastAPI 处理阶段**：
   - 框架自动将你的 Python 字典转换为 JSON
   - 设置 `content-type: application/json` 头部
   - 执行你定义的路由处理逻辑
2. **Uvicorn 服务器阶段**：
   - 计算响应体大小 → 添加 `content-length`
   - 添加当前时间 → `date` 头部
   - 标识服务器软件 → `server: uvicorn`
   - 处理 HTTP 协议细节

##### 如何自定义这些头部?

```python
from fastapi import FastAPI, Response

app = FastAPI()

@app.post("/custom/")
def custom_response(response: Response):
    # 设置自定义头部
    response.headers["X-Custom-Header"] = "MyValue"
    
    # 修改现有头部（不推荐修改标准头部）
    response.headers["server"] = "MyServer"
    
    return {"message": "Custom headers"}
```



#### 自动文档生成

#### 数据转换和序列化



## FastAPI 路径操作依赖项

### 1、依赖项（Dependencies）

允许你声明应用中不同部分所需的依赖关系，从而实现代码重用、简化复杂逻辑和提高可测试性。

#### 1.1 什么是依赖项？

依赖项是一种在路径操作函数执行**之前**运行的函数或类，它可以：

1. 共享代码逻辑
2. 处理数据库连接
3. 执行身份验证
4. 管理权限检查
5. 注入配置或共享资源

#### 1.2 依赖项的两个处理方面

- **预处理（Before）依赖项：** 在路由操作函数执行前运行，用于预处理输入数据，验证请求等。
- **后处理（After）依赖项：** 在路由操作函数执行后运行，用于执行一些后处理逻辑，如日志记录、清理等。

#### 1.3 依赖注入

* 依赖注入是将**依赖项注入到路由操作函数中**的过程。

* 在 FastAPI 中，通过在路由操作函数参数中声明依赖项来实现依赖注入。

* FastAPI 将负责解析依赖项的参数，并确保在执行路由操作函数之前将其传递给函数。

#### 1.4 依赖项的定义

##### 1.4.1定义依赖项

*  定义：

```python
from fastapi import Depends, FastAPI

app = FastAPI()

# 依赖项函数
def common_parameters(q: str = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}
```

* 使用：

```python
from fastapi import Depends

# 路由操作函数
@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons
```

在这个例子中，common_parameters 是一个依赖项函数，用于预处理查询参数。

### 2、依赖项的类型

#### 2.1 函数依赖项

```
def get_db():
    db = SessionLocal()
    try:
        yield db  # 使用yield实现上下文管理
    finally:
        db.close()

@app.get("/users/{user_id}")
def read_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(User).filter(User.id == user_id).first()
    return user
```

#### 2.2 类依赖项

```
class Pagination:
    def __init__(self, page: int = 1, size: int = 10):
        self.page = page
        self.size = size

@app.get("/products/")
def list_products(pagination: Pagination = Depends(Pagination)):
    skip = (pagination.page - 1) * pagination.size
    return {"page": pagination.page, "size": pagination.size}
```

#### 2.3 嵌套依赖项

依赖项可以依赖其他依赖项：

```
def get_current_user(token: str = Depends(oauth2_scheme)):
    # 验证token并返回用户
    return User(...)

def get_current_active_user(user: User = Depends(get_current_user)):
    if user.disabled:
        raise HTTPException(...)
    return user

@app.get("/me")
def read_me(user: User = Depends(get_current_active_user)):
    return user
```

### 3、路径操作依赖项的基本使用

#### 3.1 预处理（Before）

#### 3.2 后处理（After）

```python
from fastapi import Depends, FastAPI, HTTPException

app = FastAPI()

# 依赖项函数
def common_parameters(q: str = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

# 路由操作函数
@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

# 后处理函数
async def after_request():
    # 这里可以执行一些后处理逻辑，比如记录日志
    pass

# 后处理依赖项
@app.get("/items/", response_model=dict)
async def read_items_after(request: dict = Depends(after_request)):
    return {"message": "Items returned successfully"}
```



##### 什么是async？

`async` 是 Python 中用于定义**异步函数**的关键字，它是 Python 异步编程（asyncio）的核心。理解 `async` 对于开发高性能、高并发的应用程序至关重要。

### 3、多个依赖项的组合

### 4、异步依赖项

### 5、实际应用

#### 5.1 身份验证

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def verify_token(token: str = Depends(oauth2_scheme)):
    # 实际应用中这里会有JWT验证逻辑
    if token != "secret":
        raise HTTPException(status_code=401, detail="Invalid token")
    return {"username": "admin"}

@app.get("/protected")
def protected_route(user: dict = Depends(verify_token)):
    return {"message": f"Hello {user['username']}, you accessed a protected route"}
```

#### 5.2 数据库会话管理

```python
from sqlalchemy.orm import Session

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.post("/users")
def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = User(**user.dict())
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user
```

#### 5.3 权限检查

```
def check_admin(user: dict = Depends(verify_token)):
    if user["username"] != "admin":
        raise HTTPException(status_code=403, detail="Admin access required")
    return user

@app.delete("/users/{user_id}")
def delete_user(user_id: int, 
               user: dict = Depends(check_admin),
               db: Session = Depends(get_db)):
    # 删除用户逻辑
    return {"message": "User deleted"}
```

#### 5.4 请求参数预处理

```python
def parse_json_body():
    async def dependency(request: Request):
        body = await request.json()
        return body
    return Depends(dependency)

@app.post("/process")
def process_data(data: dict = Depends(parse_json_body())):
    # 处理数据
    return {"processed": data}
```



### 6、总结

* 依赖项系统是 FastAPI 的核心特性之一，它使得构建复杂、可维护的 API 变得简单而优雅。通过合理使用依赖项，你可以创建出结构清晰、易于维护且功能强大的应用程序。

## FastAPI 表单数据

FastAPI 提供了 Form 类型，可以用于声明和验证表单数据。

## FastAPI核心概念

FastAPI 是一个现代、快速（高性能）的 Python Web 框架，用于构建 API。它基于标准 Python 类型提示，使用 Starlette 和 Pydantic 构建。

### 1、主要特点

1. **高性能**：与 NodeJS 和 Go 相当，是最快的 Python 框架之一
2. **快速开发**：开发速度提高约 200%-300%
3. **减少错误**：减少约 40% 的人为错误
4. **直观**：强大的编辑器支持，自动补全无处不在
5. **简单**：易于使用和学习，减少阅读文档的时间

### 2、ASGI 与异步编程

#### 2.1 什么是ASGI？

ASGI（Asynchronous Server Gateway Interface）是 Python 异步 Web 服务器和应用程序之间的标准接口。

### 3、HTTP方法与状态码

### 4、json数据格式
