## 添加依赖
1. 在`build.gradle`中添加
```
dependencies {
  implementation 'org.springframework.boot:spring-boot-starter-mail'
}
```
## 添加配置
`application.yml`
```yml
spring:
  mail:
    host: smtp.qq.com #邮件发送服务器
    port: 587
    username: xxxxx@qq.com
    password: xxxxxxxxx
    test-connection: true #测试连接
    properties:
      mail:
        smtp:
          auth: true
          enable: true
```
## 使用
[spring-framework-mail](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html#mail)
```
@Service
public class EmailService {
  JavaMailSender sender;

  public EmailService(JavaMailSender sender) {
    this.sender = sender;
  }

  public void sendVerifyCode(String email,String code) throws MessagingException {
    MimeMessage message = sender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(message);
    helper.setFrom("doter1995@qq.com");
    helper.setTo(email);
    helper.setText("this is test code!");
    log.info("before send emil");
    sender.send(message);
    log.info("send emil");
  }

}
```
需要说明的是：
EmailService的构造函数传入`(JavaMailSender sender)`,这个会在该service实例化的时候，spring自动注入这个sender实例，而这个sender的构造会依赖于`org.springframework.boot.autoconfigure.mail`的`MailSenderPropertiesConfiguration`来读取配置文件中的配置。
