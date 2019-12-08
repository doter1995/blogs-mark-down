最近回顾半年其实并没有踏踏实实的静下心的学，尤其最近一个月，天天抱着手机玩游戏，感觉实在实在乏味。所以想从零使用webpack搭建一个开发环境。使用的环境是macos，vscode。
> 第一节 将会构建一个简单的页面
## 初始化git仓库。
在github等网站创建一个仓库，之后在本地克隆下来。
## 使用npm创建一个project
在项目文件夹路径下运行`git init`，根据提示完场创建。
## 创建一个简单的页面
新建`index.html`
```html
<!doctype html>
<html>
  <head>
    <title>起步</title>
    <script src="https://cdn.bootcss.com/lodash.js/4.17.11/lodash.min.js"></script>
  </head>
  <body>
    <script src="index.js"></script>
  </body>
</html>
```
引用`lodash`库，是为了后期webpack进行第三库打包的操作。
新建`index.js`
```javascript
function component() {
  var element = document.createElement('div');

   // Lodash（目前通过一个 script 脚本引入）对于执行这一行是必需的
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

   return element;
}
 document.body.appendChild(component());
```
> 至此，已经创建了一个简陋的网页开发环境。
## 小结
idea下打开`index.html`,右上角会有在浏览器中打开。
vscode下可以直接打开该目录，然后用浏览器打开html文件，可以看到效果。
but个人推荐使用vscode 安装Live Server插件。之后可以在html文件右击使用open with live server。

