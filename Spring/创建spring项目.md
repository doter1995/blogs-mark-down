# 创建一个spring初始化空项目

[start.spring.io](https://start.spring.io/)
通过在线配置生成项目包，之后即可下载解压。

## 更新配置
当需要添加某些依赖。而且你也不确定是否兼容某个spring boot版本。
最简单的办法是，在线配置一份，加入对应依赖，然后打开对应的`build.gradle`文件，抄配置。

## 设置数据库
1. 在`build.gradle`添加jdbc依赖
```
dependencies {
  runtimeOnly 'mysql:mysql-connector-java'
}
```
2. 在`application.properties`添加数据源
```
spring.datasource.url=jdbc:mysql://localhost:3306/tablename
spring.datasource.username=root
spring.datasource.password=password
```
# 推荐部分插件

## flyway 数据库版本控制
1. 在`build.gradle`添加flyway包
```
dependencies {
  implementation 'org.flywaydb:flyway-core'
}
```
2.在resources/db/migration文件夹创建sql
* 注意创建的sql文件命名规范V1__XXXXX.sql(两个下划线)

3. 配置数据源，会在tablename自动创建flyway_schema_history表记录版本
spring.flyway.url=jdbc:mysql://localhost:3306/tablename
spring.flyway.user=root
spring.flyway.password=password

[flyway](https://flywaydb.org/getstarted/)


> 注意需要保证jdbc依赖在flyway依赖前。否则flyway可能不生效。

未完待更新。。。
