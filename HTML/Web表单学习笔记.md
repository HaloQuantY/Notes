## Web表单学习笔记

​	表单是用户和web进行交互的主要内容之一，可以让用户将数据发送到web站点，可以发送到服务器或被浏览器自身拦截使用。

##### 表单基本组成元素

​	表单的基本组成是form元素和各个表单小部件：

- form元素
  - form元素是表单的基础，正式定义了一个表单
  - 是一个容器元素
  - 通常需要设置action和method属性
  - action：处理表单数据的URL
  - method：发送数据的HTTP方法（get/post）
  - form元素外使用表单部件可以运作，但不会和任何form元素相关，自身的行为需要使用JavaScript进行手动定义
- label元素
  - **for属性值对应相应的表单元素id值**
- input元素
  - type属性定义了input元素的行为方式，从根本上改变元素
  - value属性改变input元素默认值
  - 内联元素
- textarea元素
  - 使用元素的内容来改变textarea默认值
  - 内联块元素
- button元素
  - **type属性：submit，reset，button**
  - submit：发送表单数据到form元素action所指URL
  - reset：将所有表单小部件重置为默认值（通常不用）
  - button：不会进行任何行为
- fieldset和legend元素
  - 用于创建一组功能相同的表单部件组（例如把相同name的radio部件放入fieldset中）
  - legend元素则是部件组的标题或功能说明

##### 表单基本样式

- label
  - 设置等宽右对齐display：inline-block，text-align：right
  - 设置和元素上对齐，相应元素设置vertical-align：top

##### 发送数据到服务器

- name属性
  - 需要使用该属性作为键名，因此所有需要上传数据的表单部件都需要定义该元素

##### 基础表单例

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Document</title>
   <style>
      * {
         margin: 0;
         padding: 0;
      }
      form {
         margin: 50px auto;
         width: 450px;
         border: 1px solid black;
         border-radius: 10px;
      }
      div {
         margin: 10px 0;
      }
      label {
         display: inline-block;
         width: 90px;
         text-align: right;
      }
      input, textarea {
         width: 300px;
      }
      textarea {
         vertical-align: top;
         height: 5em;
      }
      #btnWrapper {
         padding-left: 95px;
      }
   </style>
</head>
<body>
   <form action="post" action="02.anchor_element.html">
      <div>
         <label for="nameInput">Name:</label>
         <input type="text" id="nameInput" name="user_name">
      </div>
      <div>
         <label for="emailInp">E-mail:</label>
         <input type="email" id="emailInp" name="user_email">
      </div>
      <div>
         <label for="">Message:</label>
         <textarea name="user_message" id="messageInp" cols="30" rows="10"></textarea>
      </div>
      <div id="btnWrapper">
         <button type="submit" id="submitBtn">Send your message</button>
      </div>
   </form>
</body>
</html>
```



### 表单构造元素

​	form，fieldset，legend，label元素是用于构建正确表单结构的元素。

#### label元素

​	label元素是为表单部件元素定义标签的正式方法，同时也能保持良好语义化。

- for属性和内联书写表单部件
  - 可以在label元素的内部直接书写表单部件元素并关联部件元素
  - 但for属性关联是最佳做法
- label元素可以点击
  - label元素点击会激活关联部件元素，对于单选和复选框更加有用

#### 表单通用结构

```html
<section>
    <h2>Contact information</h2>
    <fieldset>
      <legend>Title</legend>
      <ul>
          <li>
            <label for="title_1">
              <input type="radio" id="title_1" name="title" value="K" >
              King
            </label>
          </li>
          <li>
            <label for="title_2">
              <input type="radio" id="title_2" name="title" value="Q">
              Queen
            </label>
          </li>
          <li>
            <label for="title_3">
              <input type="radio" id="title_3" name="title" value="J">
              Joker
            </label>
          </li>
      </ul>
    </fieldset>
    <p>
      <label for="name">
        <span>Name: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="text" id="name" name="username">
    </p>
    <p>
      <label for="mail">
        <span>E-mail: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="email" id="mail" name="usermail">
    </p>
    <p>
      <label for="pwd">
        <span>Password: </span>
        <strong><abbr title="required">*</abbr></strong>
      </label>
      <input type="password" id="pwd" name="password">
    </p>
