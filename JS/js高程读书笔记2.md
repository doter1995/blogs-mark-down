# 第一章 面向对象的程序设计
js的对象定义：无序属性的集合，其属性可以包含基本值、对象或者函数。
## 1.创建对象
虽然字面量创建单个对象很方便，但是在创建多个相似对象就比较麻烦。
### 1.1工厂模式
``` javascript
function createPerson(name, age, job){
  var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function(){
    alert(this.name);
  };
  return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
不能解决对象识别的问题
### 1.2 构造函数模式
``` javascript
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function(){
    alert(this.name);
  };
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```
注意之前说过js函数本质是对象。所以以上代码解析是：
1. 当new Person()时，先创建一个function的对象。
2. 执行方法，为对象添加属性。
3. 将对象赋值给person1变量
优点是解决上面问题，缺点是：就是每个方法都要在每个实例上重新创建一遍。
那么可以将公用方法提取到外部
``` javascript
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = sayName;
}
function sayName(){
  alert(this.name);
}
```
但是如果对象需要定义很多方法，那么就要定义很多个全局函数，映像代码封装
### 1.3 原型模式
``` javascript
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
  alert(this.name);
};
var person1 = new Person();
person1.sayName(); //"Nicholas"
```
原型中查找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上 反映出来.
