# Node.js 第3天课堂笔记

## 1.知识点

### 1.1.  模块系统

1. 核心模块
2. 第三方模块
3. 自定义模块
   1. npm
   2. package.json
4. Express-web开发框架
    1. 高度封装了http模块
    2. 第三方web开发工具
    3. 更加专注于业务，而非底层细节
5. 增删改查
    1. 使用文件来保存数据--锻炼异步编码
6. MongoDB
    1. 所有方法都封装好了

### 1.2.  each和foreach

```
{{each 遍历项}}

<li> {{$value}}</li>

{{/each}}
```

### 1.3. 学习方法

​	见得东西多了就好了，多积累不着急速度，一定要吸收

### 1.4. 复习

1. PHP+Apache--默认帮你封装好了很多底层细节操作
2. php中无论代码还是网页，都可以通过web url路径来访问
3. node比较偏底层，很多东西需要你亲自写代码来实现
4. node中开启的服务器默认是黑盒子，所有资源都不允许用户来访问，用户可以访问那些资源由具体的开发人员设计
5. node具有可以把url地址处理的精准漂亮，自定义非常灵活，主要是设计url地址，设计好这个规则，让用户按照你的规则来访问使用
6. 重定向
   1. 301 永久重定向
      1. a.com b.com 
      2. a浏览器不会请求a了，直接跳到b了
   2. 302 临时重定向
      1. a.com b.com
      2. a.com 还会请求a a告诉浏览器你往b走

## 2 Node模块系统

使用Node编写应用程序就是在使用

- EcmaScript
- 核心模块
  - 文件操作 fs
  - http服务 http
  - url路径操作 url
  - os操作系统 os
- 第三方模块
  - art-template 模板引擎
  - 必须通过npm下载才可以使用
- 自定义模块
  - 自己创建的文件

### 2.1. CommonJs模块规范

什么是模块化

- 文件作用域
- 通信规则
  - require 模块加载
  - exports接口对象导出模块中的成员

模块系统

module.exports=（某个方法/变量等）这种方式可以直接导出某一个对象而不用.调用

### 2.1.1 加载 `require`

语法：

`var 自定义变量名称=require（‘模块’）`

两个作用：

1. 执行被加载模块中的代码
2. 得到被加载模块的`exports`导出接口对象
   1. 这个对象代表模块挂载的内容，可以是{方法，变量 、字符串} 导出多个成员，必须对象调用
   2. module.exports这种方式就是直接得到特定的对象，而不用 . 该对象就是该成员 module.exports多次使用，后者会覆盖前者



模块加载规则

1. 模块加载只会执行一次，不会重复执行同一个模块
2. 加载的代码重复了只能拿到其接口对象，但是不会重复里面的代码
3. 这样做的目的是为了避免重复加载，提高效率

判断模块标识

### 2.1.2 原理解析

在node中每个模块内部都有自己的成员对象module

默认在代码的最后有一句return module.exports



## 3 下午总结

1. 301和302的区别
   1. 301永久重定向
   2. 302临时重定向
2. 模块中导出单个成员和模块中导出多个成员的方式
3. module.exports和exports的区别
4. require方法加载规则
   1. 优先从缓存加载
   2. 核心模块
   3. 路径形式的模块
   4. 第三方模块
5. npm常用命令
6. package.json包描述文件
   1. dependencies选项的作用
      - 用来保存项目的第三方包依赖项信息
      - 每个项目都有package.json文件
      - 用npm init  [--yes] 来生成package.json文件
7. Express的基本使用



### 文件操作细节

1. ./data/a.txt 相当于当前目录
2. ./ 可以一起省略 data/a.txt 相当于当前目录
3. /data/a.txt 相当于C/data/a.txt  / 绝对路径 表示根目录 表示当前文件所处磁盘的根目录
4. c:/xx/xx 绝对路径

### 模块操作路径

1. 相对路径中的./不能省略
2. /还是表示当前磁盘的根目录

### nodemon修改完代码后自动重启

