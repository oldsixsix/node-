# node第五天学习

目前的前端情况使用很多新技术，然后利用编译器工具打包在低版本浏览器运行

### package.json和package-lock.json

npm5以前是不会有package-lock.json这个文件的，npm5以后才加入这个文件的。当你安装包的时候，npm都会生成或者更新package-lock.json这个文件

npm5以后的安装包不需要--save，它会自动保存依赖信息。

当你安装包的时候，它

### EcmaScript6

#### find

1. 接收一个方法作为参数，方法内部返回一个条件结束，符合该条件的元素会作为find方法的返回值，如果没有则返回undefined
2. 会遍历所有元素，执行你给定的带有条件返回值的函数

#### findIndex

接收一个方法作为参数，方法内部返回一个条件结束，符合该条件的元素会作为find方法的元素的下标，如果没有则返回undefined

# MongoDB

### 关系型数据库和非关系型数据库

#### 关系型数据库

表就是关系，或者说表与表之间存在关系

- 所有的关系型数据都需要`sql`语句来操作
- 所有的关系型数据在操作之前都需要设计表结构
- 而且数据表还支持约束
  - 主键
  - 非空
  - 外键
  - 默认值

#### 非关系型数据库

就是键值对，key-value 没有表的概念

但是在MongoDB中是长得最像关系型数据库的非关系型数据库

### MongoDB

- 数据库--数据库
- 数据表--集合（数组）
- 表记录--文档对象

MongoDB数据库不需要设计表结构

可以任意的往里面存数据，没有结构性一说，非常灵活

安装--查看版本信息，检查是否安装成功

![1565512638217](C:\Users\吴壮壮\AppData\Roaming\Typora\typora-user-images\1565512638217.png)



#### mongod的启动

##### 第一次启动前需要手动创建db目录，作为数据存储目录

d: /data/db

MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。请注意，数据目录应该放在根目录下（(如： C:\ 或者 D:\ 等 )。

```
在命令行中，在D盘的根目录下手动创建/data/db
D:\>mkdir data

D:\>cd data

D:\data>mkdir db

D:\data>cd db

D:\data\db>
```

##### 启动

在命令行中输入

`mongod`

启动成功

![1565513859832](C:\Users\吴壮壮\AppData\Roaming\Typora\typora-user-images\1565513859832.png)

**想要修改默认的数据存储目录**

`mongod --dbpath='存储目录'`

##### 停止

`在开启的服务的控制台，直接ctrl+c即可停止或者直接关闭开启服务的控制台也行`

##### 连接数据库

输入命令

```
mongo
```

会自动连接本机的数据库

连接成功截图

![1565513824635](C:\Users\吴壮壮\AppData\Roaming\Typora\typora-user-images\1565513824635.png)

##### 退出数据库

在连接状态输入

```e
exit

它会回复一个bye
```



#### 基本命令

```
show dbs
查询所有数据库

use 数据库名称
切换到指定数据库（如果没有该数据库，就会新建）

db
查看当前操作的数据

插入数据
db.students.insertOne({"name":"jack"})
会自动创建一个对象，插入一个对象

```



### node.js来连接操作mongodb数据库

##### 方式1-使用官方的MongoDB包来操作

比较原生，真正开发不用这个

##### 方式2-使用第三方mongoose

第三方包：`mongoose`基于MongoDB官方的mongod包再做一次封装网址