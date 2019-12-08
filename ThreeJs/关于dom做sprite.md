### 问题：需要为3d中的物体加入dom标签。用于展示状态属性。
#### 思路一：
使用canvas作为贴图。使用sprite。使之面向屏幕。后来发现在3d中canvas的大小和文字大小不易控制，导致显示字体模糊。（放弃 ）

#### 思路二：
后来使用通过正在3d种标记点。然后调用调用点的世界坐标投影计算屏幕的位置
```javascript
var vector = object3d.getWorldPosition().project(camera)
var  x= Math.round(vector.x * Root.W/2 + Root.W/2)
var  y= Math.round(-vector.y * Root.H/2 + Root.H/2)
```
后记：关于getWorldPosition是为了解决，camera和object3d在旋转的场景中，不变化。
因为我的写法是外层group绕y旋转。相机和group内部的object不做处理。这样的话必须获取对应对象的世界坐标。 
### 问题2：dom标签层级问题。
由于标签是一直浮动在canvas上面的，根据其dom生成的顺序，导致dom的层级是固定的，这个样的话会导致标签显示的层级不正确。
![在右上方明显有层级错误](http://upload-images.jianshu.io/upload_images/3967890-0c3b23c1dfe982eb.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为解决这个问题,通过计算点到相机的距离，根据距离排序，重设dom的层级关系。![示例代码](http://upload-images.jianshu.io/upload_images/3967890-a9d09c251f71871d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最终效果：
![GIFaa.gif](http://upload-images.jianshu.io/upload_images/3967890-753d0808afbd1d22.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

