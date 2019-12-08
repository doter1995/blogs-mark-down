## 安装
安装推荐使用docker安装
[jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins)
安推荐装这个。之前安装的[jenkins](https://hub.docker.com/_/jenkins)由于版本较低，安装插件报错。
所以下拉取[jenkins/jenkins](https://hub.docker.com/r/jenkins/jenkins)的镜像。

1. 安装
`docker pull jenkins/jenkins:latest`
`docker run --name xiaodijenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins/jenkins:latest`
注意：命令行执行后会打印初始化密码，可以直接拷贝。
2. 浏览器打开http://localhost:8080，输入密码。
## 初始化，安装必要插件以及全局工具。
1. 插件安装
直接安装推荐的插件。
2. 全局工具配置 `/configureTools/`
1.在线安装jdk，比较坑的点 
![例如jdk设置安装](https://upload-images.jianshu.io/upload_images/3967890-483261b6212e305e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如上：`/descriptorByName/hudson.tools.JDKInstaller/enterCredential`
![image.png](https://upload-images.jianshu.io/upload_images/3967890-9d353bd824fe3f65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
换需要配置一个Oracle账号。不然安装失败。
ps:由于使用的是docker的jenkins镜像，我们可以直接使用该镜像的自带openjdk。当然如果你需要特定版本的jdk的时候，就考虑这种情况需要的版本多不多，少的话，建议自己安装，然后配置下，多的话建议配置账户吧。
![gradle配置](https://upload-images.jianshu.io/upload_images/3967890-eaadc56e01e19fdf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置别名选择在线安装或者配置，
推荐直接选择在线安装(docker下跑的jenkins，省的去配置各种环境) 
### 写脚本 两种方式(Declarative和Scripted)
Declarative是新的，社区推荐。以后应该是主流。
如下：最简单的使用tools
```jenkinsfile
pipeline {
    agent any
    tools {
        gradle '5.2.1'
    }
    stages {
        stage('gradle') {
            steps {
                sh 'gradle build'
            }
        }
    }
}
````
### 拉取代码
