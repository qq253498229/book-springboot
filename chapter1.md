## 28. Security

在应用中加入Spring Security，它默认以'basic'的认证方式对所有所有的http请求进行安全认证。如果想对应用进行方法级的保护，那么请在配置类上加上`@EnableGlobalMethodSecurity`，更多信息请在这里[Spring Security Reference](https://docs.spring.io/spring-security/site/docs/5.0.0.BUILD-SNAPSHOT/reference/htmlsingle#jc-method) 找到。

权限管理器\(`AuthenticationManager`\)有一个默认的用户\(用户名是\`user\`，密码在开启服务器的时候以INFO级别的日志显示在控制台\)，就像这样：

```
Using default security password: 78fa095d-3f4c-48b1-ad50-e24c31d5cf35
```

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| 请确保控制台可以显示INFO级别的日志，否则密码你可能会找不到密码。 |



