## CSS变形，过渡，动画

### 变形transform

​	通过改变坐标空间，transforms可以不影响正常流情况下改变作用内容的位置。通过一些列CSS属性，transforms可以对HTML元素进行线性仿射变形，包括旋转，倾斜，缩放和位移四种，并且这些变形适用于平面和三维空间。

​	只有盒模型定位的元素才可以使用transform，一般指display：block元素。

​	transform为非none值时会在元素上创建一个局部坐标系统并且应用于该元素，通过元素变换模型可以绘制渲染出自己的坐标系统。

#### 变换模型

​	变换模型会通过如下transform和transform-origin属性进行计算

- 从具体模型开始
- 通过transform-origin坐标计算之进行移动
  - 元素的transform-origin起始点永远不变，为元素初始状态的原点（左上角）
- 从左至右符合应用transform属性种的transform函数
- 无效化tranform-origin并返回至初始状态远点
  - 即每次改变transform-origin都从初始原点开始
  - 但应用transform函数时，如果没有指定则tranform-origin默认在元素中心（50%， 50%）

#### transforms属性

  定义transform的属性主要是两个

- transform-origin
  - 指定原点位置，默认为元素中心
- transform
  - 指定作用在元素上的变形

##### 四种transform函数

- translate
  - 第一值为x方向，第二值为y方向
- scale
  - 放大倍数
- skew
  - 角度
  - 默认和skewX相同
- rotate
  - 角度



### 过渡transition

​	过渡可以为一个元素在不同状态间切换时定义不同过渡效果，比如在不同伪类之间切换如：hover，：active或通过Javascript实现的状态变化。

#### 语法

​	transition属性指定一个或多个CSS属性过渡效果，多个属性间用逗号分隔。

###### 值的类型

- 转换属性
  - 可以是none，all或逗号分开的属性名称
- 过渡函数
- 过渡持续和延迟时间
  - transition-duration
  - transition-delay



### CSS动画

​	CSS animations使元素可以从一个CSS样式配置转换到另一个CSS样式配置。动画包含两部分，描述动画的样式规则和用于指定动画开始，结束和中间点样式的关键帧。

###### CSS动画的优点

- 创建简单，不需要Javascript
- 动画运行效果和兼容性好，渲染引擎会自适应优化，而Javascript达到同样效果较难
- 浏览器可以控制动画序列，允许浏览器根据负载优化性能和效果

#### 配置动画

​	创建动画序列需要使用animation属性或其子鼠星，允许配置动画时间，时长和其他动画细节，但不能配置动画实际表现（定义动画怎么播放）

​	动画的实际内容由关键帧规则实现。

##### animation属性

​	animation属性规定了元素应用动画的规则，有以下子属性：

- animation-name
  - 指定关键帧
- animation-duration
  - 动画一个周期时长
- animation-delay
  - 延时，元素加载完成后到动画序列执行间时长
- animation-timing-function
  - 动画加速度曲线，设置关键帧间变化
- animation-fill-mode
  - 动画执行前后如何为元素应用样式
- animation-direction
  - 动画运行完后时反向运行还是重新运行
- animation-literation-count
  - 动画重复次数
- animation-play-state
  - 允许暂停

动画一般都需要定义name，duration，iteration-count和direction

#### 定义动画序列

​	完成动画的播放设置后，需要定义动画表现，通过@keyframes可以定义两个以上关键帧实现，每个关键帧都表述了动画元素子给定时间点上应该如何渲染。

###### @keyframes

- 关键帧用百分比进行定义，每个百分比对应一组CSS规则
- 0%和100%是最关键的时刻，也可以用关键字from/to来表示



