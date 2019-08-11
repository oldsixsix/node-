

#node第四天学习

## CRUD增删改查案例

创建一个web项目

1. npm init -y 一步生成
2. npm install express
3. 新建app.js 
4. 新建views 视图文件
5. 新建public 公共资源

### CRUD案例

##### 1.设计路由

路由设计：你要有哪些页面，怎么访问，需要传递哪些参数，实现什么样的功能

| 功能                 | 请求方法 | 请求路径        | get参数 | post参数 |
| -------------------- | -------- | --------------- | ------- | -------- |
| 首页-渲染首页        | get      | /student        |         |          |
| 添加学生页面         | get      | /student/add    |         |          |
| 处理添加学生请求     | post     | /student/add    |         | name...  |
| 渲染编辑学生信息页面 | get      | /student/edit   |         |          |
| 处理编辑请求         | post     | /student/edit   |         | name...  |
| 处理删除请求         | get      | /student/delete |         |          |

#### 2.路由模块的提取

将.get 、.post涉及路由跳转功能的内容单独提取处理放在router.js文件中

##### 路由跳转内容部分

```javascript
app.get('/student',function(req,res){
	// 如何从文件中读取数据
	
	fs.readFile('./db.json','utf-8',function(err,data){
		if(err){
			return res.status(500).send('server error')
		}
		// data这个时候是二进制数据
		// 1.转换成字符串
		// 2.将字符串转换成对象
		// 因为模板渲染的数据是以对象形式保存的
		// 手动转换成对象
		// 用调用的方式获取该对象中的students属性
	
		var students=JSON.parse(data).student
		// .student调用的是db.json中的对象
			res.render('index.html',{
			fruits:fruits,
			students:students
		})
			console.log('hells')
	})

})

app.get('/student/add',function(req,res){
	
}
app.post('/student/add',function(req,res){
	
}
app.get('/student/edit',function(req,res){
	
}
app.post('/student/edit',function(req,res){
	
}

app.get('/student/delete',function(req,res){
	
}
```

##### 实现router.js和app.js的配置功能

1. ##### 使用原生方法----模块加载的方式

   ```javascript
   相当于把router.js中的跳转全部封装到一个函数function(app)中
   将此函数挂载到module.exports，然后再另一个模块中加载，传递app参数，把其当做方法一样使用
   module.exports =  function(app){
   	app.get('/',function(req,res){
   		// 如何从文件中读取数据
   		
   		fs.readFile('./db.json','utf-8',function(err,data){
   			if(err){
   				return res.status(500).send('server error')
   			}
   			// data这个时候是二进制数据
   			// 1.转换成字符串
   			// 2.将字符串转换成对象
   			// 因为模板渲染的数据是以对象形式保存的
   			// 手动转换成对象
   			// 用调用的方式获取该对象中的students属性
   			var fruits=['苹果','香蕉','橘子','草莓']
   			var students=JSON.parse(data).student
   			// .student调用的是db.json中的对象
   				res.render('index.html',{
   				fruits:fruits,
   				students:students
   			})
   				console.log('hells')
   		})
   	
   	})
   	
   	app.get('/student/add',function(req,res){
   		
   	})
   	app.post('/student/add',function(req,res){
   		
   	})
   	app.get('/student/edit',function(req,res){
   		
   	})
   	app.post('/student/edit',function(req,res){
   		
   	})
   	
   	app.get('/student/delete',function(req,res){
   		
   	})
   	
   }
   
   ```

