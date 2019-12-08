## 基础加载器
[Loader](https://threejs.org/docs/index.html#api/loaders/Loader)
[FileLoader](https://threejs.org/docs/index.html#api/loaders/FileLoader)
[FontLoader](https://threejs.org/docs/index.html#api/loaders/FontLoader)
[ImageLoader](https://threejs.org/docs/index.html#api/loaders/ImageLoader)
[JSONLoader](https://threejs.org/docs/index.html#api/loaders/JSONLoader)
[Cache](https://threejs.org/docs/index.html#api/loaders/Cache)
缓存
[MaterialLoader](https://threejs.org/docs/index.html#api/loaders/MaterialLoader)
[ObjectLoader](https://threejs.org/docs/index.html#api/loaders/ObjectLoader)
[TextureLoader](https://threejs.org/docs/index.html#api/loaders/TextureLoader)

[AnimationLoader](https://threejs.org/docs/index.html#api/loaders/AnimationLoader)
用于以JSON格式加载动画的类。
[AudioLoader](https://threejs.org/docs/index.html#api/loaders/AudioLoader)
加载
[BufferGeometryLoader](https://threejs.org/docs/index.html#api/loaders/BufferGeometryLoader)
用于加载BufferGeometry的装载器。这在内部使用FileLoader来加载文件。
[MaterialLoader](https://threejs.org/docs/index.html#api/loaders/MaterialLoader)
用于以JSON格式加载素材的加载器
[ObjectLoader](https://threejs.org/docs/index.html#api/loaders/ObjectLoader)
用于加载JSON资源的加载程序,此加载程序无法加载几何


## TextureLoader 纹理加载器
[TextureLoader](https://threejs.org/docs/index.html#api/loaders/TextureLoader)
基类
[CompressedTextureLoader](https://threejs.org/docs/index.html#api/loaders/CompressedTextureLoader)
基于块的纹理加载器（dds，pvr，...）的抽象基类
[CubeTextureLoader](https://threejs.org/docs/index.html#api/loaders/CubeTextureLoader)
立方体加载器
[DataTextureLoader](https://threejs.org/docs/index.html#api/loaders/DataTextureLoader)
抽象基类加载通用二进制纹理格式（rgbe，hdr，...）


[MaterialLoader](https://threejs.org/docs/index.html#api/loaders/MaterialLoader)
用于以JSON格式加载素材的加载器
[ObjectLoader](https://threejs.org/docs/index.html#api/loaders/ObjectLoader)
[TextureLoader](https://threejs.org/docs/index.html#api/loaders/TextureLoader)



## 文件加载器
[BabylonLoader](https://threejs.org/docs/index.html#examples/loaders/BabylonLoader)
.babylon
[ColladaLoader](https://threejs.org/docs/index.html#examples/loaders/ColladaLoader)
.dae
[GLTF2Loader](https://threejs.org/docs/index.html#examples/loaders/GLTF2Loader)
.gltf
[MTLLoader](https://threejs.org/docs/index.html#examples/loaders/MTLLoader)
.mtl资源的装载器 (材质)
[OBJLoader](https://threejs.org/docs/index.html#examples/loaders/OBJLoader)
.obj资源的装载器 (3d对象)
[OBJLoader2](https://threejs.org/docs/index.html#examples/loaders/OBJLoader2)
.obj资源的装载器(3d对象)
[WWOBJLoader2](https://threejs.org/docs/index.html#examples/loaders/WWOBJLoader2)
用于在web worker中加载.obj资源的装载器。(3d对象)
[PCDLoader](https://threejs.org/docs/index.html#examples/loaders/PCDLoader)
用于.pcd文件的加载程序。加载ascii和二进制。不支持压缩的二进制文件(3d对象)
[PDBLoader](https://threejs.org/docs/index.html#examples/loaders/PDBLoader)
.pdb是geometryAtoms，geometryBonds和JSON结构。
[SVGLoader](https://threejs.org/docs/index.html#examples/loaders/SVGLoader)
[TGALoader](https://threejs.org/docs/index.html#examples/loaders/TGALoader)
加载.tga文件  纹理的类。
