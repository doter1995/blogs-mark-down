## 一、平面类
[CircleGeometry](https://threejs.org/docs/index.html#api/geometries/CircleGeometry)
圆形
[RingGeometry](https://threejs.org/docs/index.html#api/geometries/RingGeometry)
环形 
[PlaneGeometry](https://threejs.org/docs/index.html#api/geometries/PlaneGeometry)
矩形的几何体
[ShapeGeometry](https://threejs.org/docs/index.html#api/geometries/ShapeGeometry)
通过路径构建一个多边形=>自定义形状
## 二、立体
#### 2.1简单概念的
[BoxGeometry](https://threejs.org/docs/index.html#api/geometries/BoxGeometry)
长方体 
[TetrahedronGeometry](https://threejs.org/docs/index.html#api/geometries/TetrahedronGeometry)
四面体
[ConeGeometry](https://threejs.org/docs/index.html#api/geometries/ConeGeometry)
圆锥
[CylinderGeometry](https://threejs.org/docs/index.html#api/geometries/CylinderGeometry)
圆柱
[DodecahedronGeometry](https://threejs.org/docs/index.html#api/geometries/DodecahedronGeometry)
十二面体
[IcosahedronGeometry](https://threejs.org/docs/index.html#api/geometries/IcosahedronGeometry)
二十面体
[OctahedronGeometry](https://threejs.org/docs/index.html#api/geometries/OctahedronGeometry)
八面体
[SphereGeometry](https://threejs.org/docs/index.html#api/geometries/SphereGeometry)
球体
[TorusGeometry](https://threejs.org/docs/index.html#api/geometries/TorusGeometry)
圆环几何
[TubeGeometry](https://threejs.org/docs/index.html#api/geometries/TubeGeometry)
管状体
[LatheGeometry](https://threejs.org/docs/index.html#api/geometries/LatheGeometry)
轴向对称：例如花瓶
#### 2.1 复杂概念的
[PolyhedronGeometry](https://threejs.org/docs/index.html#api/geometries/PolyhedronGeometry)
多面体 =顶点+面             
注：将它们投射到球体上，然后将其分割到所需的细节水平
[ParametricGeometry](https://threejs.org/docs/index.html#api/geometries/ParametricGeometry)
通过函数构建几何体，参量体
[TextGeometry](https://threejs.org/docs/index.html#api/geometries/TextGeometry)
文字体
[TorusKnotGeometry](https://threejs.org/docs/index.html#api/geometries/TorusKnotGeometry)
创建圆环结，其特殊形状由一对互质整数p和q定义。如果p和q不是互质的，结果将是环面连接。
[ExtrudeGeometry](https://threejs.org/docs/index.html#api/geometries/ExtrudeGeometry)
伸出的几何体,该对象将2D形状挤出到3D几何体。
## 三、辅助类
[EdgesGeometry](https://threejs.org/docs/index.html#api/geometries/EdgesGeometry)
获取几何体的边线
[WireframeGeometry](https://threejs.org/docs/index.html#api/geometries/WireframeGeometry)
作帮助对象来将几何 对象视为线框

![如图-线框](http://upload-images.jianshu.io/upload_images/3967890-15529eeadf572f8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
