# E6的新特性
#### let const
let是为了限制变量的作用域。
```javascript
for(let aa=0;aa<7;aa++){
console.log(aa);
}
console.log(aa);
//1,2,3,4,5,6,undefind
for(var aa=0;aa<7;aa++){
console.log(aa);
}
console.log(aa);
//1,2,3,4,5,6,7
```
const是声明常量，为了防止变量发生变化。
> 注意观察下面的代码

```javascript
const a="a";
const b={
  a:"A",
  b:"B",
  c:"C'
}
a=1;//fail
b={}; //fail
b.a="AA"; //pass
b["c"]="C";//pass 
b={}//fail
```
由此可见const只是限制了value的值变化。如果其值为引用，则不会限制其引用值得变化。
> const和let一样，也会限制其作用域。

![控制台操作结果](https://upload-images.jianshu.io/upload_images/3967890-5eae91d8ffd47b72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 字符串模板``

非常好用的一个特性。

```javascript
let name="doter";
let a = `hello word,${name}!`;
console.log(a);
//hello word,doter
```
> 在常见的大量字符串变量拼接时，特别方便。在webGL写glsl的脚本时特别方便。
#### 解构
```javascript
//对象解构
let node={a:1,b:2,c:3};
let {a,b,c} = node
console.log(a,b,c);//1 2 3

//数组解构
let arr=[1,2,3];
let {d,e,f} = node
console.log(a,b,c);//1 2 3
console.log(d,e,f);//1 2 3
```
### 可变参数
```javascript
function getNode(a,...node){
console.log(node);
}
getNode(1,2,3,4);//2 3 4
getNode(a=1,b=2,c=3);//2 3
```
#### 展开运算符...
```javascript
let node={a:1,b:2,c:3};
console.log({...node});//{a:1,b:2,c:3}

//接下来很有趣的,当解构遇上展开
let {...node1} = node
console.log(node1);//{a:1,b:2,c:3}
//node1将会得到一个新的对象，以后就可以用这个做浅复制了。

node1.a= 2；
console.log(node1);//{a:2,b:2,c:3}

let {a, b, ...node2} = node
console.log(node2);//{c:3}
//
```
> 感觉不可思议吧，其实这个应该放在前面的可变参数中理解。解构的时候，右边的值解构后全部传入左侧的{},而左侧{}将参数依次按照可变参数处理。
貌似和展开符没有半毛钱关系。是了，为了加深记忆。


同理对数组也可作展开这里不做代码演示。

what？浅复制。是了，因为此操作只是展开了第一层。
```javascript
let node = {a:{a:1,ab:3},b:4};
let {...node1} = node;//浅复制
node1.a.ab=4;
console.log(node);//{a: {a: 1, ab: 4},b: 4}
```
看见了吧node.a.ab也发生了变化。所以只是浅复制。这里是个坑哦。
顺便提一下，常见的深复制操作：
```javascript
let node = {a:{a:1,ab:3},b:4};
//>>>>>>>>>>>>>>>>>>>>>>>
var node2 = JSON.parse(JSON.stringify(node));
//<<<<<<<<<<<<<<<<<<<<<<
node2.a.ab=4;
console.log(node.a.ab);//3
console.log(node2.a.ab);//4
```
#### 对象字面量简写法
```javascript
let a = 4;
let node = {
  a,
  b:1,
  c(){console.log("a");}
}
console.log(node);//{a:4,b:1,c:f}
//很简单吧？
```
# 未完待续
## for of 与原来的for in区别
```javascript
let arr=[1,2,3,4,5,6,7,8];
for(let i =0;i<arr.length;i++){
console.log(i);
}
for(let index in arr){
console.log(arr[index]);
}
for(let value of arr){
console.log(value);
}
//都可以打印出来
```
## map
## Promise
## 箭头函数=>
# ES7新特性
#### **
#### [].includes
#### async/await
