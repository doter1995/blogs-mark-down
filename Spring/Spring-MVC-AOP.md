##
> 在Spring Boot中，已经集成了AOP，所以不需要再做额外配置，可以直接使用。

[Spring2.0 AOP文档](https://docs.spring.io/spring/docs/2.0.x/reference/aop.html#d0e7028)
[Spring5.1 AOP文档](https://docs.spring.io/spring/docs/5.1.x/spring-framework-reference/core.html#aop-ataspectj)
以后整理简单的用法。

## 用法
很有用的一个东西,比如：

现在有一个FeignClient，当发请求时，需要记录请求的request和response。
可以考虑使用AOP去处理。
在你使用FeignClient发请求的业务代码中就不需要做额外的记录处理。

