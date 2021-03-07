## CSS属性值单位

### 长度单位

​	长度单位中重点是相对单位的相对基准

- 绝对长度单位
  - 常用px
- 相对长度单位
  - em
    - 相对于父元素的字体大小
  - rem
    - 根元素的字体大小
  - vw
    - 视窗宽度1%
  - vh
    - 视窗高度1%
  - lh
    - 当前元素的line-height

### 百分比的参照基础

​	不同属性的值为百分比时参照的基础通常是父元素，但细节会有不同

- width/height

  - width属性为百分比值总是以父元素宽度为基础
  - height属性只有父元素设置了高度才会生效，否则为auto
    - html标签可以使用百分比高度，基础是视口高度

- padding/margin

  - 这两个属性在所有方向上的值都参考包含块的宽度(包括padding-top，margin-top等)

- background-position

  - background-position参照的是背景图片的尺寸减去放置背景图区域所得到的负值
  - 水平位置参照宽度负值，垂直位置参照高度负值

  ![](http://segmentfault.com/img/bVcDUv)

- font-size
  - 参照父元素font-size
- line-height
  - 参照自身font-size
- bottom/left/right/top
  - left/right参照父元素宽度
  - bottom/top参照父元素高度
- transform：translate
  - 参照自身的border-box尺寸