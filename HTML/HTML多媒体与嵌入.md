## HTML多媒体与嵌入

### HTML中的图片

#### img元素

​	<img>元素可以将图片放到网页中。本身是一个空元素，需要一个src属性来指定图片路径，可以是相对路径或绝对URL。

​	绝对URL不受推荐，因为会重新通过DNS寻找IP地址，通常将图片和HTML放在同一个服务器。

#### 备选文本alt

​	是图片的文字描述，图片无法显示时可显示备选文本。

​	alt备选文本对于SEO和无法直接查看图片的用户而言很有意义。

#### 宽度和高度

​	width和height属性指定图片的宽高，在图片没有正常显示的情况下浏览器也会预留指定宽高的空间。

​	但通常不会使用HTML改变图片尺寸，一般会在图片编辑器中修改图片尺寸或者使用CSS修改。

#### title图片标题

​	鼠标悬停时会进行提示，和链接标题相同。但title属性浏览器支持性不好，并且对图片的描述通常会放在正文中。

#### figure和figcaption元素

​	是HTML5中的语义化元素，用于为图片和图片描述创建语义化描述的元素。

#### CSS背景图片

​	CSS中可以通过background-image和background-*属性来设置背景图片，拥有更多对图片的控制效果，但是CSS仅用来提升视觉效果而没有HTML的语义意义。

​	如果图片仅仅是装饰可以使用CSS，如果在内容中有意义则应该使用HTML图片。



### 音频和视频

​	在网页中嵌入音频和视频，并且为视频添加字幕。

​	在HTML5之前，传统web技术不能够嵌入音视频，因此flash是处理音视频的主流，但flash由安全性问题和对HTML/CSS支持性问题等。

​	HTML5中提出了<video><audio>标签，可以在网页中嵌入音视频并且使用Javascript进行控制。

#### video元素

​	src指向要嵌入网页的视频资源

​	controls布尔属性表示使用浏览器提供的播放控件，可以使用JavascriptAPI创建自定义控件

​	video标签内容是后备内容，浏览器不支持video标签时会显示标签内容

#### 使用多个播放源提高兼容性

​	不同浏览器对于视频格式支持不同

##### 容器格式

- MP3

- MP4

- WebM

  容器格式定义了构成媒体文件的音视频轨道存储结构，以及描述媒体文件的元数据和编码译码器

  ![WebM](https://mdn.mozillademos.org/files/16898/ContainersAndTracks.svg)

  不同浏览器支持不同容器格式，而不同的容器格式又使用不同的编码器来编成不同格式的音视频。

##### source元素

​	用于兼容不同的浏览器支持格式问题，在video标签中src属性不再需要，而是转载video标签内部放入source元素，在source元素上写入src和type属性

#### 其他video属性

- width/height
- autoplay
- loop
- muted
- poster
- preload

#### audio标签

​	audio标签和video标签使用方法几乎完全相同，不过仅支持音频文件。如果链接到一个视频文件则会播放其音频轨。

#### 显示音轨文本

​	在video标签中可以加入track元素，track可以用来添加字幕，其拥有src，kind和srclang三个属性分别定义字幕源，字幕种类和字幕语言。

​	track元素需要链接一个WebVTT格式文件。



### 其他嵌入技术

​	<iframe>，<embed>，<object>三种元素可以嵌入各种内容类型元素。其中<iframe>用于嵌入其他网页，另外两个嵌入PDF，SVG和Flash等。

#### iframe

​	iframe允许嵌入其他web文档，相比其他两种嵌入技术是一种新型功能强大的元素。

##### iframe基本属性

- allowfullscreen
- frameborder 嵌入内容和其他框架之间绘制边框
- src 文档源
- width/height
- sandbox 提高安全性设置

##### 安全隐患

- 在必要时嵌入
- 使用HTTPS协议，绝对不使用HTTP协议直接嵌入iframe
- 始终使用sandbox属性

#### embed和object

​	这两种元素和iframe不同，用于嵌入多种类型外部内容，是通用嵌入工具。



### 添加矢量图形

​	如何嵌入SVG图形到网页中

#### 矢量图

​	矢量图形是相对于位图的一种图形格式，位图是由像素点构成，包含了每个像素位置和色彩信息，包括.bmp .png .jpg .gif格式。

​	矢量图形则是利用算法定义的，包含了图形和路径的定义，在缩放时不会失真，且拥有更小的体积。

#### SVG

​	SVG时哦那个与描述矢量图像的XML语言，基本上和HTML一样标记。

- img

  - 无法控制SVG内容，无法应用JS和CSS
  - 需要srcset进行兼容，在src填写备用图像源
- svg内嵌

  - 减少HTTP请求
  - 可以应用CSS
  - 不适于复用
- iframe

  - 在iframe标签中可以填写备用图片
  - 跨域问题




### 自适应图片

