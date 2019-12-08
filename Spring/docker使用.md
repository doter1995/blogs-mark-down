# 注册
[docker hub](https://hub.docker.com/)
注册一个比较好，必要时可以分享你的镜像。

## docker 简单操作
> 首先理解镜像(image)和容器(container)
### docker指令操作分三类：
1. 账户即[docker hub](https://hub.docker.com/)。
2. image相关操作。pull/push/rmi
3. container操作
### docker 网络常见问题
1. container使用宿主机网络
`docker run ** --net=host ** `
2. 使用docker network
创建加入到对应网络。
## docker compose可以直接配置网络，自动创建网络。
推荐直接使用compose。
## 查看日志：
`docker logs -f --tail=10 f3209d0f12d6`