2. ##### 使用express提供的router方法

   router.js返回一个路由对象
   
   把路由函数全部挂载到router对象中
   
   ````javascript
   var fs=require('fs')
   
   // 1.渲染首页
   
   var express=require('express')
   
   // 创建路由对象
   var router=express.Router()
   
   // 将app改成router，将路由操作挂载到router上
   router.get('/',function(req,res){
   		// 如何从文件中读取数据
   		
   		fs.readFile('./db.json','utf-8',function(err,data){
   			if(err){
   				return res.status(500).send('server error')
   			}
   			// data这个时候是二进制数据
   			// 1.转换成字符串
   			// 2.将字符串转换成对象
   			// 因为模板渲染的数据是以对象形式保存的
   			// 手动转换成对象
   			// 用调用的方式获取该对象中的students属性
   			var fruits=['苹果','香蕉','橘子','草莓']
   			var students=JSON.parse(data).student
   // .student调用的是db.json中的对象，之前错误是写成了studnets了，获取不到db.json的数据
   				res.render('index.html',{
   				fruits:fruits,
   				students:students
   			})
   				console.log('hells')
   		})
   	
   	})
   	
   	router.get('/student/add',function(req,res){
   		
   	})
   	router.post('/student/add',function(req,res){
   		
   	})
   	router.get('/student/edit',function(req,res){
   		
   	})
   	router.post('/student/edit',function(req,res){
   		
   	})
   	
   	router.get('/student/delete',function(req,res){
   		
   	})
   	
   	// 最后要返回路由对象
   	module.exports=router
   ````
   
   app.js中如何加载路由对象
   
   ````javascript
   // 在app.js中加载router对象
   var router=require('./router.js')
   //在服务器上挂载路由 ，
   //必须在engine和body-parser之后
   app.use(router)
   //配置模板引擎和中间件必须在挂载路由之前,
   //因为根据执行流程,路由中用到了模板引擎和中间件,必须现在加载,再使用
   ````

### 完成添加学生页面的设计add.html





#### 如何把读出来的数据写到文件中

文件中的数据都是字符串

读出来的字符串转换成对象，再转成字符串再存储



#### 封装提取学生数据的API

如果需要获取一个函数中异步操作的结果，则必须通过回调函数来获取

这里的回调函数，以及异步操作的内容没有懂

### JS中的回调函数

##### 同步和异步

同步：a和b两个事件，必须等a完成才能执行下一项b，这就是同步

异步：a和b两个事件，b不用等a完成后再执行，可以同时执行，这就是异步

##### 回调函数定义

Google定义

`A callback is a function that is passed as an argument to another function and is executed after its parent function has completed.`

回调函数一个函数用来传递参数给另一个函数并且它在其父函数执行完之后再执行。

### 关于回调函数与js单线程以及js异步机制

js是单线程，我们不需要考虑各个线程之间的通信，js引擎是一件一件事情的去完成，有需要执行的事件都需要排队，等待，触发和执行。可是如果这样的话，如果在队列中有一件事情需要花费很多的事件，那么后面的任务都将处于一种等待状态，那么浏览器甚至会出现假死现象，假如其中有意见正在执行的一个任务是一个死循环，那么会导致后续其他的任务无法正常执行，所以js在同步机制的缺陷下设计出了异步模式

#### 异步模式

在异步执行的模式下，每一个异步的任务都要其自己的一个或者多个回调函数，这样在执行的异步任务执行完之后，不会马上执行时间队列中下一项任务，而是执行它的回调函数，而下一项任务也不会等待这个回调函数执行完，因为它也不确定当前的回调何时执行完，只要它被触发就会执行。即后一个任务不等待前一个任务结束就执行，所以任务的执行顺序和任务的排列顺序是不一致的是异步的。

#### JS实现异步的原理

js是单线程语言，即同一时间只能做一件事，那js如何实现异步的，异步和单线程不是自相矛盾么？js本身不能实现异步，但是js的宿主环境：浏览器,Node是多线程的，宿主环境通过某种方式（事件驱动）使得js具备了异步的特性

回调函数的目的：获取异步操作结果。

凡是需要得到一个函数内部异步操作的结果。

那些是异步操作：setTimeout 定时器 readFile writeFile 读写文件操作 ajax 这些都是js异步编程的特色

