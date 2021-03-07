## MongoDB学习笔记

### 概念

- 数据库
- MongoDB

#### 数据库

​	（维基）数据库，又称为数据管理系统，简而言之可视为[电子化](https://zh.wikipedia.org/w/index.php?title=電子化&action=edit&redlink=1)的[文件柜](https://zh.wikipedia.org/wiki/档案柜)——存储电子[文件](https://zh.wikipedia.org/wiki/檔案)的处所，用户可以对[文件](https://zh.wikipedia.org/wiki/檔案)中的资料运行新增、截取、更新、删除等操作。

所谓“数据库”系以**一定方式**储存在一起、能予多个用户[共享](https://zh.wikipedia.org/wiki/共享)、具有尽可能小的[冗余度](https://zh.wikipedia.org/wiki/数据冗余)、与应用程序彼此独立的数据[集合](https://zh.wikipedia.org/wiki/集合)。一个数据库由多个表空间（[Tablespace](https://zh.wikipedia.org/wiki/Tablespace)）构成。



- 关系型数据库
- 非关系型数据库

##### 关系型数据库

​	创建在关系模型基础上的数据库，基于表。

​	包括MySQL，Oracle等，是具标准化的数据库。其中SQL是StructuredQueryLanguage，是一种操作数据库的语言。

##### 非关系型数据库

​	NoSQL数据库，范围囊括所有不是关系型数据库的类型。MongoDB是非关系型数据库中的文档型数据库，基于文档。

#### MongoDB

​	是一种非关系型文档数据库，对比关系型数据库更加灵活利于快速开发，存储各种二进制JSON文档（BSON）。

##### MongoDB组成和安装

​	MongoDB的三个概念是数据库，集合和文档，组成数据库的最小单元是文档，以JSON形式进行书写，大部分操作都是以文档为对象。

- 组成
  - 包括MongoDB服务器和MongoDB客户端
  - MongoDB本体自带服务器，以mongod启动服务
  - MongoDB客户端有多种，默认自带MongoShell
- 安装
  - 在安装目录下用控制台启动Mongod配置服务器
  - 主要参数是dbpath和logpath，可选参数是port（默认27017）
  - 启动服务之后使用对应客户端进行连接即可（默认是mongo指令启动MongoShell）

##### 基础指令

- 查找
  - show dbs：列出所有数据库
  - show collections：列出所有集合
  - use <dbName>:  选中数据库
  - db.<collectionName>.find(): 在集合中查找文档，参数是一个filter对象，会过滤不符合filter对象属性的文档
- 创建
  - use <dbName>： 隐式创建数据库，如果该数据库存放了数据会自动创建
  - db.createCollection('collectionName'): 创建集合
  - db.<collectionName>.insert(): 插入文档，此命令中如果collectionName为一个新集合也会自动创建集合
- 删除
  - db.<collectionName>.drop(): 删除对应集合
  - db.dropDatabase(): 删除选中的数据库



### MongoDB CRUD操作

#### Create操作

- ObjectId属性：_id，所有文档必须存在且必须唯一的属性，用于对应文档
  - ObjectId（）实例：_id属性默认值，用于保证文档唯一性

- db.collection.insertOne(<document>)
  - 用于插入单个文档，参数为文档内容，可以是JSON也可以是普通对象
- db.collection.insertMany(<document1>,  <document2>)

#### Read操作

- db.collection.find(<filter>)
  - 用于查找集合中文档，如果没有filter对象则会返回所有文档
  - filter对象用于指定过滤条件
  - count（）返回数量
  - pretty（）格式化显示文档

#### Update操作

- db.collection.updateOne(<filter>,  <update>)
  - 更新第一个匹配文档
  - update对象中，属性名是操作名，值为操作对应对象
  - $set是修改操作
  - $unset是删除指定属性操作
  - $push向数组中添加元素
- db.collection.updateMany(<filter>,  <update>)
- db.collection.replaceOne(<filter>,  <replacement>)

#### Delete操作

- db.collection.deleteOne(<filter>)
- db.collection.deleteMany(<filter>)



### Mongoose

​	Mongoose是连接数据库的一种工具，相比MongoDB自带的模块更加易于操作。Mongoose是一个对象文档模型，用于将数据库中的文档转换为JS对象，可以直接用面向对象的编程方法对数据库进行操作。

​	Mongoose的三个概念

- schema
  - 模式对象，是一种文档的模板，控制插入文档的属性名和数据类型等，对要传入文档的数据进行验证和类型转换
- model
  - 模型，对应MongoDB数据库中的集合
- document
  - 文档，对应集合中的具体文档

#### Mongoose基础使用

```javascript
// 引入Mongoose模块
const mongoose = require('mongoose');
// 连接数据库
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});
// 创建CatModel，使用schema进行约束
const Cat = mongoose.model('Cat', { name: String });
// 创建新的Cat文档
const kitty = new Cat({ name: 'Zildjian' });
// 将新创建的文档写入数据库
kitty.save().then((err, data) => console.log(data));
// 查询文档
Cat.find({name: 'zildjian'}, (err, data) => {
   console.log(data) 
});
```

#### Schema

​	schema是mongoose中用来约束文档的模板，要定义schema需要用到mongoose模块中的Schema构造函数：

```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;
const mySchema = new Schema({
   name: String,
   age: Number,
   birth: Date,
   gender: {
       type: String,
       default: female
   },
   hidden: Boolean
});
```

#### Model

​	Mongoose中的CRUD操作通常通过Model对象的方法实现。Model方法中查询到的文档对象是Model的实例。

- Model.create（doc, callback(err, doc）
- Model.find(<filter>, <projection>, callback(err, docs))
- Model.findOne(<filter>, callback(err, doc))
- Model.findById('idString')
- Model.update(<filter>, doc, options, callback)
  - doc是修改后对象，同样使用操作符声明要进行的操作
- Model.updateMany()
- Model.updateOne()
- Model.replaceOne()
- Model.deleteOne(<filter>, callback)
- Model.deleteMany(<filter>, callback)
- Model.count(<filter>, callback)

#### Document

​	Document是Model的实例，通过Model查询到结果都是Document。

- new Model({  })
  - 创建一个新文档对象
- document.save(callback)
  - 保存到数据库
- document.update(doc, callback)
  - doc同样是修改后对象，可以使用操作符
  - 也可以直接修改doc属性然后调用save方法
- document.delete(callback)
- document.toJSON()
- document.toObject()
  - 转换为普通JS对象，不能再使用doc对象属性和方法