## CSS布局基础

​	布局技术用于拾取网页中元素，控制元素相对于正常布局流，周边元素，父元素或窗口的位置。

###### 布局技术

- 正常布局流
- display属性
- 浮动float
- 定位
- 弹性盒flexbox
- 网格grid

### 正常布局流

​	是网页上元素默认的布局方式，依靠HTML本身结构完成布局，是其他布局方式的基础。

##### 独立元素布局

​	默认会取得元素，然后按照盒模型添加padding，border和margin

- 块元素
  - 内容宽度默认为父元素100%，高度和自身内容高度相同
- 内联元素
  - 宽高和自身内容一致，无法设置内联元素宽高

##### 元素间影响

- 块元素
  - 按照父元素书写顺序和块流动方向放置，每个块元素在上一个元素下另起一行，被margin分隔
- 内联元素
  - 不会另起一行，只要有足够空间就会和相邻文本或内联元素在一行，如果空间不够溢出元素/文本会换行
- 外边距塌陷
  - 两个margin垂直方向会取较大值
- display属性
  - 可以更改display属性改变元素外部/内部显示方式，按照改变后显示方式进行布局

### 浮动

​	浮动最初用于简单布局，让文字环绕在浮动元素周围，后来用于网页整体布局，是传统布局方式的一种。

​	浮动的基础工作方式是令浮动元素脱离正常布局流，并吸附到父容器内容区左/右，位于该浮动元素之下文本/内联元素会围绕浮动元素（其余块元素无视浮动元素，但文本和内联元素会受浮动元素干扰）

#### 浮动存在问题

##### 设置padding，margin时难以计算宽度

​	多列浮动时为每列设置padding，margin等可能导致总宽度溢出，有列会换行。

解决：使用border-box计算尺寸，能尽可能保证总宽度

##### 清除浮动

###### 浮动元素布局规律

- 上一个元素也是浮动元素
  - 元素和该元素排列到同一行，如果行宽不够后面元素被挤到下一行（浮动元素垂直方向最高点取决于第一个浮动元素在正常流中位置）
- 上一个元素不是浮动元素
  - 仍然在上一个元素下方，根据浮动设定向左右移动，本身脱离文档流（后续浮动元素不会超过第一个浮动元素）

###### 清除浮动

- 清楚浮动并非是浮动元素回到文档流
- 清除浮动只会改变元素自身位置，并且只根据排在该元素前面的元素，让自己左边或右边没有浮动元素
- 已经浮动的元素只有排在它前面的浮动元素和父元素可以影响位置
- 清除浮动可以对浮动和非浮动元素使用，目的都是让自己位置不受之前浮动元素的影响



### 定位

​	定位允许元素覆盖基本布局流行为，用来创建浮在其他元素顶部的元素或始终停在浏览器窗口内相同位置的元素。

###### position属性值

- static
  - 每个元素默认值，按照正常布局流布局
- relative
  - 相对定位，仍然占据正常文档流位置，但可以修改最终位置并覆盖其他元素
- absolute
  - 绝对定位，元素不存在正常布局流中，位于独立的层级
- fixed
  - 固定定位，和绝对定位工作方式完全相同，只是相对于浏览器视口本身
- sticky
  - 粘滞定位，是相对定位和固定定位混合体
  - sticky定位元素的left/right等属性用来指定临界位置，而不是改变自身位置，到达临界位置之前它仍然在正常流的默认位置中

#### 绝对定位

​	定位技术中绝对定位较常用，并且也基本包含所有定位需要的知识

##### top/right/bottom/left属性

​	这四个属性用来决定定位元素的位置

- 相对定位
  - 决定元素最终所处的位置，属性值是相对于自身位置移动的距离
- 绝对定位
  - 属性值是相对于包含元素padding边界的偏移量
  - margin仍然可以影响元素的位置，但只限于元素和包含元素border-box之间，多个绝对定位元素之间不受影响也不会有margin重叠
- 固定定位
  - 属性值相对于浏览器视口的边缘
- 粘滞定位
  - 不设置属性值时，表现为相对定位
  - 设置了属性值，则该属性值是相对于视口的临界位置，滚动使元素到达该临界位置前，表现为static定位，滚动到该位置后表现为fixed定位

##### 定位上下文

​	定位上下文决定了哪些元素是绝对定位元素的包含元素，绝对定位元素的所有父元素如果position属性值都是static那么绝对定位元素会被包含在初始块容器中，根据浏览器视口定位。

​	可以通过设置父元素的position值为非static来改变定位上下文。

###### 确定包含块

​	确定元素包含块完全取决于元素position属性

