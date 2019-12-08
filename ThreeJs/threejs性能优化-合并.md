这两天在做一个地震点展示，整个地震点大约两万条数据。
如果使用Points，整个全部展示出来大约7-8fps。
```
import {
  BufferAttribute,
  BufferGeometry,
  Points,
  PointsMaterial,
  TextureLoader
} from "three";
import {get3dPosition} from "./util";

export default class QuakeSource extends Points {
  constructor(lat: number, lng: number, radius: number = 200) {
    super();
    this.setGeometry(lat, lng, radius);
    this.setMaterial();
  }

  setGeometry(lng, lat, radius) {
    const v3 = get3dPosition(lng, lat, radius+1);
    this.geometry = new BufferGeometry();
    this.geometry.addAttribute("position", new BufferAttribute(new Float32Array([v3.x, v3.y, v3.z]), 3));
    //这里使用一个点一个ponit对象
  }

  setMaterial() {
    let texture = new TextureLoader().load("./public/images/circle.png");
    this.material = new PointsMaterial({color: 0xff0000,size:3,map:texture,transparent: true})
  }
};
```
接下来将点进行合并
```
import {
  BufferAttribute,
  BufferGeometry,
  Points,
  PointsMaterial,
  TextureLoader
} from "three";
import {get3dPosition} from "./util";

export default class QuakeSources extends Points {
  constructor(latlngs: Array<any>, radius: number = 200) {
    super();
    this.setGeometry(latlngs, radius);
    this.setMaterial();
  }

  setGeometry(latlngs: Array<any>, radius) {
    let positions = [];
    latlngs.forEach(([lat, lng]) => {
      const v3 = get3dPosition(lng, lat, radius + 1);
      positions.push(v3.x, v3.y, v3.z);
    });
    this.geometry = new BufferGeometry();
    this.geometry.addAttribute("position", new BufferAttribute(new Float32Array(positions), 3));
     //这里使用多点一个ponit对象
  }

  setMaterial() {
    let texture = new TextureLoader().load("./public/images/circle.png");
    this.material = new PointsMaterial({color: 0xff0000, size: 3, map: texture, transparent: true})
  }
};
```
我只是将地震每月的数据进行合并。即达到了60fps。
![image.png](https://upload-images.jianshu.io/upload_images/3967890-13a0c98e3a96d361.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/640)
