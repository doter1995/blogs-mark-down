> 前面webpack已经构建好了，接下我们配置下Reat以及Antd

## 加入React
1. 安装依赖`npm i -s react react-dom`
2. 更新index.js
```javascript
import React from 'react'
import ReactDOM from 'react-dom'

import App from './App'

var element = document.createElement('div')

// 设置id,为ReactDOM.render提供挂载点
element.id = 'app'
document.body.appendChild(element)

let render =()=>{
  ReactDOM.render(<App/>, element)
}
render();
```
3.创建`./src/App.jsx`
```jsx
import React, { Component } from 'react'

export default class App extends Component {
  render() {
    return (
      <div>
        hello word!!!
      </div>
    )
  }
}
```
4.上一节我说了`.jsx`属于特别后缀，所以我们需要配置
```
resolve:{
    extensions: ['.js', '.jsx','.json']
}
```
不知道在哪配置吗？去上一节看去。
5. 接下来babel并不识别react的代码呀，所以我们需要配置babel的presets
在`.babelrc`问价配置
```
{
  "presets": ["@babel/env","@babel/react"]
}
```
等等，配置了我们没有这个库呀，所以接下来
6.安装`@babel/react`的依赖库`npm i -d @babel/preset-react`
好了，我们成功的配置好了react。
## 加入antd
[antd-按需加载](https://ant.design/docs/react/introduce-cn#%E6%8C%89%E9%9C%80%E5%8A%A0%E8%BD%BD)
注意审查下你的loader是否添加了css-loader等配置。
```javascript
module.exports = {
   ......
   module: {
      rules: [
        {
          test: [/\.css$/, /\.scss$/],
          use: ['style-loader', 'css-loader', 'sass-loader'],
        },
        {
          test: /\.js|jsx$/,
          exclude: /(node_modules|bower_components)/,
          use: 'babel-loader',
        },
      ],
    },
  .....
}
```
同时不要为css设置exclude，否则可能无法记载antd的样式。

> 至此，该教程已经基本完成。如果有时间，下次再写如何搭建react react-route redux的开发环境。
代码库：[源码-建议查看webpack分支](https://github.com/doter1995/game-start)
