```javascript
  // 添加分组
    var group = new THREE.Group();
    scene.add(group);

    //计算参数，这些参数在多处用到
    var segmentHeight = 8;//节高
    var segmentCount = 4;////节数
    var height = segmentHeight * segmentCount;
    var halfHeight = height * 0.5;

    var sizing = {
        segmentHeight: segmentHeight,
        segmentCount: segmentCount,
        height: height,
        halfHeight: halfHeight
    };
    //创建骨骼
    bones = [];
    var prevBone = new THREE.Bone();
    bones.push(prevBone);
    prevBone.position.y = - sizing.halfHeight;
    for (var i = 0; i < sizing.segmentCount; i++) {
        var bone = new THREE.Bone();
        bone.position.y = sizing.segmentHeight;
        bones.push(bone);
        prevBone.add(bone);
        prevBone = bone;
    }
    //创建形状
    var skeleton = new THREE.Skeleton(bones);
    var cylinderGeometry = new THREE.CylinderGeometry(
        5,                       // radiusTop
        5,                       // radiusBottom
        sizing.height,           // height
        8,                       // radiusSegments
        sizing.segmentCount * 3, // heightSegments
        false                     // openEnded
    )
    var material = new THREE.MeshPhongMaterial({
        skinning: true,
        color: 0x156289,
        emissive: 0xa72534,
        side: THREE.DoubleSide,
        shading: THREE.FlatShading,
        wireframe: true
    });
    //构建骨骼与网格的关联关系
    for (var i = 0; i < cylinderGeometry.vertices.length; i++) {
        var vertice = cylinderGeometry.vertices[i];
        var y = (vertice.y + sizing.halfHeight);
        var skinIndex = Math.floor(y / sizing.segmentHeight);
        var skinWeight = (y % sizing.segmentHeight) / sizing.segmentHeight;
        cylinderGeometry.skinIndices.push(new THREE.Vector4(skinIndex, skinIndex + 1, 0, 0));
        cylinderGeometry.skinWeights.push(new THREE.Vector4(1 - skinWeight, skinWeight, 0, 0));
    }

    var cylend = new THREE.SkinnedMesh(cylinderGeometry, material)


    cylend.add(bones[0]);
    cylend.bind(skeleton);
    scene.add(cylend);
    var helper = new THREE.SkeletonHelper(cylend);
    helper.material.linewidth = 1000;
    scene.add(helper);
    scene.add(cylend);
```


[简单骨骼实例](https://doter1995.github.io/three/Bone/)
