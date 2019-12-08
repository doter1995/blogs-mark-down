# 概念
Physijs建立在[ammo.js](https://github.com/kripken/ammo.js/)之上
# 使用
### 五步
1. 导入physi.js
2. 配置Physijs.scripts.worker和Physijs.scripts.ammo
3. 用于Physijs.Scene代替THREE.Scene
4. THREE.Mesh，选择的Physijs替换
5. 调用scene.simulate方法，当您渲染或每当你想迭代物理设置
### 基本形状
Physijs.PlaneMesh - 零厚度平面
Physijs.BoxMesh -  THREE.CubeGeometry
Physijs.SphereMesh -  THREE.SphereGeometry
Physijs.CylinderMesh -  THREE.CylinderGeometry
Physijs.ConeMesh- THREE.CylinderGeometry（锥形）
Physijs.CapsuleMesh- THREE.CylinderGeometry，除了两端半球
Physijs.ConvexMesh - 具有的任何凸几何
Physijs.ConcaveMesh - 任何凹面几何，即任意网格
Physijs.HeightfieldMesh - 匹配z坐标中给出的高度值的常规网格

### 更新和回调
因为Physijs在与主应用程序不同的线程上运行，所以不能保证每次调用时都可以迭代场景scene.simulate。因此，您可以将事件侦听器附加到运行物理模拟时触发的场景。
```
var scene = new Physijs.scene;
scene.addEventListener( 'update', function() {
    // the scene's physics have finished updating
});
```
添加一个对象
```
var readyHandler = function() {
    // object has been added to the scene
};
var mesh = new Physijs.SphereMesh( geometry, material );
mesh.addEventListener( 'ready', readyHandler );
scene.add( mesh );
````
### 碰撞检测
实例代码：
```
var mesh = new Physijs.SphereMesh(
    new THREE.SphereGeometry( 3 ),
    new THREE.MeshBasicMaterial({ color: 0x888888 })
);
mesh.addEventListener( 'collision', function( other_object, relative_velocity, relative_rotation, contact_normal ) {
    // `this` has collided with `other_object` with an impact speed of `relative_velocity` and a rotational force of `relative_rotation` and at normal `contact_normal`
});
```
其中碰撞事件的四个参数分别是：
相撞物体,速度差，角速度的差,接触之间
##### Motion Clamping(运动限制?)
后续研究
```
// Enable CCD if the object moves more than 1 meter in one simulation frame
//在一个模拟框架中，如果物体移动超过1米，就启用CCD
mesh.setCcdMotionThreshold(1);

// Set the radius of the embedded sphere such that it is smaller than the object
//设置嵌入球体的半径，使其小于物体的半径。
mesh.setCcdSweptSphereRadius(0.2);
```
### 复合形状
复合形状是将复杂几何添加到场景中的有效方法，并通过将Physijs中的[可用形状](https://github.com/chandlerprall/Physijs/wiki/Basic-Shapes)拼接在一起来创建更大，更复杂的几何体来创建。
```
var parent = new Physijs.BoxMesh( new THREE.CubeGeometry( 5, 5, 5 ), new THREE.MeshBasicMaterial({ color: 0x888888 }) );

var child = new Physijs.SphereMesh( new THREE.SphereGeometry( 2.5 ), new THREE.MeshBasicMaterial({ color: 0x888888 }) );
child.position.z = 5;

parent.add( child );
scene.add( parent );
```
### 约束
##### 点对点
```
var constraint = new Physijs.PointConstraint(
    physijs_mesh_a, // First object to be constrained
    physijs_mesh_b, // OPTIONAL second object - if omitted then physijs_mesh_1 will be constrained to the scene
    new THREE.Vector3( 0, 10, 0 ) // point in the scene to apply the constraint
);
scene.addConstraint( constraint );
```
###### 铰链约束
```
var constraint = new Physijs.HingeConstraint(
    physijs_mesh_a, // First object to be constrained
    physijs_mesh_b, // OPTIONAL second object - if omitted then physijs_mesh_1 will be constrained to the scene
    new THREE.Vector3( 0, 10, 0 ), // point in the scene to apply the constraint
    new THREE.Vector3( 1, 0, 0 ) // Axis along which the hinge lies - in this case it is the X axis
);
scene.addConstraint( constraint );
constraint.setLimits(
    low, // minimum angle of motion, in radians
    high, // maximum angle of motion, in radians
    bias_factor, // applied as a factor to constraint error
    relaxation_factor, // controls bounce at limit (0.0 == no bounce)
);
constraint.enableAngularMotor( target_velocity, acceration_force );
constraint.disableMotor();
```
###### 滑块约束
```
var constraint = new Physijs.SliderConstraint(
    physijs_mesh_a, // First object to be constrained
    physijs_mesh_b, // OPTIONAL second object - if omitted then physijs_mesh_1 will be constrained to the scene
    new THREE.Vector3( 0, 10, 0 ), // point in the scene to apply the constraint
    new THREE.Vector3( 1, 0, 0 ) // Axis along which the hinge lies - in this case it is the X axis
);
scene.addConstraint( constraint );
constraint.setLimits(
    linear_lower, // lower limit of linear movement, expressed in world units
    linear_upper, // upper limit of linear movement, expressed in world units
    angular_lower, // lower limit of angular movement, expressed in radians
    angular_upper // upper limit of angular movement, expressed in radians
);
constraint.setRestitution(
    linear, // amount of restitution when reaching the linear limits
    angular // amount of restitution when reaching the angular limits
);
constraint.enableLinearMotor( target_velocity, acceration_force );
constraint.disableLinearMotor();
constraint.enableAngularMotor( target_velocity, acceration_force );
constraint.disableAngularMotor();
```
###### 锥形约束
```
var constraint = new Physijs.ConeTwistConstraint(
    physijs_mesh_a, // First object to be constrained
    physijs_mesh_b, // Second object to be constrained
    new THREE.Vector3( 0, 10, 0 ), // point in the scene to apply the constraint
);
scene.addConstraint( constraint );
constraint.setLimit( x, y, z ); // rotational limit, in radians, for each axis
constraint.setMotorMaxImpulse( max_impulse ); // float value of the maximum impulse the motor can apply toward its target
constraint.setMotorTarget( target ); // target is the desired rotation for the constraint and can be expressed by a THREE.Vector3, THREE.Matrix4, or THREE.Quaternion
constraint.enableMotor();
constraint.disableMotor();
```
###### 自由度约束
```
var constraint = new Physijs.DOFConstraint(
    physijs_mesh_a, // First object to be constrained
    physijs_mesh_b, // OPTIONAL second object - if omitted then physijs_mesh_1 will be constrained to the scene
    new THREE.Vector3( 0, 10, 0 ), // point in the scene to apply the constraint
);
scene.addConstraint( constraint );
constraint.setLinearLowerLimit( new THREE.Vector3( -10, -5, 0 ) ); // sets the lower end of the linear movement along the x, y, and z axes.
constraint.setLinearUpperLimit( new THREE.Vector3( 10, 5, 0 ) ); // sets the upper end of the linear movement along the x, y, and z axes.
constraint.setAngularLowerLimit( new THREE.Vector3( 0, -Math.PI, 0 ) ); // sets the lower end of the angular movement, in radians, along the x, y, and z axes.
constraint.setAngularUpperLimit( new THREE.Vector3( 0, Math.PI, 0 ) ); // sets the upper end of the angular movement, in radians, along the x, y, and z axes.
constraint.configureAngularMotor(
    which, // which angular motor to configure - 0,1,2 match x,y,z
    low_limit, // lower limit of the motor
    high_limit, // upper limit of the motor
    velocity, // target velocity
    max_force // maximum force the motor can apply
);
constraint.enableAngularMotor( which ); // which angular motor to configure - 0,1,2 match x,y,z
constraint.disableAngularMotor( which ); // which angular motor to configure - 0,1,2 match x,y,z
```
### 冻结一个对象
可以使用两种方法来使对象冻结或不可移动。
1. 如果对象始终是静态的，例如地面，则可以0使用第三个参数创建网格时将其设置为质量：new Physijs.BoxMesh( geometry, material, 0)。任何具有质量的对象0将永远是静态的。
2. 用于对象在某些时候是静态的，并且在其他方​​面是动态的。
The second method can be used for objects when they will be static at some times and dynamic at others, like in the [jenga example](https://github.com/chandlerprall/Physijs/blob/master/examples/jenga.html#L166). If you call object.setAngularFactor
 and object.setLinearFactor
with a THREE.Vector3( 0, 0, 0 )
 then no energy will be applied to the object. You can use object.setAngularVelocity
 and object.setLinearVelocity
 in the same way to clear any velocities the object may already have. At a later point you can reset the object's linear and angular factors to ( 1, 1, 1 )
, again as it's done in the [jenga example](https://github.com/chandlerprall/Physijs/blob/master/examples/jenga.html#L212).
### 材质Materials
在THREE材质基础上增加了摩擦度和恢复度
```
var friction = 0.8; // 摩擦度
var restitution = 0.3; // 恢复度
var material = Physijs.createMaterial(
    new THREE.MeshBasicMaterial({ color: 0x888888 }),
    friction,
    restitution
);
var mesh = new Physijs.BoxMesh(
    new THREE.CubeGeometry( 5, 5, 5 ),
    material
);
```
### 暂停/恢复模拟
```
var render = function() {
    if (!isPaused) {
        scene.simulate();
    }
    renderer.render();
};
var unpauseSimulation = function() {
    isPaused = false;
    scene.onSimulationResume();
};
```
恢复模拟需要调用场景的onSimulationResume方法.

### 场景配置
```
var scene = new Physijs.Scene({ reportsize: 50, fixedTimeStep: 1 / 60 });
```
1. fixedTimeStep  default=1/60 此数字确定模拟步骤的模拟时间。数字越小，模拟越准确
2. broadphase  指定将使用哪个宽带，选择是dynamic和sweepprune。
3. reportsize default 50 作为优化，包含对象位置的世界报告基于此数字预先初始化。最好将其设置为您的场景将具有的对象数量。
4. setGravity方法  default ( 0, -10, 0 )  设定重力的数量和方向
5. setFixedTimeStep 在构造函数中default 1 / 60 重置fixedTimeStep给定的值

### 更新对象的位置和旋转
有一个方面，无法与three.js进行无缝集成：更改对象的位置和/或旋转。如果这样做，您必须将该对象__dirtyPosition或__dirtyRotation标志设置为true，否则将从模拟中的最后一个已知值覆盖。
```
var mesh = new Physijs.BoxMesh( geometry, material );
scene.add( mesh );

var render = function() {
    // Change the object's position
    mesh.position.set( 0, 0, 0 );
    mesh.__dirtyPosition = true;

    // Change the object's rotation
    mesh.rotation.set(0, 90, 180);
    mesh.__dirtyRotation = true;
    
    // You may also want to cancel the object's velocity
    mesh.setLinearVelocity(new THREE.Vector3(0, 0, 0));
    mesh.setAngularVelocity(new THREE.Vector3(0, 0, 0));
    
    scene.simulate();
    renderer.render();
};
```
