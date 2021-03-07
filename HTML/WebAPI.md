## WebAPI

​	API（Application Programing Interface）基于编程语言抽象了复杂代码，并提供了简单接口规则直接使用。

### WebAPI分类

##### 客户端Javascript中API

​	客户端Javascript通常分为两类

- 浏览器API
  - 内置于浏览器中，从浏览器和计算机周边提取数据
  - 如Geolocation API
- 第三方API
  - 默认不内置于浏览器中，需要从某个地方获取代码和信息
  - 如Twitter API

##### 常见浏览器API

- 操作文档A
  - DOM
- 获取服务器数据
  - XMLHttpRequest，Fetch API，即Ajax
- 绘制和操作图形
  - Canvas，WebGL
- 音视频
  - HTMLMediaElement，Web Audio API
- 设备API
  - 位置API，系统通知API，震动API
- 客户端存储
  - Web Storage API， IndexedDB API

### 操作文档

#### web浏览器组成部分

- window
  - 窗口，是浏览器的标签，在Javascript里可以中Window对象表示
- navigator
  - 浏览器存在于web上的状态和标识（用户代理），JavaScript中用Navigator对象表示
- document
  - 载入窗口的实际页面，浏览器中用DOM表示，JavaScript中用document对象表示

#### 文档对象模型

​	是一个由浏览器生成的树结构，浏览器通过DOM可以将样式和其他信息应用于正确元素。

#### 基本DOM操作

- 选取元素节点
  - Document.querySelector() ：推荐使用
  - Document.getElementById()，Document.getElementsByTagName()：较老浏览器中使用
- 创建并放置新节点
  - document.createElement()
  - document.createTextNode()
  - Node.appendChild()
- 移动和删除元素
  - appendChild()
  - Node.parentNode.removeChild()：只能通过父元素删除子节点
- 操作样式
  - HTMLElement.style获取属性，直接更改属性值
  - JavaScript中的css属性是驼峰命名（如backgroundColor）
  - JavaScript创建静态内容不利于SEO，少用



### 获取服务器数据

​	从服务端获取个别数据更新部分网页而不用加载整个页面，通常使用XMLHttpRequest和Fetch API。

- Ajax
  - Asynchronous Javascript and XML
  - 最早使用XMLHttpRequest请求XML数据
  - 现在常用XMLHttpRequest/Fetch API请求JSON，但Ajax名字仍然沿用
- HTTP协议
  - 规范了请求和响应报文
  - 请求报文四部分：请求行，头，空行，体
  - 行：协议信息（类型/版本），URL
  - 头：Host，Cookie，Content-type，User-Agent
  - 空行
  - 体：具体内容，可以为空（GET）
  - 响应报文四部分：请求行，头，空行，体
  - 行：协议信息，状态码（200，OK）
  - 头：Content-type，Content-length，Content-encoding
  - 空行
  - 体：响应内容

#### 基本Ajax请求

##### XMLHttpRequest

- 创建对象
- 初始化对象，设置请求方法和url（open）
- 设置返回类型 XHR.responseType，默认返回文本类型
- 事件绑定（onreadystatechange）
  - readystate是xhr对象中属性表示状态，分别有0，1，2，3，4
  - 判断XHR.readyState === 4
  - 判断HTTP响应状态码XHR.status >= 200 && XHR.status < 300
  - status状态码，statusText状态字符串，XHR.response响应体,XHR.getAllResponseHeaders()获得所有响应头
- 发送报文（send）

##### Fetch

​	Fetch API基本上是XHR的现代替代品，令异步HTTP请求更易实现。

```javascript
fetch('http://localhost:3000/fetch-server/verse1', {
            method: 'POST',
            body: 'username=admin&password=admin'
         })
         .then((res) => {
            return res.text();
         })
         .then((text) => {
            result.textContent = text;
         });
```

​	fetch方法接收两个参数

- url
  - url参数也从此处传入
- init对象
  - method协议类型
  - headers
  - body

​    fetch方法返回一个对象response，对于response根据返回数据类型不同可以继续使用不同方法获得数据（如text（），json（））

##### Axios

​	Axios是Ajax的一个常用库，仍然以XHR为核心，但简化了很多操作。

```javascript
axios.post('http://localhost:3000/axios-server/verse1')
         .then((res) => {
            result.textContent = res.data;
         });
```

​	Axios有和http方法对应的函数，可以发送相应请求，相比fetch方法更加简洁



### 同源策略

​	同源是指发送请求的页面和目标资源协议，域名和端口号相同

- 协议：http和https

- 域名

- 端口号

  只要请求资源非同源就是跨域，项目中可能使用多个服务器进行处理，需要进行跨域处理。

#### 跨域

- JSONP
  - 非官方跨域解决方案，仅支持GET请求
- CORS
  - 不同浏览器实现不同，IE使用XDR

##### JSONP

​	JSONP是JSON with padding，JSONP和JSON差不多，只是被包含在函数调用中的JSON。

JSONP原理

- 借助HTML中一些可以实现跨域的标签，如img，link，iframe，script
  - JSONP利用script标签实现跨域

##### CORS

​	是官方的跨域解决方案，不需要再客户端做任何操作，完全在服务器进行处理。

​	核心是服务端对响应头进行设置，为响应报文的响应头中加入对应的CORS识别字段，可以达到跨域共享资源效果。