## 28. Security

在应用中加入Spring Security，它默认以'basic'的认证方式对所有所有的http请求进行安全认证。如果想对应用进行方法级的保护，那么请在配置类上加上`@EnableGlobalMethodSecurity`，更多信息请在这里[Spring Security Reference](https://docs.spring.io/spring-security/site/docs/5.0.0.BUILD-SNAPSHOT/reference/htmlsingle#jc-method) 找到。

权限管理器\(`AuthenticationManager`\)有一个默认的用户\(用户名是\`user\`，密码在开启服务器的时候以INFO级别的日志显示在控制台\)，就像这样：

```
Using default security password: 78fa095d-3f4c-48b1-ad50-e24c31d5cf35
```

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| 请确保控制台可以显示INFO级别的日志，否则密码你可能会找不到密码。 |



如果你想改变默认密码请配置`security.user.password`，其它配置可以在这里查看[`SecurityProperties`](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-autoconfigure/src/main/java/org/springframework/boot/autoconfigure/security/SecurityProperties.java)（它们都是以\`security\`作为前缀）。

默认 的安全配置类需要实现`SecurityAutoConfiguration`，

The default security configuration is implemented in`SecurityAutoConfiguration`and in the classes imported from there \(`SpringBootWebSecurityConfiguration`for web security and`AuthenticationManagerConfiguration`for authentication configuration which is also relevant in non-web applications\). To switch off the default web application security configuration completely you can add a bean with`@EnableWebSecurity`\(this does not disable the authentication manager configuration or Actuator’s security\). To customize it you normally use external properties and beans of type`WebSecurityConfigurerAdapter`\(e.g. to add form-based login\).

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| If you add`@EnableWebSecurity`and also disable Actuator security, you will get the default form-based login for the entire application unless you add a custom`WebSecurityConfigurerAdapter`. |

To also switch off the authentication manager configuration you can add a bean of type`AuthenticationManager`, or else configure the global`AuthenticationManager`by autowiring an`AuthenticationManagerBuilder`into a method in one of your`@Configuration`classes. There are several secure applications in the[Spring Boot samples](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples/)to get you started with common use cases.

The basic features you get out of the box in a web application are:

* An
  `AuthenticationManager`
  bean with in-memory store and a single user \(see
  `SecurityProperties.User`
  for the properties of the user\).
* Ignored \(insecure\) paths for common static resource locations \(
  `/css/**`
  ,
  `/js/**`
  ,
  `/images/**`
  ,
  `/webjars/**`
  and
  `**/favicon.ico`
  \).
* HTTP Basic security for all other endpoints.
* Security events published to Spring’s
  `ApplicationEventPublisher`
  \(successful and unsuccessful authentication and access denied\).
* Common low-level features \(HSTS, XSS, CSRF, caching\) provided by Spring Security are on by default.

All of the above can be switched on and off or modified using external properties \(`security.*`\). To override the access rules without changing any other auto-configured features add a`@Bean`of type`WebSecurityConfigurerAdapter`with`@Order(SecurityProperties.ACCESS_OVERRIDE_ORDER)`and configure it to meet your needs.

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| By default, a`WebSecurityConfigurerAdapter`will match any path. If you don’t want to completely override Spring Boot’s auto-configured access rules, your adapter must explicitly configure the paths that you do want to override. |



