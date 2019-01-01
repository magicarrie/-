#### 一、用NodeJS写一个web应用

用NodeJS写一个web应用相当于编写事件回调

1. 新建并进入一个文件夹
2. 执行 `npm init`命令，一直回车直到你在文件夹根目录下创建了一个package.json文件
3. 新建一个名为server.js的文件，复制并粘贴下面的代码：

```
//server.js    
const http = require('http'),          
      server = http.createServer();    
server.on('request',(request,response)=>{       
		response.writeHead(200,{'Content-Type':'text/plain'});
		response.write('Hello world');       
		response.end();    
});    
server.listen(3000,()=>{      
		console.log('Node server created at port 3000');    
});
```

1. 在命令行中，输入 `node server.js`，你会看到下面的输出：

```
node server.js    
//Node server started at port 3000
```

打开浏览器并且进入 `localhost:3000`，你应该能够看到一个 `Helloworld`信息。

**详解如下：**

```
引入http模块、createServer()方法创建服务器。
为request事件绑定一个事件回调，传递给on方法的第二个参数。
这个回调函数有2个参数对象，request代表接收到的请求，response代表响应的数据。
response.writeHead() 设置返回报文头部字段，比如状态码和内容类型。
response.write() 对web页面进行写入操作。
response.end() 结束。
listen 告知服务器去监听3000端口，第二个参数是一个回调函数，服务器一启动，这个回调函数就会被执行。
```

#### 二、node路由

​	路由：一个服务器会存储大量的文件，当浏览器发送请求时，会告知服务器他们需要的文件，而服务器会将相应的文件返回给客户端。

```
const
  http = require('http'),
  url = require('url'),
  makeServer = function(request,response){
      let path = url.parse(request.url).pathname;
      console.log(path);
      if(path === '/'){
          response.writeHead(200,{'Content-Type':'text/plain'});
          response.write('Hello world');
      }else if(path === '/about'){
          response.writeHead(200,{'Content-Type':'text/plain'});
          response.write('About page');
      }else if(path === '/blog'){
          response.writeHead(200,{'Content-Type':'text/plain'});
          response.write('Blog page');
      }else{
          response.writeHead(404,{'Content-Type':'text/plain'});
          response.write('Error page');
      }
      response.end();
  },
  server = http.createServer(makeServer);

server.listen(3000,()=>{
    console.log('Node server created at port 3000');
});
```

​	**详解如下：**

```
引入 url 模块
url.parse(request.url) 将请求的路径拆分成protocol、host、path
、querystring等部分
https://www.website.com/about?name=victor
protocol | 	   http    | path | querystring
```

#### 三、Express路由

1. `npm install express --save` 安装Express模块包

2. **express 路由**

   ```
   //server.js
   const
     express = require('express'),
     server = express();
     server.set('port', process.env.PORT || 3000);
   //Basic routes
   server.get('/', (request,response)=>{
     response.send('Home page');
   });
   server.get('/about',(request,response)=>{
     response.send('About page');
   });
   //Express error handling middleware
   server.use((request,response)=>{
     response.type('text/plain');
     response.status(505);
     response.send('Error page');
   });
   //Binding to a port
   server.listen(3000, ()=>{
     console.log('Express server started at port 3000');
   });
   ```

   **详解如下：**

   ```
   引入express模块后，得到一个函数。
   调用这个函数启动服务器。
   server.set()设置监听端口。 process.env.PORT是程序运行时的环境所设置的。如果没有这个设置，默认它的值是3000。
   server.get()/server.post() 两种方式
   ```

   #### 四、使用Node和Express编写RESTful API

   1. **新建一个users.js文件，将数据保存在modules.exports对象中**

      ```
      //users.js
      module.exports.users = [
       {
        name:'Mark',
        age :19,
        occupation:'Lawyer',
        married :true,
        children : ['John','Edson','ruby']
       },{
        name:'Richard',
        age :27,
        occupation:'Pilot',
        married :false,
        children : ['Abel']
      }]
      ```

   2. **server.js**

   ```
   const
     express = require('express'),
     server = express(),
     users = require('./users');
   //setting the port.
   server.set('port', process.env.PORT || 3000);
   //Adding routes
   server.get('/',(request,response)=>{
     response.sendFile(__dirname + '/index.html');
   });
   server.get('/users',(request,response)=>{
     response.json(users);
   });
   //Binding to localhost://3000
   server.listen(3000,()=>{
     console.log('Express server started at port 3000');
   })
   ```

   1. **index.html**

   ```
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="utf-8">
   <title>Home page</title>
   </head>
   <body>
   <button>Get data</button>
   <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
   <script type="text/javascript">
   const btn = document.querySelector('button');
   btn.addEventListener('click',getData);
   function getData(e){
     $.ajax({
         url : '/users',
         method : 'GET',
         success : function(data){
            console.log(data);
         },
         error:function(err){
           console.log('Failed');
         }
     });
   }
   </script>
   </body>
   </html>
   ```

   #### 五、使用socket进行网络连接

   1. **filewatcher.js**

      ```
      const
        net = require('net'),
        fs = require('fs'),
        filename = process.argv[2],
        server = net.createServer((connection)=>{
            console.log('Subscriber connected');
            connection.write(`watching ${filename} for changes`);

            let watcher = fs.watch(filename,(err,data)=>{
                connection.write(`${filename} has changed`);
            });

            connection.on('close',()=>{
                console.log('Subscriber disconnected');
                watcher.close();
            });
        });
      server.listen(3000,()=>console.log('listening for subscribers'));
      ```

      1. **subject.txt**

         ```
         Hello world, I'm gonna change
         ```

      2. **client.js**

         ```
         const
           net = require('net');
         let
           client = net.connect({port:3000});
           client.on('data',(data)=>{
              console.log(data.toString());
           });
         ```

      3. **打开一个终端**

         ```
         //subject.txt会保存在filename变量中
             node filewatcher.js subject.txt
         //监听订阅者
         ```

      4. **打开另一个终端**

         ```
         node client.js
         ```

      5. **修改 subject.txt ,观察客户端的命令行**

         ```
         //subject.txt has changed.
         ```

         **详解如下：**

         ```
         这个demo有两个终端，一个是服务端（filewatcher.js),一个是客户端（client.js)
         引入 'net' 模块,执行网络连接（connect）
         引入 'fs' 模块,读写文件
         process.argv[2]：process是一个全局变量，提供nodejs运行代码的第三个参数（subject.txt）
         net.createServer(callback)：创建一个服务器
         connection.write()：向连接到该端口的客户端发送数据
         ```

