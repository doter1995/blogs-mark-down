## 重新踩坑记录。
### 按照简单的实现。
1. 创建mysql
`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.6`

2. 当我创建javaApp容器时，先是链接不上mysql。
找到原因时代码中配置的数据库地址是localhost。
当在容器运行时，localhost指向了容器。
> 解决方案：
*  修改数据库链接地址主机为mysql。
* 在本地hosts文件中加入 127.0.0.1   mysql。用来解决本地开发链接msyql。
* docker创建一个网络：xiaodi
`docker network  create xiaodi`
* 重新将之前创建的mysql容器加入到这个网络。并设置mysql在xiaodi网络中的别名为msyql。
`docker  network connect xiaodi --alias mysql f3209d0f12d6 `
* 运行javaApp容器时加入到xiaodi网络。
`docker run --name xiaodi-server -p8080:8080 --net=xiaodi -d xiaodi:0.1`
> 之前使用--net=host，容器使用宿主机网络，发现端口不能映射了。所以8080访问不通。
