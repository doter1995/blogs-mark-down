# 上卷

## 1.声明问题

如下例子：

```javascript
foo(a)
var a=1;
console.log(a);// undefined
function foo(a){
    console.log(a);//1
}
```

执行前编译：会将所有声明提前

```javascript
var a;
function foo(a){
    console.log(a);
}
```

最后执行：

```javascript
foo(a);
a=1;
console.log(a)
```

再来一个有趣的例子：

```javascript
foo(a)
var a=1;
console.log(a);
var foo = function(a){
    console.log(a);
}
```

报错：foo is not a function
原因是foo的声明提前了，但是赋值在后面。foo()执行的时候，其实是undefined
> es6中的let 不会声明提前

```javascript
foo(a)//error: a is not defined
let a=1;
console.log(a);
var foo = function(a){
    console.log(a);
}
```

#### 2.闭包

```javascript
function foo() {
    var a = 2;
    function bar() {
        console.log( a );
    }
    return bar;
}
var baz = foo();
baz(); // 这就是闭包的效果。
```

闭包解决的问题

```javascript
for (var i=1; i<=5; i++) {
  setTimeout( function timer() {
    console.log( i );
  }, i*1000 );
}
```

结果：`6 6 6 6 6`

原因是timer()中的i是引用，而真实数据在for循环后为6

```javascript
for (var i=1; i<=5; i++) {
    (function(j) {
        setTimeout( function timer() {
                console.log( j );
        }, j*1000 );
    })( i );
}
```

结果：1 2 3 4 5

```javascript
// let 为es6
for (let i=1; i<=5; i++) {
    setTimeout( function timer() {
        console.log( i );
    }, i*1000 );
}
```

结果：1 2 3 4 5

#### 3.模块

```javascript
function CoolModule() {
    var something = "cool";
    var another = [1, 2, 3];
    function doSomething() {
        console.log( something );
    }
    function doAnother() {
        console.log( another.join( " ! " ) );
    }
    return {
        doSomething: doSomething,
        doAnother: doAnother
    };
}
var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

#### this

this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调
用时的各种条件。this 的绑定只取决于函数的调用方式。

当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。这个记录会包
含函数在哪里被调用（调用栈）、函数的调用方法、传入的参数等信息。this 就是记录的其中一个属性，会在函数执行的过程中用到。

```javascript
function identify() {
    return this.name.toUpperCase();
}
function speak() {
    var greeting = "Hello, I'm " + identify.call( this );
    console.log( greeting );
}
var me = {
    name: "Kyle"
};
var you = {
    name: "Reader"
};
identify.call( me ); // KYLE
identify.call( you ); // READER
speak.call( me ); // Hello, 我是 KYLE
speak.call( you ); // Hello, 我是 READER
```

#### this的指向

指向所在方法的调用者

```javascript
function foo() {
    console.log( this.a );
}
var obj2 = {
    a: 42,
    foo: foo
};
var obj1 = {
    a: 2,
    obj2: obj2
};
obj1.obj2.foo(); // 42
```

显示绑定this

```javascript
function foo() {
    console.log( this.a );
}
var obj={
    a:1
}
foo.call(obj)// 1
```

显式绑定优先级大于隐式

```javascript
function foo() {
console.log( this.a );
}
var obj1 = {
a: 2,
foo: foo
};
var obj2 = {
a: 3,
foo: foo
};
obj1.foo(); // 2
obj2.foo(); // 3
obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后
就可以顺序应用下面这四条规则来判断 this 的绑定对象。

1. 由 new 调用？绑定到新创建的对象。
2. 由 call 或者 apply（或者 bind）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到 undefined，否则绑定到全局对象。

#### 3.绑定例外

1. 将null,undefined传入做绑定对象，这些值在调用是会忽略，实际应用默认绑定规则。

#### 4.对象

1. 内置对象：
    - String
    - Number
    - Boolean
    - Object
    - Function
    - Array
    - Date
    - RegExp
    - Error

注意：

```javascript
var strPrimitive = "I am a string";
typeof strPrimitive; // "string"
strPrimitive instanceof String; // false
var strObject = new String( "I am a string" );
typeof strObject; // "object"
strObject instanceof String; // true
// 检查 sub-type 对象
Object.prototype.toString.call( strObject ); // [object String]
```

strP属于文字形式，strO属于构造形式，在对strP进行操作时，如获取长度等等，引擎会自动将strP转换为object，进行处理。

null和undefined只有构造形式，而Date只有文字形式。

Object，Array，Function，RegExp无论以文字形式和构造形式创建都属于对象。

### 对象的深浅copy

浅复制，就是将源对象的值传递过去，如果出现引用，会将引用传递过去.
常见的json复制是：

```javascript
var newObj = JSON.parse( JSON.stringify( someObj ) );
```

在Es6中可以使用 Object.assign(..)实现浅copy

### 属性描述

`writable`: true, //可修改

`configurable`: true, //可配置

`enumerable`: true //可枚举

可以使用 defineProperty(..)修改属性描述

#### 不变性

1. 对象的属性使用 writable:false 和 configurable:false 就行。
2. 禁止扩展：

```javascript
var myObject = {
a:2
};
Object.preventExtensions( myObject );
myObject.b = 3;
myObject.b; // undefined
```

3. 密封 Object.seal(..)

实际上会在一个现有对象上调用

Object.preventExtensions(..) 并把所有现有属性标记为 configurable:false

4. 冻结 Object.freeze(..)

- 这个方法实际上会在一个现有对象上调用
Object.seal(..) 并把所有“数据访问”属性标记为 writable:false 

