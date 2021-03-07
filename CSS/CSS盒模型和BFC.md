## CSS盒模型和BFC

### 盒模型

​	CSS中所有元素在布局和元素排列时都遵循盒模型，是复杂布局的基础知识。

#### 块盒子和内联盒子

​	CSS中块盒子和内联盒子是广泛应用的两种盒子，这两种盒子在页面流和元素间关系方面有不同行为：

###### 块盒子（block）

- 内联方向上扩展占用父容器在该方向上所有可用空间
- 每个盒子都会换行
- 可以设置height/width属性
- padding，margin，border会把其他元素从当前元素推开

###### 内联盒子（inline）

- 盒子不会换行
- width/height属性不起作用
- 垂直方向padding，margin，border会应用但不会推开其他inline盒子
- 水平方向padding，margin，border表现正常

##### 内部和外部显示类型

- 所谓块盒子和内联盒子，是根据盒子的外部显示类型区分的，使用display：block/inline可以切换外部显示类型
- 外部显示类型决定盒子本身的布局规则，内部显示类型则规定了盒子内部元素的显示规则
- 内部显示类型也使用display属性来定义
  - display：flex定义了一个元素的内部显示类型为flex，外部显示类型为block
  - display：grid同样定义元素的内部显示类型为grid

#### CSS盒模型

​	块盒子可以应用完整CSS盒模型，而内联盒子则应用CSS盒模型的一部分

##### 盒模型组成

- content box
- padding box
- border box
- margin box

##### 标准盒模型

​	元素默认使用标准盒模型

​	标准盒模型中设置width/height实际上是设置content box的width/height，盒子完整的宽度需要再加上padding和border（margin不计入盒子大小，影响盒子外部空间）

##### IE（怪异）盒模型

​	需要使用box-sizing：border-box切换怪异盒模型

​	怪异和盒模型中设置width/height就是设置盒子的可见宽高（content+padding+border）

##### 外边距折叠

​	两个外边距相邻元素，外边距将会合并为一个最大外边距（仅限于块方向，一般是垂直方向）

##### 内联盒子的盒模型

​	内联盒子的width/height会被忽略，padding和margin会生效，但仍然按照内联盒子进行布局



### BFC

​	BFC是块级格式化上下文，是块盒子布局过程发生的区域和浮动元素和其他元素交互区域

#### 常用自动创建BFC方式

- 浮动元素
- 绝对定位元素
- 行内块元素
- display：tabel-cell
- display：flow-root
  - 创建无副作用的BFC
- overflow：invisible/auto
- flex元素
- grid元素

BFC包含创建它元素内部所有内容，对于浮动定位和清除浮动，外边距折叠比较重要（外边距折叠只会发生在同属一个BFC的块级元素间）

#### 父元素包含子浮动元素

​	没有BFC的块元素，其background和border只会包含内容而不包含浮动元素，而获得BFC之后的父元素会包含所有内容，包括浮动元素

- 父元素设置overflow：auto
- 父元素设置display：flow-root

#### 解决外边距塌陷

​	两个相邻块元素如果同属一个BFC会发生外边距塌陷，解决方法是在其中一个块元素外包裹一个div，为此div生成BFC



