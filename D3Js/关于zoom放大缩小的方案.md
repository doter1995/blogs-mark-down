  之前做了一个[时间轴折线图](https://doter1995.github.io/d3/show/timeLine/)，处理思路是，在线条上层绘制一个rect，然后屏蔽掉折线区域，之后为rect添加zoom事件。
昨天在做一个时间轴的非连续线段,需要加入线段的点击事件，由于rect将线条屏蔽，所以思路是，通过将mouse位置x,y转化为时间及数值，然后通过计算，判断位于那个线段只上。
 今天早上回顾了下，感觉逻辑有点南辕北辙，因此将rect下调一层，将线段层上移，之后为rect，线条都添加相同的zoom事件。之后直接为线段添加onClick事件。
```
    //添加缩放事件
    zoomRoot.call(zoom)
    lines.call(zoom)
    Tip.call(zoom)
```
#### 活学活用，分析比写代码更重要。 
