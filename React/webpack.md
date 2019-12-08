## 最基础的核心实现
如下为源码
```
import a from "./a.js";
function component() {
  a();
  console.log("aa");
}

document.body.appendChild(component());
/****a.js****/
export default function() {
  var B = 100;
  function println() {
    console.log(B);
  }
}

```
如下为编译后的结果：
```js
"./src/a.js": function(module, __webpack_exports__, __webpack_require__) {
    "use strict";
    __webpack_require__.r(__webpack_exports__);
    __webpack_exports__["default"] = function() {
      var B = 100;
      function println() {
        console.log(B);
      }
    };
  },

  "./src/index.js": function(module, __webpack_exports__, __webpack_require__) {
    "use strict";
    __webpack_require__.r(__webpack_exports__);
    var _a_js__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__("./src/a.js");

    function component() {
      Object(_a_js__WEBPACK_IMPORTED_MODULE_0__["default"])();
      console.log("aa");
    }

    document.body.appendChild(component());
  }
});
```
通过闭包将进行模块。```__webpack_require__("./src/a.js")```。
所以webpack中的import 和require之类的，都是webpack的实现。

## 关于循环依赖问题处理
参考：[阮一峰-avaScript 模块的循环加载]([http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html](http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)
)
