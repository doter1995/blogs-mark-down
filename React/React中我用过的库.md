#web
## UI库
  *  [antd](https://ant.design/docs/react/introduce-cn) 

## 富文本插件
  * [draft-js](https://draftjs.org/)

  * [react-draft-wysiwyg](https://jpuri.github.io/react-draft-wysiwyg/#/)
      这个库的的使用：[例子源码](https://github.com/jpuri/react-draft-wysiwyg/blob/master/docs/src/components/Demo/index.js)。
有个小问题关于样式导入
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3967890-36559c287c4c4d79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* [quill.js](https://quilljs.com/)

## 地图
  * [react-leaflet](https://github.com/PaulLeCam/react-leaflet)

## 图表
* [d3](https://github.com/d3/d3/wiki)
* [recharts](http://recharts.org/#/zh-CN) 基于D3的react库

## 3d
* [threejs](https://threejs.org/)

## 文件导出
 * [js-export-excel](https://github.com/cuikangjie/js-export-excel)

## 音乐谱获取
* [AnalyserNode](https://developer.mozilla.org/zh-CN/docs/Web/API/AnalyserNode)
# node.js
图片本地压缩：
* [imagemin](https://github.com/imagemin/imagemin)
```
const imagemin = require("imagemin");
const imageminMozjpeg = require("imagemin-mozjpeg");
const imageminPngquant = require("imagemin-pngquant");
const imageminGifsicle = require("imagemin-gifsicle");
const makeDir = require('make-dir');
const Path = require("path");
const {promisify} = require('util');
const fs = require('fs');

const writeFile = promisify(fs.writeFile);

module.exports = async function(path, name) {
  await imagemin([`${path}/${name}`], {
    plugins: [
      imageminMozjpeg({
        quality: 80,
      }),
      imageminPngquant({
        quality: [0.6, 0.8],
      }),
      imageminGifsicle(),
    ],
  }).then(data =>
    data.map(async re => {
      await makeDir(Path.dirname(re.sourcePath));
      await writeFile(Path.join(Path.dirname(re.sourcePath),Path.basename(re.sourcePath).replace(/\./,".min.")), re.data);
      return re;
    }),
  );
};

```
# 验证码生成
* svg-captcha

持续不定时更新，欢迎大家推荐比较好的库。
