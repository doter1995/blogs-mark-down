# 基础
1. spring-security是基于web Filter实现的
## 安装依赖
1. `build.gradle`文件下添加依赖
```
dependencies {
  implementation 'org.springframework.boot:spring-boot-starter-security'
}
```
2. spring-security的filter链
[4.2.4.RELEASE.doc](https://docs.spring.io/spring-security/site/docs/4.2.4.RELEASE/reference/html/ns-config.html#ns-custom-filters)
3. [参照-Spring Security做JWT认证和授权](https://www.jianshu.com/p/d5ce890c67f7)
开始我也是按照这个写法去做的，后来觉得不能扩展UserDetails。所以我将授权部分直接在controller中去做。鉴权部分做了部分修改。
```
@Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/register").permitAll() //将注册和登录放行，在controller处理
                .antMatchers("/login").permitAll()
                .antMatchers("/image/**").permitAll() //静态资源访问无需认证
                .antMatchers("/admin/**").hasAnyRole("ADMIN") //admin开头的请求，需要admin权限
                .antMatchers("/article/**").hasRole("USER") //需登陆才能访问的url
                .anyRequest().authenticated()  //默认其它的请求都需要认证，这里一定要添加
                .and()
                .csrf().disable()  //CRSF禁用，因为不使用session
                .sessionManagement().disable()  //禁用session
                .formLogin().disable() //禁用form登录
                .cors()  //支持跨域
                .and()   //添加header设置，支持跨域和ajax请求
                .headers().addHeaderWriter(new StaticHeadersWriter(Arrays.asList(
                new Header("Access-control-Allow-Origin", "*"),
                new Header("Access-Control-Expose-Headers", "Authorization"))))
                .and() //拦截OPTIONS请求，直接返回header
                .addFilterAfter(new OptionsRequestFilter(), CorsFilter.class)
                //此处鉴权
                .apply(new JwtLoginConfigurer<>()).tokenValidSuccessHandler(jwtRefreshSuccessHandler())
                .permissiveRequestUrls("/logout")
                .and()
                .logout().logoutUrl("/logout")   //默认就是"/logout"
                .addLogoutHandler(tokenClearLogoutHandler())  //logout时清除token
                .logoutSuccessHandler(new HttpStatusReturningLogoutSuccessHandler()) //logout成功后返回200
                .and()
                .sessionManagement().disable();

    }
```
余下代码不贴了，改天上源码。