#### [[Get]]

myObject.a 在属性访问中,实际是实现了[[Get]]操作，会先在对象的属性中查找，没找到将会遍历可能存在的[[prototype]]链中查找
#### [[Put]]

当存在属性时：

1. 属性是否是访问描述符（参见 3.3.9 节）？如果是并且存在 setter 就调用 setter。
2. 属性的数据描述符中 writable 是否是 false ？如果是，在非严格模式下静默失败，在严格模式下抛出 TypeError 异常。
3. 如果都不是，将该值设置为属性的值。当不存在属性时：待补充。

### 混合对象“类”

#### 显式混入

```javascript
// 非常简单的 mixin(..) 例子 :
function mixin( sourceObj, targetObj ) {
  for (var key in sourceObj) {
    // 只会在不存在的情况下复制
    if (!(key in targetObj)) {
      targetObj[key] = sourceObj[key];
    }
  }
  return targetObj;
}
var Vehicle = {
  engines: 1,
  ignition: function() {
    console.log( "Turning on my engine." );
  },
  drive: function() {
    this.ignition();
    console.log( "Steering and moving forward!" );
  }
};
var Car = mixin( Vehicle, {
  wheels: 4,
  drive: function() {
    Vehicle.drive.call( this );
    console.log("Rolling on all " + this.wheels + " wheels!");
  }
});
```

由于混入这种方式，两个对象引用的是同一个函数，因此这种复制（或者说混入）实际上并不能完全模拟面向类的语言中的复制。

#### 寄身继承

```javascript
// “传统的 JavaScript 类”Vehicle
function Vehicle() {
  this.engines = 1;
}
Vehicle.prototype.ignition = function() {
  console.log( "Turning on my engine." );
};
Vehicle.prototype.drive = function() {
  this.ignition();
  console.log( "Steering and moving forward!" );
};
// “寄生类” Car
function Car() {
// 首先，car 是一个 Vehicle
var car = new Vehicle();
// 接着我们对 car 进行定制
car.wheels = 4;
// 保存到 Vehicle::drive() 的特殊引用
var vehDrive = car.drive;
// 重写 Vehicle::drive()
car.drive = function() {
  vehDrive.call( this );
  console.log("Rolling on all " + this.wheels + " wheels!");
  return car;
}
var myCar = new Car();
myCar.drive();
```

注意在 var car=new Car()时，会生成新对象并丢弃，所以使用时可以不用new。

#### 隐式混入

```javascript
var Something = {
  cool: function() {
    this.greeting = "Hello World";
    this.count = this.count ? this.count + 1 : 1;
  }
};
Something.cool();
Something.greeting; // "Hello World"
Something.count; // 1
var Another = {
  cool: function() {
  // 隐式把 Something 混入 Another
  Something.cool.call( this );
  }
};
Another.cool();
Another.greeting; // "Hello World"
Another.count; // 1 （count 不是共享状态）
```

## 原型[[Prototype]]

`[[Prototype]]`机制就是指对象中的一个内部链接引用另一个对象

如果在第一个对象上没有找到需要的属性或者方法引用，引擎就会继续在 `[[Prototype]]`
关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的
`[[Prototype]]` ，以此类推。这一系列对象的链接被称为“原型链”

```javascript
var anotherObject = {
  a:2
};
// 创建一个关联到 anotherObject 的对象
var myObject = Object.create( anotherObject );
console.log(myObject);// {}
myObject.a; // 2
```

myObject的属性是空的，但是其原型链中有a属性。

原型链的尽头是 Object.prototype。

如果使用myObject.a赋值，则会在myObject中添加a的属性，并赋值。

#### “类”

虽然这些 JavaScript 机制和传统面向类语言中的“类初始化”和“类继承”很相似，但是 JavaScript 中的机制有一个核心区别，那就是不会进行复制，对象之间是通过内部的[[Prototype]] 链关联的

#### 委托代替类

```javascript

Task = {
  setID: function(ID) { this.id = ID; },
  outputID: function() { console.log( this.id ); }
};
// 让 XYZ 委托 Task
XYZ = Object.create( Task );
XYZ.prepareTask = function(ID,Label) {
  this.setID( ID );
  this.label = Label;
};
XYZ.outputTaskDetails = function() {
  this.outputID();
  console.log( this.label );
};
```

举例：

```javascript
//es6写法
class Widget {
  constructor(width,height) {
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
}
render($where){
  if (this.$elem) {
    this.$elem.css( {width: this.width + "px",height: this.height + "px"} )
      .appendTo( $where );
    }
  }
}
class Button extends Widget {
  constructor(width,height,label) {
    super( width, height );
    this.label = label || "Default";
    this.$elem = $( "<button>" ).text( this.label );
}
render($where) {
  super( $where );
  this.$elem.click( this.onClick.bind( this ) );
}
onClick(evt) {
  console.log( "Button '" + this.label + "' clicked!" );
  }
}
```

```javascript
var Widget = {
  init: function(width,height){
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
},
insert: function($where){
  if (this.$elem) {
    this.$elem.css( {width: this.width + "px",height: this.height + "px"})
      .appendTo( $where );
  }
}
};
var Button = Object.create( Widget );
Button.setup = function(width,height,label){
    // 委托调用
    this.init( width, height );
    this.label = label || "Default";
    this.$elem = $( "<button>" ).text( this.label );
};
Button.build = function($where) {
  // 委托调用
  this.insert( $where );
  this.$elem.click( this.onClick.bind( this ) );
};
Button.onClick = function(evt) {
  console.log( "Button '" + this.label + "' clicked!" );
};
```
