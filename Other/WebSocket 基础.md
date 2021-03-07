## WebSocket 基础

​	websocket是一种传输协议，用来解决HTTP协议只能够由单方面向服务器传输数据，而又需要服务器实时传输数据的情况。

​	websocket对于服务端和浏览器都有支持，HTML5中已经默认支持了websocket协议并提供了API，nodejs可以根据各种第三方模块来操作websocket。

### websocket在HTML5中支持

- 创建websocket连接
  - const ws = new WebSocket('serverPath');
- open事件
  - ws.onOpen = function() {};
- message事件
  - ws.onMessage = function(msg) {};
  - 接收到数据时触发，回调函数中自动传入参数为接收数据msg
- send方法
  - ws.send(data)
  - 发送数据

```javascript
const input = document.querySelector('input');
const btn = document.querySelector('button');
const div = document.querySelector('div');
// 创建websocket服务，需要一个服务地址
const serverStr = 'ws://localhost:3000';
const ws = new WebSocket(serverStr);
// open事件
ws.addEventListener('open', () => {
    const p = document.createElement('p');
    p.textContent = `WebSocket conneted to ${serverStr}`;
    div.appendChild(p);
});
// send方法
btn.addEventListener('click', () => {
    const value = input.value;
    ws.send(value);
});
// message事件
ws.addEventListener('message', (msg) => {
    const p = document.createElement('p');
    p.textContent = msg.data;
    div.appendChild(p);
});
```



## websocket在服务端支持

​	websocketAPI有很多第三方模块进行封装，直接使用nodejs-websocket可以完成简单开发。

- 创建服务器
  - const server = ws.createServer((connect) => {});
  - 创建一个server实例，每次有连接到实例都会执行回调函数，回调函数中的参数connet是创建的连接
- 在创建的connect上绑定回调函数
  - connect的text事件
    - connect.on('text', (data) => {})
    - 接收到数据时触发，回调函数中参数为接收到数据
  - connect的send方法
    - connect.send(data)
    - 发送数据
- 服务器连接到指定端口
  - server.listen(PORT, () => {})

```javascript
const ws = require('nodejs-websocket');

const PORT = 3000;

// 创建ws服务器，每次有ws连接就会执行回调函数创建一个connect
const server = ws.createServer((connect) => {
   console.log('Connection comming in');
   connect.on('text', (data) => {
      connect.send(`response ${data} from 127.0.7`);
   });
});

server.listen(PORT, () => {
   console.log(`WS server running on port ${PORT}`);
});
```

