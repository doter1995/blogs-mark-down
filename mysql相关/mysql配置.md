# mysql配置

在linux下或者docker下安装完mysql常见的几个细节要注意。

`1.` mysql默认的字符集

`show variables like '%character%';`

查看字符集，可能需要修改为utf-8；

```text
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8
```

简单的修改mysql的字符集
方法1：

```SQL
set character_set_client  = utf8;
set character_set_connection  = utf8;
set character_set_database  = utf8;
set character_set_results  = utf8;
set character_set_server  = utf8;
set character_set_system  = utf8;
set collation_connection = utf8 ;
set collation_database = utf8 ;
set collation_server = utf8 ;
```

方法2：
修改配置文件my.ini

```text
default-character-set = utf8
character_set_server =  utf8
```

`2.` 密码及账户权限问题。

mysql默认的root只能本机登录。

使用
`use mysql;`
`select  User,authentication_string,Host from user;`
结果如下：

```text
+------+-----------------------+-----------+
| User | authentication_string | Host      |
+------+-----------------------+-----------+
| root |                       | localhost |
| root |                       | %         |
+------+-----------------------+-----------+
```

第一行root配置了loclhost为本地登录。

第二行root配置了%代表了任何地址都可访问。

> 正常情况下推荐root只使用本地%。然后为单独的数据库设置管理员，并为管理员配置制定登录的地址。以防止源码，或程序被逆向破解后密码泄露。

修改限制登录地址：

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' `

其中`root`为账户，`123456`为密码，@后面的`%`为地址限制。