#### 一：node.js 的包管理器npm，是世界上最大的开放源代码的生态系统

1、桌面右击 git bash here

2、$ cnpm info jquery 查看jQuery的版本

3、$ cnpm install jquery@1.11.0 （若不指定版本，则默认下载最新版）

4、$ cnpm install bootstrap 下载bootstrap

5、$ cnpm list 查看下载历史

6、$ cnpm list | grep jquery 只查看jQuery的

#### 二：node.js 的功能？

可以解析js代码，没有浏览器安全级别的限制，提供很多系统级别的API，例如 文件的读写 ，进程的管理 ， 网络通信。

#### 三：关于node的一些工具或者框架

Node-Webkit：基于node的webkit的内核

Node-OS：基于node的操作系统

Express：基于node的框架

Jade、EJS：模板

[Log.io](http://Log.io)：浏览器打印各种输出日志，报错等

Grunt、Gulp、Webpack：前端工程化工具

forever、pm2：负载均衡

#### 四：创建服务器

1、使用require指令载入http模块

```
var http = require("http");
```

2、 使用http.createServer( ) 方法创建服务器

```
http.createServer(function(request,response){
	response.writeHead(200, {'Content-Type': 'text/html;charset=utf-8'});
	response.write('Hello Carrie!<br/>');
	response.end('ending...<br/>');
}).listen(8000);
```

3、终端打印信息

`console.log("server is running at http://localhost:8000");`

#### 五：命令行中体验

`$ node` 可以进入控制台，交互式解释器，

输入`1+1` 回车 出现 `2`

#### 六：模块化

基于commonJs规范

```
//定义接口
var util = {
   sayHello：function(res){
  		res.write("111");
   }
}
//暴露接口
module.exports = util;
//引入模块
var util = require("./util.js");
//使用
util.sayHello(response);
```

#### 七： nodeJs API

1、url网址解析

`$ node` 进入node

- url.parse("<https://www.baidu.com>")， 解析网址；


- 将解析出来的用url.format()执行，合成网址；


- url.resolve("<http://www.baidu.com>","/api/list.php")，合并网址；


- querystring.stringify({name:"1000",course:"nodeJs"},"#",":")；把对象解析成字符串
- querystring.parse('name=1000&course=nodeJs') ：{ name: '1000', course: 'nodeJs' } 把字符串解析成对象
- querystring.escape("郑翠翠") ：'%E9%83%91%E7%BF%A0%E7%BF%A0' 把汉字转换成编码
- querystring.unescape('%E9%83%91%E7%BF%A0%E7%BF%A0') ：'郑翠翠' 把编码转换成汉字

### 八：http 爬虫 详见lagou.js

```
response.on("data",function(data){}) //获取数据
response.on("end",function(){})  //获取数据结束
response.on("error",function(err){}) //获取数据错误
```

```
//引入依赖 cheerio 相当于jquery的东西
$ cnpm install cheerio --save-dev    
--save 表示安装到本地   项目依赖   dependencies
-dev  表示安装到能开发依赖  devDependencied  项目上线时不需要开发依赖
```

### 九：文件操作

得到文件与目录的信息：stat

创建一个目录：mkdir

创建文件并写入内容：writeFile,appendFile

读取文件的内容：readFile