这种情况必须通过：回调函数。回调函数是在其父函数执行完之后再执行的



```javascript
function add(x,y,callback){
    console.log(1)
    setTimeout(function(){
        var ret=x+y
这是在函数内部执行的函数，异步操作，即父函数执行完才执行setTimeout函数，为了拿到setTimeout函数的返回值，必须使用回调函数，回调函数的目的就是为了获取异步操作的结果，因为回调函数在父函数执行完之后才执行的目的，就可以拿到异步操作的结果。
		callback(ret) //这里就是封装了一个回调函数，目的就是获取这个结果
如何使用回调函数，增加一个callback参数，
    },1000)
}

最后调用函数
add(10,20,function(){
    这就是callback回调函数，上面的callback只是形参，这里相当于实参
    x--10
    y--20
    callback--function
    console.log(ret)
    这样就通过回调函数拿到了异步操作的结果
    
})
```

#### 如何实现读写文件中获取数据--同理异步操作中获取结果

### 增删查改API student.js

#### 1.编写获取学生列表API

**获取学生信息API**

````javascript
exports.findAll = function (callback) {
  // 因为fs读取文件是异步操作，但是我们又要拿读取文件操作的数据
  // 要获取异步操作中的数据，必须使用回调函数
 	var dbPath = './db.json' 
    doPath是数据文件的路径
  fs.readFile(doPath,'utf8',function(err,data){
	  if(err){
		  // 这是拿异步操作的err数据
		  // 报错还要结束,必须用return
		  return callback(err)
	  }
	  // y因为callback第一个参数已经传递了err，所以data就不能放在第一个参数，要不然不好区分。
	  // 没有报错，err传递null,
	  // 此时data是二进制数据，要么读文件的加utf8编码,要么转换成字符串格式
	  // 然后用JSON.parse()方法，将字符串转换成对象,获取里面的student属性
	  callback(null,JSON.parse(data.student))
  })
}
````

**在路由中使用该API**

```javascript
router.get('/',function(req,res){
		// 如何从文件中读取数据
		这是没有数据操作的工具类
	 	fs.readFile('./db.json','utf-8',function(err,data){
			if(err){
				return res.status(500).send('server error')
			}
			// data这个时候是二进制数据
			// 1.转换成字符串
			// 2.将字符串转换成对象
			// 因为模板渲染的数据是以对象形式保存的
			// 手动转换成对象
			// 用调用的方式获取该对象中的students属性
			var fruits=['苹果','香蕉','橘子','草莓']
			var students=JSON.parse(data).student
			// .student调用的是db.json中的对象
				res.render('index.html',{
				fruits:fruits,
				students:students
			})
				console.log('hells')
		})

    
	// 使用数据操作API进行渲染
	//  callback(null,JSON.parse(data.student)) 这个是其参数
	//function(err,students) 这里面的students相当于已经拿到了db.json中的学生对象了
	// JSON.parse(data.student)==students
	// 回调函数就是为了获取异步操作的结果，拿到了students
	Daostudent.findAll(function(err,students){
		// 这里面就不用读数据那些复杂的操作，只处理渲染的业务，
		// 功能更加清楚了
		if(err){
			// 这里来实现具体的业务逻辑
			return res.send('server error')
		}
		// 模板引擎渲染 这里拿到了students的对象
		res.render('index.html',{
			fruits:[
			'苹果',
			'香蕉',
			'橘子'
			],
			students:students
		})
	})
	
	})
```

#### **2.保存待添加学生信息API**

**步骤**

````
1. 函数传递的参数（student,callback） 
   1. student：带保存的学生对象
   2. callback(err)：回调函数用于获取错误信息，因为保存只需要把数据写到文件中，而不用取出来，所以只有一个参数err
