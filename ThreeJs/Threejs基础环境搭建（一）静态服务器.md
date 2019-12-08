由于后期threeJs需要使用各种外部资源，所以需要搭建服务器，nginx，apache，iis等都可以。
本人则使用nodeJs的koa搭建一个简单的服务器
开发工具使用VSCode

首先需要确保电脑上已经按装了node.js
# 一 ，创建一个文件夹 如：three
# 二，运行npm init 一直点回车构建package.json
# 三，安装依赖 在这里使用koa koa-static
```
cnpm i koa -s
cnpm i koa-static -s
```
# 四，建立index.js文件
```
const Koa = require('koa'); 
const app = new Koa(); 
app.use(require('koa-static')('.'));//.代表index.js所在目录，如需更换端口 自行修改。
app.listen(3000); //监听3000端口，如需更换端口 自行修改。
```
# 五, 开启服务器
如果使用VSCode：
 1. 可以使用VSCode打开three文件夹
 2. 点击左侧调试按钮

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3967890-c770940bd991fc63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 如上图点击右上按钮选择nodejs环境 ，默认情况下会配置好运行的index.js为执行文件
4. 按F5开启服务器
 5. 按Ctrl+Shift+F5重启服务器
6. 按Shift+F5 关闭服务器

