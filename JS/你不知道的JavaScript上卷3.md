### 原型[[Prototype]]
 [[Prototype]] 机制就是指对象中的一个内部链接引用另一个对象

如果在第一个对象上没有找到需要的属性或者方法引用，引擎就会继续在 [[Prototype]]
关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的
[[Prototype]] ，以此类推。这一系列对象的链接被称为“原型链”

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
