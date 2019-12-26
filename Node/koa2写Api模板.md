
# 模版

```javascript

var koa = require('koa');
var router = require('koa-router')();  //路由
var app =new koa();
var index = require('./router/index')
var user = require('./router/user')
const logger = require('koa-logger'); //日志
var bodyparser=require('koa-bodyparser')(); //request参数挂载
var cors = require('koa2-cors');  //跨域支持
var session = require("koa-session")  //session支持
var json = require("koa-json")  //返回结果json化
//session 配置
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

app.use(index.routes(),index.allowedMethods());  //公共api
app.use(async(ctx,next)=>{  //登陆检验
  if(ctx.session.islogin){
    await next();
  }else{
    ctx.body={state:-4,tip:"请登录"}
  }
})
app.use(user.routes(),user.allowedMethods());

app.use( ()=>{
  this.body = '请求无法处理';
});

app.listen(3001);

```
