> 现在我们基本已经完成了webpack的搭建，接下来我们优化开发体验。

目前为止，我们代码需要自己去手动的格式化，或者使用vscode默认的配置去格式化代码。
##  使用eslint和git-commit前自动格式化代码
需要使用三个插件。
husky是一个git 操作的钩子，通常使用git add时对代码进行格式化。
lint-staged是只对本次提交时修改的文件进行处理。渐进式的。
eslint代码格式检查工具。
1.安装依赖`npm i -D husky lint-staged eslint prettier`;
2.添加配置`package.json`
```json
+  "husky": {
+    "hooks": {
+       "pre-commit": "lint-staged"
+     }
+  },
+  "lint-stage": {
+    "*.js": [
+      "eslint --fix",
+      "git add"
+   ]
+  }
```
3.配置添加eslint配置文件`.eslintrc`
```json
{
  "rules": {
    "prettier/prettier": 2
  },
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    },
  },
  "plugins": ["prettier"]
}
```
这里使用了prettier做了代码检查及格式化
## VScode使用pretter格式化代码
idea 推荐插件[prettier](https://plugins.jetbrains.com/plugin/10456-prettier)
vscode 推荐插件[prettier-vscode](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
vscode安装完该插件后，打开设置`"editor.formatOnSave":true"`
接下来按找个js文件试试保存下文件。

[prettier-option](https://prettier.io/docs/en/options.html)
[prettier-Configuration](https://prettier.io/docs/en/configuration.html)
## 引入balel-loader
1. 安装依赖`npm i -D babel-loader @babel/core @babel/preset-env`
其中`babel-loader`是webpack的loader,
而`@babel/core`是babel核心库，
`@babel/preset-env`则是允许使用新的es特性，最终会帮你转译为低版本浏览器支持的代码。
**PS：下一节我们会扩充babel识别react并转译。**
2.更新配置`webpack.common.js`
```javascript
....
module.exports = {
  ......
+  module: {
+   rules: [
+      {
+       test: /\.js$/,
+        exclude: /(node_modules)/,
+        use: 'babel-loader',
+      },
+    ],
+  }
  ......
```
3.添加babel配置`.babelrc`
```javascript
{
  "presets": ["@babel/env"]
}
```
以上是使用webpack4.x, babel-loader 8.x , babel 7.x。
**PS：babel在7.x对presets配置进行了更新，所以要注意对比版本。**
## 加速css开发
[webpack-sass-loader](https://www.webpackjs.com/loaders/sass-loader/)
[webpack-less-loader](https://www.webpackjs.com/loaders/less-loader/)

## 识别特殊文件后缀
webpack打包时更够识别特殊文件后缀。
例如:react的jsx
```
....
module.exports = {
    ......
+  resolve:{
      extensions: ['.js', '.jsx','.json']
    },
.....
```

> 截止现在我们成功的构建了还算完成的webpack打包流程。剩下的就是React需要的一些配置。

反过来说，其实上面的流程已经让我们有一个比较的好开发环境及配置了。接下来，你可以用它去添加你喜欢的库，并使用它去开发了。比如去添加Vue或者React。或者使用其他的库。

## 总结
为此我们总结一下目前webpack等等配置的具体思路。
1. 初始化git仓库(非必须)
2. 初始化npm创建项目
3. 添加webpack，配置入口，出口。
4. 使用`webpack-merge`为不同环境做不同配置。
4. 添加webpack插件`HtmlWebpackPlugin`，自动生成html。
5. 添加webpack插件`CleanWebpackPlugin`，自动清除构建前目录。
6. webpack开启`source-map`。
7. 使用webpack-dev-server，实现开启服务，并监听文件，并自动更新构建。
8. 配置babel-loader

剩下的便是可选的配置：
* css相关的loader。`css-loader style-loader (sass-loader|style-loader)`
建议阅读以下:
[webpack-loaders](https://www.webpackjs.com/loaders/)
[webpack-plugins](https://www.webpackjs.com/plugins/)


