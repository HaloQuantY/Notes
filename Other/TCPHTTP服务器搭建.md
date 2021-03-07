### TCP/HTTP服务器搭建

- TCP服务器
  - nodejs中net模块可以建立TCP服务器
- HTTP
  - nodejs模块自带HTTP模块
  - express框架

#### TCP服务器

​	使用nodejs中net模块可以创建TCP服务器，net模块主要是server模块和socket模块。

##### net模块组成

- net.Server
  - 用于创建服务器和绑定服务器事件回调函数
  - 在回调函数中会返回net.Socket对象
- net.Socket
  - 用于绑定socket连接事件回调函数

##### net.Server

- net.createServer()
  - 创建一个Server对象
- server.listen(port, host)
  - port为端口号，整数型
  - host为ip地址，字符串型
  - 根据传递的参数会创建TCP或IPC服务器
- server.close()
  - 关闭服务器
- server绑定事件回调函数
  - listening事件：指定监听端口host后触发
  - connection事件：有连接建立时触发
    - 回调函数会自动传入一个socket对象
  - close事件：关闭服务器时触发

```javascript
// app.js
const net = require('net');
const PORT = 3000;

const server = net.createServer();

server.on('listening', () => {
   console.log(`Server running on ${PORT}`);
});

server.on('close', () => {
   console.log('Server closed'); 
});

server.on('connection', (socket) => [
   console.log('New connection built');
   server.close();
]);

server.listen(PORT, '127.0.0.1');
```

##### net.Socket

​	用于创建和管理一个连接，nodejs中可以使用server的connection事件回调函数获得socket对象，而要直接创建socket对象使用net.connect()方法，返回一个socket对象。

- net.connect(port, host)
  - 创建一个socket连接，返回socket对象
  - 传入port为端口号，整型
  - host为ip地址，字符串
- socket.write(data)
  - 发送数据，传入的data可以是整型，字符串等类型
- socket.end()
  - 关闭socket连接
- socket事件
  - connect
    - 创建连接时使用，一般在回调函数中指定data事件回调函数
  - end
  - data
    - 获得数据时触发，回调函数中会自动传入data数据（二进制）
  - error

```javascript
// client.js
const net = require('net');
const PORT = 3000;

const socket = net.connect(PORT, '127.0.0.1');

socket.on('error', () => {
   console.log('Connecting failed');
});

socket.on('connect', () => {
   console.log('New connection built');

   socket.on('data', (buffer) => {
      console.log(buffer.toString());
      socket.end();
   });
});

socket.on('end', () => {
   console.log('Connection ended');
});
```



#### demo：在线计算器

​	通过TCP服务器连接，在client.js中输入计算表达式，app.js中获取表达式并返回计算表达式结果。

##### nodejs输入输出

​	readline模块用于在nodejs中输入表达式

```javascript
const readline = require('readline');

const rl = readline.createInterface({
   input: process.stdin,
   output: process.stdout
});

rl.on('line', (str) => {
   if (str === 'close') {
      rl.close();
   }
   console.log(str);
});
```

##### demo代码

```javascript
// app.js
const net = require('net');
const PORT = 3000;

// 创建TCP服务器
const server = net.createServer();

// 绑定事件回调函数
server.on('listening', () => {
   console.log(`Server running on port ${PORT}`);
});

server.on('close', () => {
   console.log('Server closed');
});

server.on('connection', (socket) => {
   console.log('New connection built');
   socket.write('Calculator(Input your line plz):');
	
    // 接收表达式，使用eval函数直接取得运行结果
   socket.on('data', (buffer) => {
      const line = buffer.toString();
      const result = eval(line);
      socket.write(`result: ${result}`);
   });
});

// 监听端口
server.listen(PORT, '127.0.0.1');
```

```javascript
// client.js
const readline = require('readline');
const net = require('net');
const PORT = 3000;

// 创建socket连接
const socket = net.connect(PORT, '127.0.0.1');

// 绑定事件回调函数
socket.on('end', () => {
   console.log('Connection ended');
});

socket.on('error', () => {
   console.log('Connecting failed');
});

socket.on('connect', () => {
   console.log('New connection built');
});

socket.on('data', (buffer) => {
    // 接收结果，进行显示
   console.log(buffer.toString());
});

// 输入处理
const rl = readline.createInterface({
   input: process.stdin,
   output: process.stdout
});

rl.on('line', (str) => {
   if (str === 'close') {
      rl.close();
   }
    // 发送表达式
   socket.write(str);
});
```

​	运行结果：

```powershell
PS D:\vsCode_local\tcp_server> node app.js
Server running on port 3000
New connection built

PS D:\vsCode_local\tcp_server> node client.js
New connection built
Calculator(Input your line plz):
1+1
result: 2
2+2
result: 4
2*100000
result: 200000
173*229
result: 39617
```



#### nodejs原生HTTP模块建立服务器

```javascript
const HTTP = require('HTTP');
const fs = require('fs');
```

​	要实现基本的HTTP服务器需要用到nodejs的HTTP模块和fs模块，用于创建服务器和读取本地文件。

