## Ajax学习笔记

### 简介

- Ajax用来和服务器异步交换数据，能在页面不刷新情况下获取数据。
- HTTP协议
- 四种Ajax操作：原生，jQuery，fetch，axios

​	

### XML

​	XML是用于传输数据的标记语言，和HTML结构类似，但所有标签都是自定义标签。Ajax最早传输数据时使用XML格式（现在为JSON）

```
{
	"name": "Gokkon",
	"age": "18",
	"gender": "male"
}

// XML
<student>
	<name>Gokkon</name>
	<age>18</age>
	<gender>male</gender>
</student>
```



### HTTP

​	（HyperText Transport Protocol）超文本传输协议，主流的应用层传输协议。

#### 请求报文

包含四部分

- 行

  请求类型（GET/POST），URL ，协议版本（HTTP/1.1）

- 头

  Host，Cookie，Content-type，User-Agent等

- 空行

- 体

  GET请求体为空，POST则体可以不为空

#### 响应文本格式

- 行

  协议版本，响应状态码（200），响应字符串（OK）

- 头

  格式和请求报文相同

- 空行

- 体

  主要返回结果

  ```
  <html>
  	<head>
  	</head>
  	<body>
  		<p></p>
  	</body>
  </html>
  ```


