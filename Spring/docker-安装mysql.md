
[docker mysql ](https://hub.docker.com/_/mysql)
## 下载镜像

`docker pull mysql:5.6`
制定5.6版本

## 运行镜像
`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.6`
1. my-secret-pw为root密码
2. some-mysql为实例的名称
3. 本地开发暴露端口 -p 3306:3306 这样在本地即可访问mysql
`docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.6`

## 访问数据库
`docker exec -it some-mysql bash`
进入some-mysql实例的shell中。
`mysql -u root -p`
输入密码进入
