## 环境光[AmbientLight](https://threejs.org/docs/index.html#api/lights/AmbientLight)
为场景提供整体的光照效果，没有明确光源，各处形成亮度一致
##点光源[PointLight](https://threejs.org/docs/index.html#api/lights/PointLight)
没有大小，只是一个点
## 平行光[DirectionalLight](https://threejs.org/docs/index.html#api/lights/DirectionalLight)
例如太阳光
平行光照的亮度相同，于平面所在位置无关
## 聚光灯[SpotLight](https://threejs.org/docs/index.html#api/lights/SpotLight)
一种特殊的点光源
朝某方向投射光线，光线形式为圆锥形
## 矩形平面光[RectAreaLight](https://threejs.org/docs/index.html#api/lights/RectAreaLight)
此灯可以投射阴影，这个类目前正在积极开发中，可能还没有生产就绪（从r83起）


注
##阴影
只有平行光、聚光灯、矩形平面光可以实现阴影，能表现阴影的材质也只有LambertMaterial和PhongMaterial
