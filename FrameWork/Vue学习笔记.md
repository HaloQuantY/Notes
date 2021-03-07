## Vue学习笔记

​	Vue是一套用于构建用户界面的渐进式框架，核心库仅关注图层，易于上手且便于和第三方库进行整合。

### 概念

- MVVM模型
- SPA
- Vue的作用

##### MVVM模型

​	MVVM（Model-View-Viewmodel）是一种软件架构模式，有助于将图形用户界面和业务逻辑或后端逻辑（数据模型）开发分离开。

- 模型Model
  - 指代真实状态内容的领域模型，或是代表内容的数据访问层（从服务器拿到的JSON数据）
- 视图View
  - 用户在屏幕上看到的外观布局（UI）
- 视图模型Viewmodel
  - 视图适配器，用于暴露属性和将View元素显示内容进行对应
- 绑定器binder
  - 用于实现View和Viewmodel之间的数据双向绑定

​	MVVM来源于MVC，是MVC的改进版。

![MVVM](https://upload-images.jianshu.io/upload_images/2667543-2f70f417cd78ade5.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

##### SPA

​	单页面应用（Single Page Application）是一种网络应用程序或网站的模型，它通过动态重写当前页面来与用户交互，而不是从服务器重新加载整个页面。

SPA使用技术

- JavaScript框架
  - Vue
  - Angular
  - React
- Ajax
- Websocket
- 服务器发送事件

SPA面临问题

- SEO
- 浏览器历史记录
- 初次加载速度

### Vue的基本使用

​	Vue处理的事情主要是模板渲染，事件绑定，处理用户交互。

- 创建Vue实例
- 模板渲染
  - el属性：挂载元素
  - template属性：使用属性值替换挂载元素
  - data属性：绑定数据
    - 只有初始化时传入的data对象才是响应式的
    - 传入一个对象，将直接代理该对象而不会进行深拷贝
    - 一般会使用data函数来返回对象，防止data对象的泄露
  - v-bind:<属性名> = ‘属性值’
    - 用来绑定属性
- 事件绑定
  - v-on:<事件名> = '回调函数名'
  - Vue实例中methods属性，值为一个对象，对象的属性对应v-on的回调函数
- 用户交互
  - v-model用来实现数据的双向绑定
  - v-model不支持普通元素，一般是input元素

#### 创建Vue实例

```javascript
const vm = new Vue({
    // 选项对象属性
});
```

##### 数据和方法

- Vue实例创建时，会将data对象中所有property加入到Vue响应式系统中
- Object.freeze()方法可以冻结data对象，使data对象停止响应更新数据

##### 生命周期和钩子

​	Vue实例的创建过程中有一系列生命周期，在不同节点有对应名称并挂载钩子函数。生命周期包括:

- beforeCreate
- created
  - 初始化完成，创建Vue实例
  - 之后在内存中执行模板编译
- beforeMount
- mounted
  - 替换挂载元素
  - 常用钩子函数，可以在其中书写定时器等操作
- beforeUpdate
- updated
  - 数据发生变化，重新渲染页面
- beforeDestroy
  - 常用钩子函数，用于删除前进行处理（例如清除定时器）
- destroyed
  - 删除实例，手动使用$destroy删除

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="LifeCircle" style="zoom: 33%;" />

#### 模板语法

​	Vue可以声明式将DOM绑定至底层Vue实例中，完成模板渲染工作。

- 插值
  - 文本
    - 双大括号语法可以取用data数据中的property值
    - v-once一次性插值：视图中数据不会改变
  - 原始HTML
    - v-html指令，可以将字符串作为html原始代码解析
  - Attribute
    - v-bind指令
- 指令
  - 所有带v-前缀的特殊attribute就是指令
- 缩写
  - v-bind缩写：`<a :href='url'></a>`
  - v-on缩写：`<a @click='callback'></a>`

#### 计算属性

​	模板中可以写JS表达式，但是为了视图的简洁和直观，所有复杂的逻辑都应该用计算属性来表达。

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

- 计算属性缓存
  - 计算属性拥有缓存属性，只要参与计算的属性没有改变就不会重新执行getter函数
  - 只要参与计算属性发生改变就会自动触发计算属性

#### Class&Style绑定

​	class和style和其他属性没有本质区别，只是为了书写方便Vue添加了对象和数组语法。

```html
<div v-bind:class="{ active: isActive }"></div>
```

### 模板渲染

​	早期web服务依靠服务器端进行渲染，服务器从数据库获得数据后利用后端模板引擎生成HTML文件发送到浏览器端。

​	而前端渲染则是在浏览器端利用JS将服务器传来的数据和HTML模板进行组合

#### 条件渲染v-if

​	v-if指令用于条件性渲染一块内容，这块内容旨在指令表达式返回真时被渲染。Vue可以在浏览器端进行渲染，使得前后端分离将计算量转移到浏览器端。

- v-if
  - 根据数据值来判断是否显示该DOM元素以及包含的子孙
  - v-if为false的元素会直接不被浏览器渲染，HTML中不包含该元素
- v-show
  - 同样根据数值判断是否显示该DOM元素
  - v-show为false的元素虽然不被显示，但只是display属性值为false，HTML的DOM树中仍然有该元素
  - v-show不支持template元素和v-else
- v-else
- template元素
  - 用来包含多个需要使用v-if的DOM块
- key
  - 管理复用元素，相同key值的v-if/v-else代码块切换时会进行复用

##### 使用key管理可复用元素

​	Vue在渲染元素时，会判断是否为已经用过的元素并进行复用，而非从头开始渲染，

例如

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
// 这两个代码块在切换时，不会全部重新渲染，而是只替换placeholder属性值而已
```

​	但如果要保证每次都重渲染元素，需要使用key来标记相互独立的元素，让他们成为不可复用元素。



#### 列表渲染v-for

​	v-for指令可以遍历一个数组或对象（JSON），然后迭代渲染指定元素。

```html
<ul id="vForList">
    <li v-for="item in items">{{ item.name }}</li>
</ul>
```

```javascript
const vm0 = new Vue({
    el: '#vForList',
    data() {
        return {
            items: [
                {name: "linzhiying"},
                {name: "zhangsanfeng"},
                {name: "yangkang"}
            ]
        };
    }
});
```

- 数组
  - v-for="(item, index) in items"
  - 遍历数组，item则表示数组项
- 对象
  - v-for="(value, name, index) in items"
  - 遍历对象，value为属性值，name为属性名，index则是目录
- key值
  - 和v-if相同，需要key值作为识别根据，但由于不能一一写出渲染元素，需要使用v-bind进行绑定属性，且属性值一般为item.id
- 视图更新
  - 数组的原生方法可以触发视图更新，但直接修改将不会触发



#### 表单输入绑定

​	是数据绑定的一部分，用来对表单元素<input>，<textarea>，<select>，<radio>元素上进行双向数据绑定。v-model指令本质上仍然是语法糖，本质上是监听用户输入并更新数据。

​	双向数据绑定会忽略表单自身的value属性等，只由Vue实例中的数据来渲染视图。

##### 基础用法

- 文本
  - input元素绑定，使用v-model指定data对象中属性即可完成双向数据绑定
- 多行文本
  - textarea元素绑定，同样直接使用v-model指定data对象属性
  - 在textarea元素中直接使用双括号语法不会进行数据绑定，只能通过v-model语法绑定数据
- 复选框
  - checkbox元素绑定，使用v-model语法绑定到data对象上的一个数组
  - 用来将选取到的value值，响应式地绑定到一个数组中，方便获取数据
- 单选框
  - radio元素绑定，和复选框几乎相同。需要用name属性值来识别一组radio
  - v-model绑定的是一个data对象上的属性值
- 选择框
  - select和option元素，是对radio元素的另一种表达方式，可以将选项隐藏起来
  - 同样绑定到data对象上的一个属性值



### 组件基础

​	组件是可以复用的Vue实例，web网页如果有部分在多个场景中使用可以写成一个组件进行复用。

#### 创建组件和组件复用

- 使用Vue.component（'componentName', {  }）全局创建组件
  - 参数和创建Vue实例相同，只是el属性换为template属性以便复用
- 组件复用
  - 直接在html中使用组件名作为标签名即可使用

```html
<div id="component">
    <componentName></componentName>
</div>
```

```javascript
Vue.component('componentName', {
   template: `<h1>{{ text }}</h1>` ,
   data() {
       return {
           text: 'This is template'
       }
   }
});

const vm0 = new Vue({
   el: '#component',
   data() {
       return {
           
       }
   }
});
```

- 组件中data必须是一个函数，返回一个包含数据的对象
  - 目的是让组件实例都可以有自己的data对象
- 全局和局部注册组件
  - 组件需要注册后在根实例中才能使用
- 模板中只能有一个根元素

#### 向子组件中传递数据

​	创建好的Vue组件自身保存的数据只有data属性等，所有数据的处理也是由自身的data和methods，computed等进行，如果要从外部获取数据需要从父组件获得。

##### props

​	props是Vue组件的一个属性，值为一个数组，数组内是自定义属性的属性名，当组件复用时，在html标签中可以传入对应属性，数据通过自定义属性值的形式传入组件。

```javascript
Vue.component('componentName', {
   template: '<h1>{{ title }}</h1>' ,
   props: ['title']
});
```

```html
<componentName title="test title"></componentName>
```

- 在父组件中使用的情况，可以使用v-bind来进行数据绑定
  - 每级组件都有自己的定义域，prop重复时一般不会发生影响
- prop和data对象的取用一样，会成为组件实例的一个属性
- 可以接收一个对象

##### 父子组件的事件通信

​	子组件中可以使用`v-on="$emit('event-name')"`来绑定一个自定义事件，这个事件的触发只会向父组件发送该事件名，当父组件作用域中设置v-on="event-name"时，就会监听该子组件自定义事件。

```html
<div id="mainComp">
    <div :style="styleObject">
        <vue-post 
                  v-for="(post, index) in posts"
                  :key="index"
                  :post="post"
                  @enlarge-text="enlargeText"
        ></vue-post>
    </div>
</div>
```

```javascript
Vue.component('vue-post', {
    template: `
<div class="vue-post-class">
<h1>{{ post.title }}</h1>
<button @click="$emit('enlarge-text', 0.1)">Enlarge text</button>
<p>{{ post.text }}</p>
</div>
`,
    props: ['post'],
});

const vm0 = new Vue({
    el: '#mainComp',
    data() {
        return {
            posts: [
                {
                    title: 'This is post0',
                    text: 'This is text0'
                },
                {
                    title: 'This is post1',
                    text: 'This is text1'
                },
                {
                    title: 'This is post2',
                    text: 'This is text2'
                },
            ],
            styleObject: {
                fontSize: "1em"
            }
        }
    },
    methods: {
        enlargeText(enlarge) {
            this.styleObject.fontSize = parseFloat(this.styleObject.fontSize) + enlarge + 'em';
        }
    }
});
```

- 行内的自定义事件不能使用驼峰命名法
- 子组件的事件最终执行函数在父组件中执行，子组件中仅仅是使用$emit来定义子事件的触发条件并发送该事件名到父组件
- $emit的第二个参数可以是要发送的data
  - 父组件的执行函数以第一个参数进行接收

###### 特殊：子组件上使用v-model

- v-model="inputText"实际上等价于两条指令
  - v-bind:value="inputText"
    - 将该元素value属性设置为inputText
  - v-on:input="inputText = $event.target.value"
    - 该元素input事件触发，将inputText更新为事件的value值
- 组件中v-model=“inputText”则会变成
  - v-bind：value=“inputText”
    - 仍然是设置组件的value属性
  - v-on：input=“inputText = $event”
- 所以在组件模板中需要
  - 设置props:["value"]接收父组件的value值
  - v-on:input="$emit('input', $event.target.value)"
    - 组件中input触发，发送input事件和更新的值

```html
<div id="mainComp">
    <custom-input
                  v-model="inputText"
                  ></custom-input>
    <p>{{ inputText }}</p>
</div>
```

````javascript
Vue.component('custom-input', {
    template: `
<input
// 由于value属性和input事件名固定的，组件中使用v-model方法也固定
:value="value"
@input="$emit('input', $event.target.value)"
>
`,
    props: ['value']
});

const vm0 = new Vue({
    el: '#mainComp',
    data() {
        return {
            inputText: ''
        }
    }
});
````



### Vue-Router

​	Vue-Router是Vue的官方插件，用来对页面进行路由编辑，对Vue组件和路径进行映射关系，是用于展示组件的一个插件。

#### 基本概念

- 路由route
  - 是路径和资源的对应关系，通常会使用键值对来表示，而多个键值对会构成路由表，即routes
- 路由器router
  - 用于实现路由的具体媒介，在VueRouter中用VueRouter实例来完成

#### 基本使用

- 引入/注册组件
  - 在网页和node操作不同，网页中只要使用带template属性对象就可以完成注册，node中需要使用import进行组件引入
- 编写路由表routes
  - routes由所有需要的路由关系组成，每个route有两个属性path和component，分别对应路径和资源（组件
- 实例化路由器router
  - 使用VueRouter构造函数实例一个路由器对象，构造函数中需要一个routes数组作为参数
- 创建根实例配置router
  - 在根实例中router属性写入路由器实例即可
- 创建视图
  - router-link标签创建链接，默认将会渲染为a标签，且to属性会指向路由中对应path属性
  - router-view标签创建视图，会根据path寻找对应组件并进行渲染

```html
<body>
    <div id="vm0">
        <router-link to="/test"></router-link>
        <router-view></router-view>
    </div>
</body>
```

```javascript
const testComponent = {template: '<div>This is test component</div>'};
routes = [{path: '/test', component: testComponent}];
const router = VueRouter(routes);

const vm0 = new Vue({
   el: '#vm0',
   router
});
```

#### 命名视图/命名路由

​	路由表中可以对组件和路由本身进行命名

```javascript
const routes = [
    {
        path: '/test',
        components: {
            default: test,
            common: testCommon
        },
        name: 'testCpn'
    }
];
```

​	对应到模板中可以使用相应语法

```html
<div id="vm0">
    <router-link to="{name: 'testCpn'}"></router-link>
    <router-view></router-view>
    <router-view name="common"></router-view>
</div>
```

#### 动态路由匹配

​	用于将多个路由映射到同一个组件，并且可以传递数据。在VueRouter中可以使用动态路径参数实现，也就是在路径中写入匹配模式，用其中的参数进行多个路由匹配并将参数传递到对应组件中。

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

- 传递数据
  - 使用路径中参数进行数据传递
  - 路径中的参数会存储到组件的**this.$route.params**属性
- 响应路由参数变化
  - 路由参数变化时，组件会复用而不是销毁再创建，因此组件的钩子不会响应
  - 要相应路由参数变化可以使用watch来检测变化或使用导航守卫

#### 嵌套路由



