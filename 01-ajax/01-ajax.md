# ajax 局部渲染页面 不用刷新整个页面

​	## 请求方式:

​		get 接收数据 

​		post 发送数据

​		head 信息传递

​		put 发送数据 + 更改数据

​		delete 删除数据 风险操作

## get用法

~~~
function ajax(data){

	//创建实例
	let xhr = new XMLHttpRequset()
	// 0
	
	function join(data){
		let str = "?"
		for(let i in data){
			str += i + "=" + data[i] + "&" 
		}
		// return str.replace(/&$/,"")
		// ?name=midi&age=20
		return str.slice( 0 , -1)
	}
	
	/*
		readyState状态码
			0 请求建立 但没初始化

			1 初始化完成 可以发送请求

			2 发起请求 后台接收完成

			3 后台正在处理中

			4 后台处理完成
	*/
	
	xhr.open( "GET" , "http://localhost:8000" + join(data) , true)
	// 1
	
	xhr.send(null)
	xhr.onreadystatechange = function(){
		//判断readyState状态码 和 http状态码
		if( xhr.readyState == 4 && xhr.status == 200 ){
			//xhr.responseText 是后台返回的数据
			document.body.innerHTML  = xhr.responseText
		}
	}
	
}

var data = {
	name : "midi",
	age : "20"
}
ajax(data)
~~~

## post 用法

~~~
function ajax(data){

	//创建实例
	let xhr = new XMLHttpRequset()
	// 0
	
	function join(data){
		let str = "?"
		for(let i in data){
			str += i + "=" + data[i] + "&" 
		}
		// return str.replace(/&$/,"")
		// ?name=midi&age=20
		return str.slice( 0 , -1)
	}
	
	/*
		readyState状态码
			0 请求建立 但没初始化

			1 初始化完成 可以发送请求

			2 发起请求 后台接收完成

			3 后台正在处理中

			4 后台处理完成
	*/
	
	xhr.open( "POST" , "http://localhost:8000" , true)
	// 1
	
	xhr.send( join(data) )
	xhr.onreadystatechange = function(){
		//判断readyState状态码 和 http状态码
		if( xhr.readyState == 4 && xhr.status == 200 ){
			//xhr.responseText 是后台返回的数据
			document.body.innerHTML  = xhr.responseText
		}
	}
	
}

var data = {
	name : "midi",
	age : "20"
}
ajax(data)
~~~

## get 和 post 的区别

### 用法上:

​	get是通过创建的实例.open()中url中传递数据.

​	post是通过创建的实例.send()中传递数据

## 配置本地后台 app.js

### 配置app.js 

 	1. 安装node
		2. npm install express --save(和app.js在同一个目录下)
		3. npm install body-parser --save 
		4. node app.js(启动项目)
		5. 这个时候localhost:8000 这个端口就可以直接访问了

~~~
const express = require("express")

const bodyParser = require("body-parser")

const app = express()

app.use(bodyParser.json())
app.use(badyParser.urlencoded({
    extended: true
}))
app.get("/",function(req,res){
  	res.header({
        "Access-Control-Allow-Origin":"*"
  	})
  	res.send("这是get请求")
})

app.post("/",function(req,res){
    res.header({
        "Access-Control-Allow-Origin":"*"
    })
    res.send("这是post请求")
})

app.listen(8000,function(){
    console.log("项目启动监听在8000端口")
})
~~~