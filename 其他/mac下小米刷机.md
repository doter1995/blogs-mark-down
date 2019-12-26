# mac下小米刷机

> 想把旧手机红米note3升级下系统，以前刷过其他三方的rom，最后刷了个非官方的miui8版本，结果一堆广告。后来安装官网教程，发现要线刷，需要下载软件，只支持windows。。。

## 准备工作

安装adb

`brew cask install android-platform-tools`

安装好后配置一下环境变量。

在/etc/profile中加入。

`export PATH=~/android-sdks/platform-tools:~/android-sdks/tools:$PATH`

运行`fastboot --version`检测是否成功安装并配置好环境。

下载线刷镜像：[各版本线刷包地址](http://www.miui.com/shuaji-393.html)

下载好后解压

## 开始刷机

1. 打开手机调试模式，并手机连接到电脑
2. 进入fastboot模式：
手机已开机使用：`adb reboot bootloader`
没有开机：开机键+音量下长按
3. 查看当前手机设备名：
`fastboot devices`
![如图](https://upload-images.jianshu.io/upload_images/3967890-85065766e4645fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
11b39c8f 为当前设备名
4. 控制台进入到解压后的目录：(如上图)
5. 运行脚本：

`sh ./flash_all.sh -s 11b39c8f`

刷机完成

> 关于其他两个sh脚本，一个是加锁，一个是保留用户数据。
