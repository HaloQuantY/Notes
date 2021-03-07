## Bootstrap学习

### 容器

#### 流体容器.container-fluid

​	弹性容器，相当于width=100%的盒子。

#### 固定容器.container

​	宽度阈值

​		[1200, 100%]→1170px

​		[992, 1200]→970px

​		[768, 992]→750px

​		[0, 768]→auto



### 栅格系统源码分析

​	栅格默认分成12列，使用.row和.col-xx-yy来进行栅格划分。

```less
// 固定容器
.container {
  .container-fixed();

  @media (min-width: @screen-sm-min) {
    width: @container-sm;
  }
  @media (min-width: @screen-md-min) {
    width: @container-md;
  }
  @media (min-width: @screen-lg-min) {
    width: @container-lg;
  }
}
// 流体容器
.container-fluid {
  .container-fixed();
}

// 生成行
.row {
  .make-row();
}

// 生成列
.make-grid-columns();
// 生成网格（移动端优先）
.make-grid(xs);
// 根据媒体查询生成对应网格
@media (min-width: @screen-sm-min) {
  .make-grid(sm);
}
@media (min-width: @screen-md-min) {
  .make-grid(md);
}
@media (min-width: @screen-lg-min) {
  .make-grid(lg);
}
```



##### 行

​	生成行（设置左右外边距和清除浮动）

```less
.make-row(@gutter: @grid-gutter-width) {
  margin-left:  ceil((@gutter / -2));
  margin-right: floor((@gutter / -2));
  &:extend(.clearfix all);
}
.row {
  .make-row();
}
```

##### 列

- 生成列

  将所有列设置相对定位，给与最小高度以及左右内边距

```less
.make-grid-columns() {
  // Common styles for all sizes of grid columns, widths 1-12
  .col(@index) { // initial
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general; "=<" isn't a typo
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      position: relative;
      // Prevent columns from collapsing when empty
      min-height: 1px;
      // Inner gutter via padding
      padding-left:  ceil((@grid-gutter-width / 2));
      padding-right: floor((@grid-gutter-width / 2));
    }
  }
  .col(1); // kickstart it
}
```

- 生成网格

```less
.make-grid(@class) {
  .float-grid-columns(@class);
  .loop-grid-columns(@grid-columns, @class, width);
  .loop-grid-columns(@grid-columns, @class, pull);
  .loop-grid-columns(@grid-columns, @class, push);
  .loop-grid-columns(@grid-columns, @class, offset);
}
```

​	其中第一步将网格浮动

```less
.float-grid-columns(@class) {
  .col(@index) { // initial
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      float: left;
    }
  }
  .col(1); // kickstart it
}
```

​	随后进行循环loop-grid-columns

```less
.loop-grid-columns(@index, @class, @type) when (@index >= 0) { // @index12，@class尺寸
  .calc-grid-column(@index, @class, @type);
  // next iteration
  .loop-grid-columns((@index - 1), @class, @type);
}
```

​	循环内部是计算网格尺寸和位置

```less
// 设定列宽
.calc-grid-column(@index, @class, @type) when (@type = width) and (@index > 0) {
  .col-@{class}-@{index} {
    width: percentage((@index / @grid-columns));
  }
}

// 列排序，push设置网格的左偏移，并将宽度为0网格左边距设为auto（网格右移动）
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index > 0) {
  .col-@{class}-push-@{index} {
    left: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index = 0) {
  .col-@{class}-push-0 {
    left: auto;
  }
}

// 列排序，pull设置网格的右偏移，并将宽度为0网格右边距设为auto(网格左移)
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index > 0) {
  .col-@{class}-pull-@{index} {
    right: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index = 0) {
  .col-@{class}-pull-0 {
    right: auto;
  }
}

// 列偏移，设置网格的左边距，并将宽度为0网格左边距设为0
.calc-grid-column(@index, @class, @type) when (@type = offset) {
  .col-@{class}-offset-@{index} {
    margin-left: percentage((@index / @grid-columns));
  }
}
```

#### 栅格的盒模型

​	容器：15px左右padding，使容器能够完全包含行

​	行： -15px左右margin，防止列中嵌套行时改变槽宽

​	列： 15px左右padding，实现槽宽