- position为static，relative，sticky
  - 包含块为最近祖先块元素**内容区边缘**组成（即正常和relative定位父元素内容区可看作包含块）
- position为absolute
  - 包含块为最近的position非static的祖先元素**内边距区边缘**组成
- position为fixed
  - 包含块在连续媒体为视口

###### 盒模型属性和偏移值的百分比

​	根据的是包含块属性值

- height/top/bottom中百分比
  - 根据包含块的height值计算，如果包含块height值没有指定则为auto
- width/left/right/padding/margin中百分比
  - 根据包含块width值计算，未指定width则会根据实际值计算

##### z-index

​	决定元素重叠时，哪些元素出现在顶部

- 默认z-index值为auto，实际上为0
  - 后定位的元素会出现在上方
- 改变z-index值越大，显示越靠顶部



### 弹性盒flexbox

​	弹性盒是一种一维布局方法，元素可以膨胀或收缩适应多余空间。使用弹性盒可以解决传统布局方法难以解决的问题

- 垂直居中块元素
- 容器子项自适应占用等量宽高
- 多列布局中使所有列等高

#### flexbox模型

![](https://developer.mozilla.org/files/3739/flex_terms.png)

- 主轴
- 交叉轴
- flex项

#### 容器属性

- flex-flow
  - 属性值为两个，第一个是flex-direction，可选row/column
  - 第二个是flex-wrap，可选nowrap/wrap/wrap-reverse
- justify-content
  - 定义沿主轴的排列，分配多余空间
  - 可选值flex-start|flex-end|center|space-between|space-around|space-evenly
- align-items
  - 定义沿交叉轴排列，用于单轴情况（flex-wrap为nowrap）
  - 可选flex-start|flex-end|center|baseline
  - 默认值为strech，会纵向填充（列等高）
- align-content
  - 定义交叉轴排列，用于多轴情况（flex-wrap为wrap）
  - 可选flex-start|flex-end|center|strech|space-between|space-around|space-evenly

#### flex项属性

- flex
  - 定义有多余空间时行为，共三个值
  - 第一个值是flex-grow，有多余空间时的分配比例
  - 第二个值是flex-shrink，空间缩小时收缩比例
  - 第三个值是flex-basis，有多余空间前的默认尺寸，如果值为auto就会以元素自身尺寸为值
  - 可以缩写成一个或两个值。缩写成一个值时代表flex-grow和flex-shrink共用，且flex-basis为auto。缩写成两个值则第一个值为flex-grow和flex-shrink，第二个值时flex-basis
- align-self
  - 单独调整自身的交叉轴布局
  - 可选值和容器中align-items相同
- order
  - 可选值为数值型，调整flex-item项在主轴中顺序（默认为0）



### 网格grid

​	CSS网格是一种二维布局技术，用于把内容按行列格式排版，通常用于大型复杂布局。使用网格布局首先要定义网格元素（display：grid），然后初始化网格并设置网格项的排列。

#### 容器属性

- 初始化网格
- 内容项排列

###### 初始化网格

- 显式网格

  - **grid-template-rows/columns**
    - 显式网格，定义后网格数量不会改变
    - 值可以是 px|auto|fr|百分比|repeat函数（包含minmax函数）
    - 各值用空格分开，被中括号[]包裹的是网格线名称
  - **grid-template-areas**
    - 值是一组用空格分开的字符串，字符串代表一行中各网格名称
- 隐式网格
  - **grid-auto-rows/columns**
    - 隐式网格，只定义行/列的宽度让浏览器自动生成
    - 值类型和显式网格相同
  - **grid-auto-flow**
    - 控制网格的自动布局（包括显式和隐式）
    - 值可以是row|column|dense（不推荐使用dense）

- gap

  - 定义沟槽宽度，是两个属性缩写
  - row-gap
  - column-gap
  - 只有一个值时则row/column-gap均为该值

###### 网格项排列

- place-content
  - 当整个网格尺寸小于grid容器时，用这个属性控制行/列的位置
  - <align-content>/<justify-content>
  - 值可以是strech|start|end|center|space-between|space-around|space-evenly
- place-items
  - 控制网格项在网格中的布局
  - <align-items> /<justify-items>
  - 值可以是strech|start|end|center

#### 网格项属性

- 定位
- 布局

###### 定位

- grid-area
  - 属性值可以是区域名或一组网格线
  - 区域名：容器中定义过的区域名
  - 一组网格线：用斜线/分开，分别是row-start/column-start/row-end/column-end
    - row-end/column-end中可以使用span后跟数字或网格线名
      - 数字表示扩展行/列的数量
      - 网格线名表示扩展到该网格线

###### 布局

- place-self
  - <align-self>/<justify-self>