##### HTTP模块

- 创建服务器
  - HTTP.createServer((req, res) => {})
  - 参数是回调函数，当有HTTP连接到监听端口时触发
- 设置回调函数
  - 回调函数的两个参数是req和res，对应HTTP请求报文和相应报文
  - req常用req.url获得请求的url地址，解析之后可以进行路由
  - res要响应需要进行两步，res.writeHead()和res.end()
    - 前者修改响应头，后者修改响应体
    - 修改响应头需要有对应MIME
- 监听端口
  - server.listen(port, host)

##### demo：实现一个简单静态网站

```javascript
const path = require('path');
const http = require('http');
const fs = require('fs');
const url = require('url');

const Mime = {
   '.html': 'text/html;charset=UTF-8',
   '.css': 'text/css',
   '.png': 'image/png',
   '.jpeg': 'image/jpeg',
   '.jpg': 'image/jpeg',
   '.gif': 'image/gif'
};

http.createServer((req, res) => {
   let pathname = url.parse(req.url).pathname;
   let extname = path.extname(pathname);

   res.writeHead(200, {'Content-type': 'text/plain;charset=UTF-8', 'Access-Control-Allow-Origin': '*'});

   if (pathname === '/') {
      fs.readFile('./index.html', (err, data) => {
         if (err) {
            fs.readFile('./static/404.html', (err, data) => {
               res.writeHead(404, {'Content-type': 'text/html;charset=UTF-8'});
               res.end(data);
            });
         } else {
            res.writeHead(200, {'Content-type': 'text/html;charset=UTF-8'});
            res.end(data);
         }
      });
   } else {
      fs.readFile('.' + pathname, (err, data) => {
         if (err) {
            fs.readFile('./static/404.html', (err, data) => {
               res.writeHead(404, {'Content-type': 'text/html;charset=UTF-8'});
               res.end(data);
            });
         }
         res.writeHead(200, {'Content-type': `${Mime[extname]}`});
         res.end(data);
      });
   }


})
.listen(3000, '127.0.0.1');
```



#### express框架搭建HTTP服务器

```javascript
const express = require('express');
const app = express();
```

##### 创建服务器

- express()
- app.listen()

````javascript
const express = require('express');
const PORT = 3000;

const app = express();

app.listen(PORT, () => {
   console.log(`Server running on ${PORT}`); 
});
````

##### 服务器响应

- app.get/post/put/delete
- res.send()
  - send方法会自动检测响应内容类型并自动设置http状态码和contentType

```javascript
const express = require('express');
const PORT = 3000;

const app = express();

app.get('/', (req, res) => {
   res.send('Hello World'); 
});

app.listen(PORT, () => {
   console.log(`Server running on ${PORT}`); 
});
```

##### 使用中间件

​	中间件就是用于处理HTTP请求的方法，可以使用多个中间件，服务器将按顺序使用中间件。

- next方法

  - 默认情况下，请求匹配到一个中间件就会终止匹配，需要在中间件中使用next（）交给下个中间件才会链式处理
  - next方法要在回调函数参数中声明

  ```javascript
  app.get('/', (req, res, next) => {
  	req.name = 'add name';
  	next();
  });
  ```

- use方法

  - use方法匹配所有请求方式
  - 第一个参数可以指定请求地址

##### 错误处理中间件

​	同步错误：

```javascript
app.use((err, req, res, next) => {
	rse.status(500).send(err.message); 
    // 会捕捉同步运行过程中报的错误
});
```

​	异步错误：

```javascript
app.get('/', (req, res, next) => {
   fs.readFile('test.txt', (err, data) => {
       if (err) {
           // 使用next方法将err对象传到错误处理中间件
           next(err);
       }
   }) ;
});
```

##### 模块化路由

- express.Router()
  - 返回一个路由对象
- 创建不同路由
  - 一级路由使用use函数，对地址进行路由，并应用路由对象
  - 二级路由将使用路由对象上的get/post/put/delete方法进行路由

```javascript
const express = require('express');
const home = express.Router();
home.get('/home', (req, res) => {
   res.sendFile('/index.html'); 
});

const app = express();

app.use('/home', home);

app.listen(3000);
```

##### 获得GET参数

- req.query
  - 获得GET参数，内部将会将GET参数转换为对象返回

```javascript
app.get('/query', (req, res) => {
   res.send(req.query);
});
```

##### 获得POST参数

- 使用第三方包body-parser

  ```javascript
  const bodyParser = require('body-parser');
  
  app.use(bodyParser.urlencoded({ extended: false }));
  
  app.use(bodyParser.json());
  
  app.post('/add', (req, res) => {
      res.send(req.body);
  });
  ```

##### 获得路由参数

- req.params
  - 直接使用该属性获得req.params

##### 静态资源处理

- express.static

  ```javascript
  app.use(['virtualPath', ]express.static('public'));
  // 其中‘public’是静态资源所在路径，推荐使用绝对路径path.resolve
  // virtualPath是一个可选的虚拟路径参数
  ```

  

