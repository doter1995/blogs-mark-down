9021年了，现在重学spring感觉比较晚了，但是好多概念要么忘了，要么没学过。现在只好重头过一遍。9021年了，直接看注解模式，xml估计也没人用了。
### IOC(DI:dependency injection)控制反转(依赖注入)
核心的就是bean和bean容器，怎么说？
通过各种配置来去定义bean如何实例化，最终bean容器通过你的配置帮你实例化bean。

举例：
现在需要调用A的getName方法
正常使用：
```java
Class A{
  public String getName(){
    return "AAA";
  }
}
.....
A a = new A();
a.getName();
```
spring下呢？
```
@Bean
Class A{
  public String getName(){
    return "AAA";
  }
}
@Autowired
A a;
a.getName();
```
如上，将A注册为Bean，当看到@Autowired时，bean容器会帮你实例化A。
### 注解下Bean的加载过程。
AnnotationConfigApplicationContext也是一个 BeanDefinitionRegistry。

调用ClassPathBeanDefinitionScanner.doScan或者AnnotatedBeanDefinitionReader.register去解析

中将注解的Bean统一的由BeanDefinitionRegistry(也就是AnnotationConfigApplicationContext)管理。

最终会调用getBean去实例化。


