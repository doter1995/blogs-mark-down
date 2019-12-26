# 中卷

## 类型语法

### 1.类型

#### 1.1内置对象

- 空值（ null ）
- 未定义（ undefined ）
- 布尔值（ boolean ）
- 数字（ number ）
- 字符串（ string ）
- 对象（ object ）
- 符号（ symbol )，ES6 中新增
- `typeof  null == 'object' //true`

```javascript
typeof function a(){ /* .. */ } === "function"; // true
typeof [1,2,3] === "object"; // true
```

 function,数组实际上是 object 的一个“子类型”

```javascript
function a(b,c) {
/* .. */
}
a.length// 2 
```

函数对象的 length 属性是其声明的参数的个数

* JavaScript 中的变量是没有类型的，只有值才有。变量可以随时持有任何类型的值

```javscript
var a;
console.log(a)//undefined
console.log(b)//error: b is not defined
```

* NaN not a number ,typeof 仍然是"number"
* NaN != NaN 为 true
* JSON.stringify(-0) 返回 "0" ，而 JSON.parse("-0") 返回 -0 。

#### 1.2值与引用

* 简单值（即标量基本类型值，scalar primitive）总是通过值复制的方式来赋值 / 传递，包括
null 、 undefined 、字符串、数字、布尔和 ES6 中的 symbol 。
* 复合值（compound value）——对象（包括数组和封装对象，参见第 3 章）和函数，则总

是通过引用复制的方式来赋值 / 传递。

```javascript
function foo(x) {
  x.push( 4 );
  x; // [1,2,3,4]
  // 然后
  x = [4,5,6];
  x.push( 7 );
  x; // [4,5,6,7]
}
var a = [1,2,3];
foo( a );
a; // 是[1,2,3,4]，不是[4,5,6,7]
```

函数传递 a 的时候，实际是将引用 a 的一个复本赋值给 x ，而 a 仍然指向 [1,2,3] 。
在函数中我们可以通过引用 x 来更改数组的值（ push(4) 之后变为 [1,2,3,4] ）。但 x =
[4,5,6] 并不影响 a 的指向，所以 a 仍然指向 [1,2,3,4] 

##### 假值

- undefined
- null
- false
- +0 、 -0 和 NaN
- ""

```javascript
a || b;
// 大致相当于(roughly equivalent to):
a ? a : b;
a && b;
// 大致相当于(roughly equivalent to):
a ? b : a;
```