2. 读取db.json文件中的student对象
3. 将带保存的对象添加到student对象（db.json文件中的）中
4. 控制id值不唯一，通过对象数组中最后一个对象id值+1的方式给带保存对象赋值
5. 将添加好的student对象数组写入db.json
   1. 把json对象通过stringify方法转换成字符串，而且还要给其赋别名{{student:students}},保证json文件中的数据格式与原来一样
   2. writeFile(dbpath,data,function(err))，这里的data是字符串形式的，读写文件只能是字符串或者2进制数据
````

**实现代码**

````javascript
exports.save=function(student,callback){
	// 保存学生信息
	fs.readFile(dbPath,'utf8',function(err,data){
		if(err){
			// 如果读取失败，callback获取错误信息
			return callback(err)
		}
		// 首先获取db.json中的学生对象
		var students=JSON.parse(data).student
	
		// 控制id ，唯一且不重复
		// 123456,通过对象数组的长度来控制
		// 拿到列表中最后一个对象的id+1,这样就能保证id不为1
		student.id=students[students.length-1].id+1
		
		// 添加待保存的学生信息
		students.push(student)
		
		// 把json对象数组转换为字符串
		var studentsString=JSON.stringify({
			student:students
			// studets:studets,     
			// 第一个students是为对象数组起的名字
			// 第二个才是真正要转换的json对象
		})
		console.log(studentsString)
		
		// 把字符串写到db.json中
		fs.writeFile(dbPath,studentsString,function(err){
			if(err)
			{
				callback(err)
			}
			// 如果成功 err等于空 错误对象就是空
			callback(null)
		})
		
	})
}
````

#### 3.渲染编辑学生页面

**步骤**

1. 首先在页面中点编辑，跳转到编辑页面要拿到你所需要编辑的那条数据。

   1. 在编辑跳转的href上传递参数id

      `href="/student/edit?id={{$value.id}} `

      `id={{$value.id}}`一定要是键值对的形式，要不然获取的数据无效，无法处理

   2. 在`/student/edit`页面通过`req.body.id`拿到你要编辑的id值，然后再db.json中查找该对象。

   3. 把对象渲染到`/student/edit`页面上

**封装根据ID查找对象的API**

```javascript
用到Ecmascript6中的find方法，参数为一个函数
查找满足return  parseInt(item.id) === parseInt(id)条件，返回符合该条件的对象
用callback获取该id值的对象
exports.getbyId=function(id,callback){
	fs.readFile(dbPath,'utf8',function(err,data){
		if(err)
		{
			callback(err)
		}
		var students=JSON.parse(data).student
		var stu=students.find(function(item){
			return  parseInt(item.id) === parseInt(id)
		})
		// 这里要拿stu对象了
		callback(null,stu)
	})
}
```

渲染`/student/edit`页面

为了格式的统一，把id值全部转化成int类型parseInt(req.query.id)

```javascript
router.get('/student/edit',function(req,res){
		// 通过id找对象，需要在数据操作中封装一个方法，这样利于代码的可维护性
		console.log(req.query.id)
		Daostudent.getbyId(parseInt(req.query.id),function(err,student){
			// student拿到了student
			if(err)
			{
				return console.log('edit渲染失败')
			}
			console.log(student)
			res.render('edit.html',{
				student:student
//根据你要编辑的对象作为模板数据进行渲染，              
			})
		})
	})
```



#### 4.保存更新学生数据

**步骤**

1. 函数参数（student，callback）

   1. student为修改后的学生对象的所有信息
   2. callback取回错误信息

2. 需要修改谁就要通过ID把谁找出来

   - 这里用Ecmascript中的方法 find，需要接收函数作为参数，当符合return条件时返回该对象
   - ` var stu = students.find(function (item) {
           return item.id === student.id
         })`

