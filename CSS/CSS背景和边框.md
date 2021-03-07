## CSS背景和边框

​	CSS的background属性是设置元素背景属性的简写，包含的值较多且复杂。

### background属性

​	background属性是以下属性的简写：

- background-color
  - 值为任何color值，改变元素margin内所有区域背景色
- background-image
  - 值为url（）函数，改变元素margin内所有区域背景图片，会覆盖背景色
  - background-repeat
    - 控制背景图像是否平铺
    - no-repeat
    - repeat-x
    - repeat-y
    - repeat
  - background-size
    - 设置背景图片大小，可以是关键字cover，contain，百分比和px等
    - 关键字
      - cover保持原图比例覆盖
      - contain保证元素能容纳背景图片，会改变原图比例
    - 百分比
      - 默认相对于padding-box宽高设置尺寸，第一个尺寸宽第二个尺寸高
  - background-position
    - 设置背景图片位置，可以使用关键字或百分比，px等
    - 关键字top|right|bottom|left|center
      - 关键字没有顺序
    - 百分比
      - 第一个为横向坐标，第二个为纵向
      - 默认以padding-box宽高为参照
  - gradient类型
    - 值为gradient函数，多为linear-gradient和radial-gradient
    - linear-gradient
      - 值为两个，角度和颜色
    - radial-gradient
      - 第一个值为circle或描述形状关键字
  - background-attachment
    - 控制滚动时的表现
      - scroll跟随滚动
      - fixed固定不动

###### 使用background属性时规则

- background-color
  - 只能在逗号之后指定（之前是用空格隔开的image设定）
- background-size
  - 只能跟在background-position后，用/隔开（实际上两个属性一组）

