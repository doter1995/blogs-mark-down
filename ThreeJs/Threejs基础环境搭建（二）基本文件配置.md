接下来，需要构建基础的three.js环境

# 一，创建index.html文件
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="https://cdn.bootcss.com/three.js/r83/three.js"></script>
<style type="text/css">
*{margin:0;padding:0;}
*{overflow:hidden}
</style>
    <title>Three基础测试</title>
</head>
<body>
    <div id='root' />
</body>
<script src="./index.js"></script>
</html>
```
如上，采用cdn的three.js库
# 二，构建index.js
```
// import THREE from 'three'
window.onload = function () {
    console.log("this Root verion:", THREE.REVISION)
    const R = 200;
    var Root = {
        W: window.innerWidth,
        H: window.innerHeight,
        Root: document.getElementById('root'),
    }
    var renderer = new THREE.WebGLRenderer({ antialias: true })
    //建议设置大小，否则会出现锯齿，也可能是因为我没有设置div具体宽高
    renderer.setSize(window.innerWidth, window.innerHeight);
    Root.Root.appendChild(renderer.domElement)

    //创建场景
    var scene = new THREE.Scene();

    //添加光源
    var light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(0, 150, 0);
    scene.add(light);
    //  添加辅助线
    var axisHelper = new THREE.AxisHelper(800);
    scene.add(axisHelper);

    //添加相机
    var camera = new THREE.PerspectiveCamera(60, Root.W / Root.H, 0.1, 2000);
    camera.position.set(0, 0, 1000);
    camera.lookAt(scene.position);
    scene.add(camera);
 

    //添加组
    var groupSphere = new THREE.Group();



    function animate() {
        renderer.setSize(window.innerWidth, window.innerHeight);
        //重复渲染
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
    }
    animate();
}
```

# 三，关于Threejs的提示
  个人习惯使用VSCode，但它需要配置一下才能出现提示
  1. npm安装ThreeJS库
```  
cnpm i three -s
```
2. 代码首行加入    ```  import THREE from 'three' ```
3. 测试时则需要注释掉

## 如有更好的办法解决，欢迎您的评论
