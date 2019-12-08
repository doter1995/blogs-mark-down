#### 1.声明问题
如下例子：
```
foo(a)
var a=1;
console.log(a);// undefined
function foo(a){
    console.log(a);//1
}
```
执行前编译：会将所有声明提前
```
var a;
function foo(a){
    console.log(a);
}
```
最后执行：
```
foo(a);
a=1;
console.log(a)
```
再来一个有趣的例子：
```js
foo(a)
var a=1;
console.log(a);
var foo = function(a){
    console.log(a);
}
```
报错：foo is not a function
原因是foo的声明提前了，但是赋值在后面。foo()执行的时候，其实是undefined
>es6中的let 不会声明提前
```js
foo(a)//error: a is not defined
let a=1;
console.log(a);
var foo = function(a){
    console.log(a);
}
```
#### 2.闭包
```
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
```
for (var i=1; i<=5; i++) {
  setTimeout( function timer() {
    console.log( i );
  }, i*1000 );
}
```
结果： 6 6 6 6 6 
原因是timer()中的i是引用，而真实数据在for循环后为6
```
for (var i=1; i<=5; i++) {
    (function(j) {
        setTimeout( function timer() {
                console.log( j );
        }, j*1000 );
    })( i );
}
```
结果：1 2 3 4 5
```
// let 为es6
for (let i=1; i<=5; i++) {
    setTimeout( function timer() {
        console.log( i );
    }, i*1000 );
}
```
结果：1 2 3 4 5
#### 3.模块
```
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
