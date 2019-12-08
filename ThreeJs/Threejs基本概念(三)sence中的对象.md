## [Group](https://threejs.org/docs/#api/objects/Group)
 分组：可以将众多的物体组合

## [LensFlare](https://threejs.org/docs/#api/objects/LensFlare)( texture, size, distance, blending, color )
镜头光晕：可以做出炫光的效果
## [Line](https://threejs.org/docs/#api/objects/Line)线段( geometry, material )
 [LineLoop](https://threejs.org/docs/#api/objects/LineLoop) 闭环的线
[LineSegments](https://threejs.org/docs/#api/objects/LineSegments) 线段
他们三个绘制模式不一样而已
#### 依次对应：
 - gl.LINE_STRIP：画一条直线到下一个顶点。
 - gl.LINE_LOOP：绘制直线到下一个顶点，并将最后一个顶点连接到第一个顶点。
 - gl.LINES：在一对顶点之间画一条线。

## [LOD](https://threejs.org/docs/#api/objects/LOD)
Level of Detail  详细程度-根据距离相机的距离显示具有或多或少几何的网格。

每个级别与对象相关联，并且可以在指定的距离之间切换渲染。通常，你会创建三个网格，一个在很远的地方（低细节），一个用于中等范围（中等细节），一个用于关闭（高细节）。
不太懂，后期补充！

## [Mesh](https://threejs.org/docs/#api/objects/Mesh)( geometry, material )
网格=形状+材质
## [Points](https://threejs.org/docs/#api/objects/Points)
点 与线段相似，绘制模式使用gl.POINTS [详情请点击](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/drawElements)
## [Skeleton](https://threejs.org/docs/#api/objects/Skeleton)
骨架
#### [Bone](https://threejs.org/docs/#api/objects/Bone)  
骨骼 可以构建骨架
## [SkinnedMesh](https://threejs.org/docs/#api/objects/SkinnedMesh)
蒙皮网格 =添加骨骼+bind骨架+再加入材质+几何体 
通过控制骨骼及骨架实现 骨骼动画。
## [Sprite](https://threejs.org/docs/#api/objects/Sprite)( spriteMaterial )
精灵
始终面向相机的贴图，只需要材质

以上对象，及其子类常用于构建物体。

# 网格=形状+材质
基本物体构建思路为：网格=形状+材质
例如构建线段：
```javascript
    var lineGeometry = new THREE.Geometry();//创建几何体
    var lineMaterial = new THREE.LineBasicMaterial({ color: 0x000000 });//创建一个线类型的基本材质，黑色

    lineGeometry.vertices.push(new THREE.Vector3(0, 0, 110));//几何体插入点
    lineGeometry.vertices.push(new THREE.Vector3(0, 110, 0));
    lineGeometry.vertices.push(new THREE.Vector3(110, 0, 0));

    var line = new THREE.Line(lineGeometry, lineMaterial);//构建线性网格
    group.add(line);
```
例如加载obj模型：
```javascript
var mtlLoader = new THREE.MTLLoader();
    mtlLoader.load('./models/a.mtl', function (materials) { 
        materials.preload();
        var objLoader = new THREE.OBJLoader();
        objLoader.setMaterials(materials);                                //添加贴图材质
        objLoader.load('./models/a.obj', function (object) {              //加载obj模型体
            object.position.set(0, 0, 0);                                 //加载成功后的处理
            scene.add(object);
        });
    });
```
