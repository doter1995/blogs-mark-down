
# koa2 width Egg

## koa2

![image.png](https://upload-images.jianshu.io/upload_images/3967890-8161ad746e60b8fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如上图，每一个请求都会一层层穿过中间件，最终在某个中间件处结束(不再继续调用下边的中间件)，然后一层层返回。
如果需要写一个服务，我们需要自己写具体的中间件去处理逻辑，而在前边可以使用中间件，帮助实现

```javascript
const SESSION_Config = {
    key:'koa:sessssss',
    maxAge:86400000,
    overwrite: true, /** (boolean) can overwrite or not (default true) */
    httpOnly: true, /** (boolean) httpOnly or not (default true) */
    signed: true, /** (boolean) signed or not (default true) */
}
app.keys = ['mykoa'];

app.use(cors({credentials:true}));
app.use(session(SESSION_Config, app));
app.use(async(ctx,next)=>{                    //session 处理
  if(!ctx.session.islogin){
    ctx.session.islogin=false;
  }
  await next();
});

app.use(json());
app.use(logger());
app.use(bodyparser);
//如上代码，我使用cors，session，json，logger，bodyparser
//这些中间件帮我现实了跨域，session，json化，日志，请求体解析。

app.use(index.routes(),index.allowedMethods());  //公共api
app.use(async(ctx,next)=>{  //登陆检验
  if(ctx.session.islogin){
    await next();
  }else{
    ctx.body={state:-4,tip:"请登录"}
  }
})
app.use(user.routes(),user.allowedMethods());
//以上我只需要实现路由中间具体逻辑处理即可
```

如果学过spring的，可能想到那么controller和service呢？对不起，没有。

使用koa的话，controller及以后的需要自己实现。或者找比较好的中间件。

## Egg.js为企业级框架和应用而生

首先egg是在koa上的封装。

那koa没有controller，service，router，所以egg加入了这些，并且约定了文件目录结构，等等，这样的话，更利于工程化的开发。
