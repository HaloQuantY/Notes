## Vue深入了解组件

### 组件注册

#### 组件名

​	在注册组件时始终需要一个名字。在DOM中使用组件时使用W3C规范定义组件名，字母全小写名且包含一个连字符。

- 组件名大小写
  - 组件名有两种方式
  - kebab-case：全小写用连字符连接
  - PascalCase：首字母大写，这种方式DOM中不支持

#### 全局注册

​	使用Vue.component注册组件是全局注册，可以在注册之后在任何新创建Vue实例模板中使用。

#### 局部注册

​	使用普通JS对象定义组件，这个JS对象就是组件的选项对象。

```javascript
var ComponentA = {
    template: `
		<h1>This is ComponentA.</h1>
	`
};
```

​	然后在根实例中，可以使用components属性定义需要包含的组件：

```javascript
const div1 = new Vue({
    el: "#div1",
    components: {
        "component-a": ComponentA
    }
});
```

​	在HTML模板中，使用components中定义属性名来进行组件复用：

```html
<div id="div1">
    <component-a></component-a>
</div>
```

​	局部注册的组件实际上就是选项对象，可以通过import或require来进行引入。

#### 模块系统