</section>
```

- 所有部件使用fieldset和legend元素包围
- 选择框使用ul/li进行包围
- 文本框使用p进行包围



### 表单控制

​	表单各部件可以通过元素的属性来控制自身的行为，其中包含通用的属性和各元素特有的属性。

##### 通用属性

- autofocus
  - 布尔值
- disabled
  - 布尔值
- name
  - 元素名称，和表单数据一起提交构成键名
- value
  - 元素初始值

##### 元素特有属性

- input元素文本框
  - 单行文本框
  - email
  - password
  - search
  - tel
  - url
  - 使用list属性值绑定对应id的datalist元素，在datalist元素内部用option来给定候选值
- 下拉内容
  - 下拉单选框
    - select元素创建选择框，包含一个或多个option元素为子元素
    - optgroup为option分组，label属性值为分组名
  - 下拉多选框
    - select创建的选择框默认为单选，需要使用multiple转换为复选框
    - 使用multiple属性后的复选框，选项由列表展示，使用ctrl+单击完成多选操作
- 选择框
  - 使用type为checkbox的input元素来创建复选框
  - 使用type为radio的input元素创建单选框
  - 使用ul/li元素包围label和input元素，使用name属性来表示一组选择框
- 按钮
  - button或input元素来创建按钮
  - submit，reset和button三种按钮，使用type属性值来创建
  - button和input行为相同
  - 但二者创建的按钮有不同之处
    - button创建按钮的内容可以直接在元素内部书写，且内部可以是HTML标签并相应的样式化
    - input创建按钮的内容只能用value指定，不能使用内联元素
    - button也可以设置value值，并且能够和元素内容不同



### 表单美化

​	使用CSS对表单构建进行美化比较复杂，因为不同浏览器厂商不愿对表单控件样式进行调整，对表单元素的CSS支持进展缓慢并且各浏览器进度不同，现在虽然可以直接使用CSS对表单进行美化，但可用性并不高。许多产品也使用自定义表单控件来避免这个问题。

##### 浏览器对各种表单控件的CSS支持程度

​	文本框和按钮可以使用任意CSS样式，选择框元素需要借助伪类和CSS3语法，其余元素需要通过自定义完成美化。

- 支持程度高，可以直接使用CSS进行美化
  - form
  - fieldset
  - label
  - 各种文本控件（input，textarea）
  - 按钮
- 支持程度中，需要借用CSS3和伪类
  - legend，checkbox，radio（CSS3）
  - placeholder（伪类）
- 支持程度低，不能应用CSS样式
  - 其余所有控件

#### 盒子模型

​	所有文本字段支持CSS盒模型的相关属性，但每个部件都有自己border，padding和margin的规则，所以需要使用box-sizing属性

```css
input, textarea, select, button {
  width : 150px;
  margin: 0;

  -webkit-box-sizing: border-box; /* For legacy WebKit based browsers */
     -moz-box-sizing: border-box; /* For legacy (Firefox <29) Gecko based browsers */
          box-sizing: border-box;
}
```

#### 定位

​	HTML表单部件通常都可以使用定位，只有两点比较特殊

- legend元素
  - legend元素使用其他CSS样式都比较容易，但绝对位置总在fieldset边框顶部
  - 通常将legend元素设置为隐藏，仅保持可访问性
  - width：1px；height：1px；overflow：hidden；
- textarea
  - 多行文本框默认是inline-block元素，并且和文本底线对齐
  - 通常会转换为block元素或应用vertical-align：top；来进行对齐



#### 浏览器表单扩展

​	CSS不同版本加入了对表单部件的伪类操作，但很多伪类有对浏览器和复杂表单部件如日期部件有兼容性问题。

​	浏览器厂商也各自对表单部件的美化进行了扩展，这些扩展功能并非标准功能。

- 基于webkit浏览器（Chrome，Safari）的专有属性

  - -webkit-appearance
  - -moz-appearance
  - 这两种控制表单外观的扩展属于非标准的，应减少使用
    - 但none属性值可以控制部件样式
- 更多库和polyfills
  - 对表单部件的完全控制，唯有通过Javascript实现
  - 使用库可以简化操作
    - Bootstrap
    - jQuery UI
    - Uni-form



  

  

  

  

  