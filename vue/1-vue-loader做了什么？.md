首先基于webpack的打包构建，所以暂不考虑parcel。
webpack处理.vue文件配置
```
module: {
    rules: [
      {
        test: /\.vue$/,
        loader: "vue-loader",
        options: vueLoaderConfig,
      },
      {
        test: /\.js$/,
        loader: "babel-loader",
        include: [
          resolve("src"),
          resolve("test"),
          resolve("node_modules/webpack-dev-server/client"),
        ],
      },
      ....
}
```
> 在探究vue-loader之前，请先阅读该文章：[深入Webpack-编写Loader](https://segmentfault.com/a/1190000012718374)

### vue-loader
```javascript
const descriptor = parse({
    source,
    filename,
    sourceRoot,
    needMap: sourceMap
  })
```
