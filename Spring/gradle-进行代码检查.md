[gradle code analysis](https://docs.gradle.org/current/userguide/plugin_reference.html#code_analysis)

## java 使用 Checkstyle
>主要检查代码缩进，等等问题

[Checkstyle 文档](https://docs.gradle.org/current/userguide/checkstyle_plugin.html#sec:checkstyle_usage)

1. 安装依赖`build.gradle`

```gradle
plugins {
  id 'checkstyle'
}
```

这里直接使用的插件，所以会自动安装依赖包。

2. 添加配置文件

安装下列结构新建配置文件

```
<root>
└── config
    └── checkstyle           // (1)
        └── checkstyle.xml   // (2)
        └── suppressions.xml
```

3. 写入配置
   [文档](http://checkstyle.sourceforge.net/config.html)
   简单点，打开[google,sun 两种配置文件](https://github.com/checkstyle/checkstyle/tree/master/src/main/resources)
   随便选一个复制就行。

> 到这里简单的配置已经完成了。
> 运行`gradle check`，就会在`build/reports/checkstyle/`下生成报告。

看到 xml 的报告。是不是很不爽？可以在`build.gradle`配置下。

```
tasks.withType(Checkstyle) {
    reports {
        xml.enabled false
        html.enabled true
        html.stylesheet resources.text.fromFile('config/xsl/checkstyle-custom.xsl')
    }
}
```
运行下来发现好多警告，怎么办？
[checkstyle-idea/](https://plugins.jetbrains.com/plugin/1065-checkstyle-idea/)
为idea安装这个插件，这样就可以帮你实时报告了。
接着我们设置idea的自动格式化代码风格。
打开`preferences/Edit/codestyle:`
![image.png](https://upload-images.jianshu.io/upload_images/3967890-04aff107ca19422d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意选择idea code style xml
[idea-java-google-style.xml](https://raw.githubusercontent.com/google/styleguide/gh-pages/intellij-java-google-style.xml)
导入上面的文件，然后就可以愉快的编写代码了。

1. 自动生成javadoc
[idea-javadoc](https://plugins.jetbrains.com/plugin/7157-javadoc)插件
mac下：option+shift+g 生成方法注释（选中方法名）
具体在代码编辑下按command+n查看

## java 使用 spotbugs

> 主要找一些代码bugs，比如import顺序有误，使用getset方法，get返回了对象可能会修改对象等。

> 因为 findbugs 已经 n 年没有更新了，gradle 也将会移除 findbugs。所以直接用推荐的 spotbugs。

1. 安装依赖`build.gradle`

```gradle
plugins {
  id "com.github.spotbugs" version "1.7.1"
}
```

2. 配置项选用默认。所以不作处理

   [spotbugs 的配置和 FindBugsExtension 配置一样](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.quality.FindBugsExtension.html)

3. 配置 report,这个不配置默认生成 xml

```
tasks.withType(com.github.spotbugs.SpotBugsTask) {
	reports {
		xml.enabled false
		html.enabled true
	}
}
```