3. 找到待更新的对象之后，就要给对该对象属性值进行更新

   1.  **for in 在js数组和对象上使用**

   2. for (var key in student) {
            stu[key] = student[key]
          }`

   3. **key代表键 student[key]代表值 这是一组键值对**

   4. **js中的对象是与数组关联在一起的**

      

4. 最后就是将对象添加到db.json中和之前的操作一样

5. ·· 传值是不行的，一定要传键值对 

6. href=/student/edit？id={{student.id}}

**封装更新学生数据API**

```javascript
exports.update = function(student,callback){
fs.readFile(dbPath,'utf8',function(err,data){
		if(err)
		{
			// 回调函数拿异步操作的结果
			return callback(err)
		}
		// 拿db.json中的学生数据
		var students=JSON.parse(data).student
		// 细节，此时拿到的id是字符串类型
		 student.id = parseInt(student.id)
		
		// 在students中找到需要修改的student对象,用Ecmascript6中的find方法
		// 传递一个函数,当满足return item.id=student.id返回该对象
		var stu = students.find(function(item){
			return parseInt(item.id)===student.id
		})
		// 找到了待修改的对象stu，现在遍历属性进行修改,键值对的方式修改，遍历student中的键值对
		for(var key in student)
		{
			stu[key]=student[key]
		}
		console.log('11')
			console.log(students)
		// 再将修改后的students重新写入db.json
		var studentString=JSON.stringify({
			// var students=JSON.parse(data).student 这里用了.调用了其属性，要还原
			student:students
		})
		fs.writeFile(dbPath,studentString,function(err){
			if(err)
			{
				callback(err)
			}
			callback(null)
		})
	})
	
}
```

**渲染加重定向**

```javascript
router.post('/student/edit',function(req,res){
		Daostudent.update(req.body,function(err){
			if(err){
				return console.log('保存更新信息失败')
			}
			console.log(req.body)
			res.redirect('/')
		})
	})
```



#### 5.删除学生信息

1. 与编辑类似，点击删除，传递id值到删除页面
2. 根据id值加findindex,找到待删除对象的下标
3. 然后根据index+splice方法删除students中的对象
4. 最后将students写入数据库

**封装删除学生信息API**--通过id作为媒介

```javascript
exports.deleteById= function(id,callback){
	// 通过id寻找对象
	fs.readFile(dbPath,'utf8',function(err,data){
		if(err)
		{
			callback(err)
		}
		var students=JSON.parse(data).student
		// findIndex 和find类型，但是这里是返回下标
		var deleteId=students.findIndex(function(item){
		return	parseInt(item.id)===parseInt(id)
		})
		// 参数说明(下标开始,删除几个)
		students.splice(deleteId,1)
		
		var studentString=JSON.stringify({
			// var students=JSON.parse(data).student 这里用了.调用了其属性，要还原
			student:students
		})
		
		fs.writeFile(dbPath,studentString,function(err){
			if(err)
			{
				callback(err)
			}
			callback(null)
		})
		})
}
```

**重定向**

```javascript
router.get('/student/delete',function(req,res){
		 Daostudent.deleteById(req.query.id,function(err){
			 if(err)
			 {
				 console.log('删除失败')
			 }
			 res.redirect('/')
		 })
	})
```



### 自己编写的步骤

1. 处理模板
2. 配置开放静态资源
3. 简单路由，渲染静态页，先能看到页面出来
4. 路由设计--路由设计表
5. 提取路由模块--router.js
6. 由于接下来一系列操作都需要处理文件数据，所以我们需要封装数据操作方法 ---student.js
   1. 查询所有学生列表的API
   2. 保存添加学生信息
   3. 更新学生信息
   4. 删除学生信息
7. 实现具体功能
   1. 通过路由收到请求
   2. 接受请求中的数据（get/post）
      1. req.query ---post
      2. req.body ----get
   3. 调用数据处理API
   4. 渲染数据到页面上
8. 列表功能顺序
   1. 列表
   2. 添加
   3. 编辑
   4. 删除



### 模块化思想

模块如何划分：依据模块职责要单一





### JSON知识补充



### Jquery知识补充

### 闭包的原理