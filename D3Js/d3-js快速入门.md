# D3快速入门
> Data Driven Documents
## 简介

### 图表的绘制
在web下，支持绘图的有svg和canvas。
百度的echarts是使用canvas，阿里的AntV的底层绘图G是支持svg和canvas(ps:G4.0正在准备支持webgl，这样的话可以使用显卡增强绘制)。
今天的主角d3js是支持同时svg和canvas的。
#### svg与canvas
svg是及其形状都是标签形式的，就像提供了很多各种形状，你选择图形给上贴。而且每个形状都支持事件以及属性。
canvas是给你一个画板，给你一个笔，调api绘制来绘制图形。
svg可以很轻松的捕捉鼠标事件，比如鼠标移入到矩形等等。也可以直接操作某个形状的属性。
canvas只能先捕获事件，然后定位，计算是否在某个形状上。绘制大数据量时，性能比较好。

#### d3和echarts
echart是配置式的，需要按照文档进行配置即可。
d3是需要通过调用api，先处理数据转换。绘制，加入交互。

所以目前推荐在常规交互下的图表使用echarts，定制度较高或者重交互的图表可以使用d3js使用svg绘制。
### 关于文档
> 强烈建议速读一遍api文档，通过文档以及例子，就能很快了解实现某种图需要哪些api

详细文档请参考：
[中文文档](https://d3js.org.cn/)
[英文文档](https://github.com/d3/d3/blob/master/API.md)

## 先来演示一版
本文就以饼图举例，echarts饼图截图：
![image.png](https://upload-images.jianshu.io/upload_images/3967890-209a05c5dd966826.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 写代码之前
假如需要实现这个饼图。我们可以拆解图表：
在上面我们看到：有标题，图例，饼图上有：扇形，文字，还有浮动框，鼠标hover扇形是有变大效果，如果如果数据发生变化，我们需要更新。

#### 第一步 绘制基础结构及初始化相关参数
首先是将图表重与数据无关的dom结构绘制出来，比如：标题，图例，以及定位。
其次我们需要初始化绘制需要的相关对象。比如：d3.pies用于布局计算，d3.arc用于绘制扇形，以及tips。
![image.png](https://upload-images.jianshu.io/upload_images/3967890-bac5c896491fb748.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 第二步 更新图表加入更新动画
首先由于数据量会变，所以我们需要每次更新dom，将多余的dom删除，缺少的dom天加上，保证dom和数量一致。
数据值的更新，调用绘图，重新绘制扇形。
![image.png](https://upload-images.jianshu.io/upload_images/3967890-5e8be98d0e210b4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 第三部 加入交互
首先是饼图中扇形区域鼠标移入移出的交互，
其次是图例中矩形点击隐藏/显示该数据。
![image.png](https://upload-images.jianshu.io/upload_images/3967890-eae495a5891480cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/3967890-9a0c7bcf43c4a419.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这是我们实现的效果。
![image.png](https://upload-images.jianshu.io/upload_images/3967890-8204edf71c4050a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[在线demo](https://jsbin.com/rixicup/edit?js,output)

#### 总结通用架子
通过这个例子，我们可以看到，d3js做图表比较麻烦，所有的内容基本需要自己调用api绘制，正是这样可控性高，可以很轻松的实现各种图表定制。

d3实现一个交互图表。我们其实都是需要这三步：
第一步，初始化，与数据发生变化无关的相关参数，dom及对象
第二步，绘制函数，将数据进行绑定，绘制出与数据相关的形状及对象。
第三步，加入交互，各种交互事件，定时更新等等。

### 常见图表的绘制
柱状图，折线，散点图：直接使用比例尺，即可换算位置和大小。
d3-chord：制作玄图
d3.pie：制作饼图，环图，仪表盘
d3-geo：制作地图
d3-force：力导图
d3-hierarchy：树图，树矩形图，包等等
d3.histogram:直方图
交互类：
d3-zoom：实现缩放
d3-drag：实现拖动

