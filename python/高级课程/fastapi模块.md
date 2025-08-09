# FastAPI

## 一、 环境配置

### 1. 配置环境

安装fastapi（python web 框架）和uvicorn（类似于uwsgi的web服务器）

### 2. 前后端分离和前后端不分离

1. 前后端不分离
2. 前后端分离

### 3. restful风格的api

基础写法

/addstudent/

/pudatastudent/

/deletestudent/

/selectstudent/

改造写法

/student/

/student/

/student/

/student/



| 请求方法 | 请求地址 | 后端操作        |
| -------- | -------- | --------------- |
| POST     | /book/   | 增加图书        |
| GET      | /book/   | 获取所有图书    |
| GET      | /book/1  | 获取id为1的图书 |
| PUT      | /book/1  | 修改id为1的图书 |
| DELETE   | /book/1  | 删除id为1的图书 |

### 4. 快速创建一个`fastapi`项目



