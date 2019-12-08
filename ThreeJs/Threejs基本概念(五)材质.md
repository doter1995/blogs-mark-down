[Material](https://threejs.org/docs/index.html#api/materials/Material)
材质属于基类
它们以（大多数）与渲染器无关的方式定义，因此如果您决定使用不同的渲染器，则不需要重写材料。
[PointsMaterial](https://threejs.org/docs/index.html#api/materials/PointsMaterial)
点的材质

[LineBasicMaterial](https://threejs.org/docs/index.html#api/materials/LineBasicMaterial)
线的基础材质
[LineDashedMaterial](https://threejs.org/docs/index.html#api/materials/LineDashedMaterial)
虚线的基础材质

[MeshBasicMaterial](https://threejs.org/docs/index.html#api/materials/MeshBasicMaterial)
网格基础材质

[MeshDepthMaterial](https://threejs.org/docs/index.html#api/materials/MeshDepthMaterial)
网格深度材质 根据网格到相机距离 染色

[MeshLambertMaterial](https://threejs.org/docs/index.html#api/materials/MeshLambertMaterial)
网格朗伯材质 考虑光照影响，只考虑漫反射，不考虑镜面反射   例如木材等
[MeshNormalMaterial](https://threejs.org/docs/index.html#api/materials/MeshNormalMaterial)
网格法向材质 根据网格的法向量计算颜色

[MeshPhongMaterial](https://threejs.org/docs/index.html#api/materials/MeshPhongMaterial)
光泽表面材质，镜面高光。考虑镜面反射     例如：油漆等
[MeshToonMaterial](https://threejs.org/docs/index.html#api/materials/MeshToonMaterial)
MeshPhongMaterial的扩展与遮蔽。

[MeshStandardMaterial](https://threejs.org/docs/index.html#api/materials/MeshStandardMaterial)
在实践中，这比MeshLambertMaterial 或MeshPhongMaterial提供了一个更准确和逼真的结果，其成本在于计算成本更高。
[MeshPhysicalMaterial](https://threejs.org/docs/index.html#api/materials/MeshPhysicalMaterial)
MeshStandardMaterial的扩展，可以更好地控制反射率。

[ShaderMaterial](https://threejs.org/docs/index.html#api/materials/ShaderMaterial)
使用自定义着色器渲染的材质
[RawShaderMaterial](https://threejs.org/docs/index.html#api/materials/RawShaderMaterial)
类与ShaderMaterial类似，除了内置制服和属性的定义不会自动添加到GLSL着色器代码中。

[ShadowMaterial](https://threejs.org/docs/index.html#api/materials/ShadowMaterial)
阴影材质

[SpriteMaterial](https://threejs.org/docs/index.html#api/materials/SpriteMaterial)
精灵材质