这里我们可以使用一个第三方命令行工具，nodemon来帮我们解决频繁修改代码重启服务器问题。

`nodemon`是一个基于node.js开发的一个第三方命令行工具，我们使用的时候需要独立安装

````javascript
npm -install --global nodemon
在任意目录执行这个命令都可以
后面使用node app.js 变成
nodemon app.js 修改完后ctrl+s 修改后服务器自动重启，就不需要手动重启了

````

### 路由

路由其实就是一张表，这个表里面有具体的映射关系。

#### 路由器

- 请求方法
- 请求路径
- 请求函数

### 静态服务

````
提供资源服务的两种方式
1.起别名
app.use('/public/',express.static('./public/'))
访问方式 /public/img/ab3.jpg
2.省略别名，可以直接找子目录访问
app.use(express.static('./public/'))
访问方式 /img/ab3.jpg-
````

### 模板引擎-art-template结合express

##### 读取html等静态资源必须要使用模板引擎

### 安装

````
npm install art-template 
npm install express-art-template 
需要装两个包，express-art-template 依赖于 art-template 
````

### 配置

````
不需要加载，只用配置
配置使用art-template模板引擎
app.engine('art',require('express-art-template'))
第一个参数，表示当渲染以.art结尾的文件时候，使用art-template模板引擎，
express-art-template是专门在express中使用的模板引擎，做了兼容
虽然不用加载art-template包，但仍然需要安装art-template，因为express-art-template依赖了art-template
````

### 渲染

````
Express为response相应对象提供了一个方法：render--渲染
render方法默认是不可以使用的，但是配置了express-art-templata就可以使用了
res.render('html模板名'，{模板数据})
第一个参数不能写路径，默认会去项目中的views目录中去找模板文件
也就是说express有一个约定，开发人员把所有的视图文件都放到views目录中
res.render('index.html',{
		comments:comments,
		// 要这样声明才对
		第一个comments对象的成员，表示index.html里面的变量
		第二个comments是app.js声明的对象数组
		这样右边往左边赋值，index.html中就有了模板数据
	})
````

### get获取参数的方法

````var comment=req.query
url地址栏按回车都是get方式提交
get方式 用req.query获取
    var comment=req.query
	comment.dataTime='2019年8月9日14:19:03'
	comments.push(comment)
````

### post获取参数的方法-第三方包body-parser

express没有内置API获取post请求体数据，我们需要一个第三方包  `body-parser`

安装

````
post方式
表单提交post ，ajax方式提交post
1.需要先安装一个插件 body-parser
npm install --save body-parser
2.配置
var bodyParser=require('body-parser')
这两句是默认的
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
3.使用
app.post('/comment',function(req,res){
	// post方法怎么获取post请求体数据
	console.log(req.body)
	// { name: 'aaaaaaa', message: 'aaaaabbbbbb' }
	var comment=req.body
	comment.dataTime='2019年8月9日15:36:58'
	comments.unshift(comment)
	// 这些方法reponse会自动结束响应
	res.redirect('/')
	console.log(req.body)
})
````

### 从文件中读取数据

#### 1.创建一个db.json的数据文件

````
db.json
{
	"student":[
		{"id":1,"name":"张三","gender":0,"age":18,"hobby":"basketball"},
		{"id":2,"name":"张四","gender":0,"age":18,"hobby":"basketball"},
		{"id":3,"name":"张五","gender":0,"age":18,"hobby":"basketball"},
		{"id":4,"name":"张六","gender":0,"age":18,"hobby":"basketball"},
		{"id":5,"name":"张七","gender":0,"age":18,"hobby":"basketball"}	
	]	
}
````

#### 2.利用fs以及数据格式转换的方式读取文件数据，渲染到页面中

````
	// data这个时候是二进制数据
	// 1.转换成字符串
	// 2.将字符串转换成对象
	// 因为模板渲染的数据是以对象形式保存的
	// 手动转换成对象
	// 用调用的方式获取该对象中的students属性
	var students=JSON.parse(data).students
			res.render('index.html',{
			fruits:fruits,
			students:students
		})
````

2. 