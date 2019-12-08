> 接下来，我们将使用webpack server帮我们优化开发过程。

再开始之前，我们换需要补一下之前遗漏的内容。
## html-webpack-plugin自动生成index.html
目前我们的`index.html`，仅仅只是引用了我们导入的`buddle.js`,我们可以使用webpack的插件`html-webpack-plugin`帮我们生成html
[webapck-htmlwebpackplugin](https://www.webpackjs.com/guides/output-management/#%E8%AE%BE%E5%AE%9A-htmlwebpackplugin)
1. 安装依赖`npm i -d html-webpack-plugin`
2. 在`webpack.config.js`配置插件
```javascript
  const path = require("path");
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
+   plugins: [
+     new HtmlWebpackPlugin({
+       title: "hello word"
+     })
    ]
  }
```
这样我们就可以删除`index.html`,当打包时会在`./dist/`下生成新的`index.html`文件
## 自动清除
[webpack-清除dist](https://www.webpackjs.com/guides/output-management/#%E6%B8%85%E7%90%86-dist-%E6%96%87%E4%BB%B6%E5%A4%B9)
1.安装依赖``
2.更新`webpack.config.js`
```javascript
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
    plugins: [
+     new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
         title: "hello word"
       })
    ]
  };
```
这样我们每次构建时，会先删除./dist目录，然后重新生成
## 开启source map方便debug
webpack编译的时候回对代码做混淆压缩，这样出现bug的时候很难追踪代码
1.更新webpack.config.js
```javascript
const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: './src/index.js',
+  devtool: 'inline-source-map',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    },
    plugins: [
      new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
         title: "hello word"
       })
    ]
  };

```
> 好了，接下来开始正题
## 开启webpack-server
1. 安装依赖`npm i -d webpack-dev-server`
2. 更新`webpack.config.js`
```javascript
module.exports = {
  entry: './src/index.js',
  devtool: 'inline-source-map',
+ devServer: {
+   contentBase: './dist',
+   port:"3000"
+ },
 ......
```
这样我们就可以使用`npx webpack-dev-server --open`，来开启服务器端口为3000

但是好像还有问题，比如开发时我希望能看到具体源码，但是在打包发布时，我并不希望看到具体源码，所以我们再来优化。
## 使用不同环境不同的webpack配置。使用npm start
参考这个[webpack-merge](https://www.webpackjs.com/guides/production/#%E9%85%8D%E7%BD%AE)，不做赘述
