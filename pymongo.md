# Pymongo

[【**pymongo官方中文文档入口**】](https://www.mongodb.com/zh-cn/docs/languages/python/pymongo-driver/current/)

【**前提**】需要先下载、配置MongoDB（远程服务或本地都可）

【判断是否安装成功（Windows）】打开终端，输入：

```cmd
mongod --version
```

如果输出类似：

``````cmd
db version vxx.xx.xx
``````

说明安装成功

【拓展】在新版的MongoDB的下载中不包含Mongo shell，需要自行下载Mongo shell并配置环境变量

打开终端，输入：

``````cmd
mongosh --version
``````

**一、安装（以pip包管理器为例）**

```python
pip install pymongo
```

以下pymongo的版本是4.16.0

**二、连接数据库（同步）**

**（1）未开启认证（no_auth）**

适用于本地开发、MongoDB没开启权限控制（默认）

【特点】

* 不需要用户名和密码
* **前提：MongoDB没开启auth（即MongoDB 没有启用 authorization）**

在配置文件（一般在MongoDB的安装目录的bin目录下，可查看.cfg或.conf文件）里通常是：

``````yaml
security:
  authorization: disabled
``````

或

``````yaml
#security:
``````

【示例】

``````python
from pymongo import MongoClient

# 1. url方式
url= "mongodb://localhost:27017/"
client = MongoClient(url)
db = client['test_db']

# 2. 参数方式
client = MongoClient(
	host="localhost",
    port=27017
)
db = client['test_db']
``````

**（2）用户名密码链接（auth）**

适用于生产环境、云数据库（如Atlas）

【示例】

``````python
from pymongo import MongoClient

# 1. url方式（不指定数据库）
url = "mongodb://username:password@localhost:27017"
client = MongoClient(url)
db = client['test_db']

# 2. url方式（指定数据库）
url = "mongodb://username:password@localhost:27017/test_db"
db = MongoClient(url)

# 3. 参数方式
client = MongoClient(
	host="localhost",
    port=27017,
    username="username",
    password="password"
)
db = client['test_db']
``````

> [!NOTE]
>
> **如何判断当前MongoDB是no_auth还是auth？**
>
> （1）在Python环境中判断
>
> ``````python
> from pymongo import MongoClient
> 
> client = MongoClient("mongodb://localhost:27017/")
> db = client.test
> db.list_collection_names()
> ``````
>
> + 如果报错：`OperationFailure: command listCollections requires authentication` => auth
>
> + 如果正常输出 => no_auth
>
> （2）使用Mongo shell命令行判断
>
> 打开终端，输入：
>
> ``````cmd
> mongosh
> ``````
>
> 进入后执行：
>
> ``````cmd
> show dbs
> ``````
>
> + 如果能正常列出数据库 => no_auth
> + 如果报错：`MongoServerError: command listDatabases requires authentication` => auth

**三、增删改查操作**

【创建集合】

``````python
col = db['test_col'] # 创建集合
``````

**插入文档—增**

> [!NOTE]
>
> **`_id`字段必须是唯一的**
>
> 在MongoDB集合中，每个文档都必须包含具有唯一值的 `_id`字段
>
> 如果为 `_id`字段指定值，则必须确保该值在集合中是唯一的。如果不指定值，驱动程序会自动为该字段生成唯一的 `ObjectId` 值
>
> [摘自MongoDB官方文档]

（1）insert_one()

``````python
col.insert_one(document={'name':'zhangshan','age':18})
``````

还可以将自定义类的实例传递给`insert_one()`方法，自定义类需继承`TypedDict`类

``````python
from typing import TypedDict # python 3.8+

class Student(TypedDict):
    name:str
    age:int

stu = Student(name="zhangsan",age=18)
col.insert_one(document-stu)
``````

（2）insert_many()

``````python
``````



**删**



**改**



**查**















