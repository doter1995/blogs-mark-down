> 上面我们创建了一个简单的html。接下来我们使用webpack进行打包。

[webpack教程-起步](https://www.webpackjs.com/guides/getting-started/)

## 安装webpack
这里推荐使用[cnpm](https://npm.taobao.org/)
毕竟国内cnpm快了不只一点。
`npm install webpack webpack-cli --save-dev`
## git忽略掉/node_modules/
创建`.gitignore`
```file
/node_modules/
```
## 配置webpack
1.创建src目录
2.移动`index.js`到`./src/index.js`
3.创建`webpack.config.js`
```javascript
const path = require("path");
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```
根据配置webpack会将`./src/index.js`进行打包，并输出为`./dist/bundle.js`
所以我们需要将html中引用的index.js替换为`./dist/bundle.js`

##  更新index.html
```html
- <script src="index.js"></script>
+ <script src="dist/bundle.js"></script>
```
> 现在我们可以使用`npx webpack`去打包`bundle.js`从而不在直接引用`index.js`

## 更新lodash
但是目前loadsh依然是在页面引入。所以我们需要更新。
运行 `npm install loadsh -s`，安装依赖。
更新`./src/index.js`,第一行加入`import _ from 'lodash'`
```
+ import _ from 'lodash'
  function component() {	
    var element = document.createElement('div');
     // Lodash替换为import
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');

    return element;
  }
```
删除`index.html`引用的loadsh
> 至此我们成功的将代码进行了打包。

But现在开发流程是:
1. 修改js
2. 运行`npx webpack`
3. 浏览器打开html或者刷新页面

这开发体验，估计很多人会炸，我也是。so下一节我们开启webpack server让它帮我们做2和3步骤。

