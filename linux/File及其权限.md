# File及其权限

## 文件权限

```text
drwxr-xr-x@  4 userA  staff   128B 12  8 22:09 fileName
```

### 操作权限(rwx)

* r 可读(4)
* w 可写(2)
* x 可执行(1)

* `7 代表rwx`
* `4 代表r--`

### 操作命令

#### 修改文件权限

* chgrp
* chown
* chmod

#### 操作目录

* ls: 列出目录
* cd：切换目录
* pwd：显示目前的目录
* mkdir：创建一个新的目录
* rmdir：删除一个空的目录
* cp: 复制文件或目录
* rm: 移除文件或目录

#### 查看文件

* cat
* tac
* nl
* more
* less
* head
* tail
* od
* file 查看文件类型

#### 修改文件时间

* touch

#### 查找文件

* whereis 在目录中查找文件
* locate 根据数据库去查找文件名
* find 根据硬盘中查找

#### 查看命令

* which 在PATH中查找
* type  在PATH中查找同时在内建命令中查找

#### 压缩与解压

* gzip
* tar
