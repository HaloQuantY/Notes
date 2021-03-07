## Canvas入门学习

​	在页面中引入Web图像的方式有img元素，CSS的background-image属性和SVG等方式。虽然CSS和Javascript可以对SVG矢量图进行操作，但缺少对位图的支持，动画，游戏画面和3D场景的需求得不到满足。

​	HTML画布元素canvas和相关canvasAPI可以用来生成2D动画，游戏画面和数据分析图。而3D画布后来原化成WebGL，可以在web浏览器中生成3D图形。

### 基础使用

#### canvas元素

​	canvas标签的使用本身很简单，是一个HTML5标签并且只有width和height两个属性。

- 不指定width和height元素则会默认初始化为300x150像素
  - 绘制图像时会伸缩自适应
- canvas标签的内容是替换内容，如果浏览器不支持canvas则会渲染替换内容部分

#### 渲染上下文

​	The rendering context是canvas的渲染上下文，大部分操作基于渲染上下文进行。

- canvas依靠JavaScript进行操作，取得DOM后可以使用canvas.getContext()获取渲染上下文ctx
  - getContext参数是渲染上下文的类型，二维是‘2D’，三维则根据不同库进行选用（3D的底层都是WebGL）

#### 栅格

​	canvas画布以左上角为坐标原点

#### 绘制矩形

- fillRect()
  - 实心矩形
- strokeRect()
  - 矩形边框
- clearRect()
  - 清除矩形区域
- 以上三个函数参数都为x, y, width, height,并且是渲染上下文ctx的API

#### 绘制路径

- beginPath()
  - 新建路径，闭合之后生成路径
- closePath()
  - 闭合路径，绘制命令重新指向上下文
- stoke()
  - 描边
- fill()
  - 填充成实心图形，会自动闭合路径
- moveTo(x, y)
  - 移动笔触，相当于移动起点
- lineTo(x, y)
  - 绘制一条从当前位置到x，y的线段