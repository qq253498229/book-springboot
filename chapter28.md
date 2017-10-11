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

默认 的安全配置是在`SecurityAutoConfiguration`以及它导入的类中实现的\(包括WEB安全`SpringBootWebSecurityConfiguration`，认证配置`AuthenticationManagerConfiguration`，这些都同样适用于非WEB应用\)。如果你想完全关闭默认的WEB安全配置，你可以使用`@EnableWebSecurity`添加一个bean\(这不会关闭认证管理配置和Actuator安全配置\)。如果你想定制这些配置，你可以使用外部属性或者是继承`WebSecurityConfigurerAdapter`的类\(比如你想添加一个基于表单的登录\)。

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| 如果你既添加了`@EnableWebSecurity`，又禁止了actuator安全，那么整个应用都会默认基于表单登录，除非你配置了一个继承于`WebSecurityConfigurerAdapter`的类。 |

想要关闭认证管理器的话，可以实现`AuthenticationManager`；或者是将`AuthenticationManagerBuilder`注入在一个方法里，然后将这个类加上`@Configuration`注解，这样就配置了一个全局的`AuthenticationManager`。在Spring示例项目中（[Spring Boot samples](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples/)）有几个例子可以让您快速上手。

在Web应用中，您可以获得这些基本的功能：

* AuthenticationManager，它可以通过内存的方式存储单个用户（请查看SecurityProperties.User这个类中的几个属性）
* 您可以忽略常见静态资源的不安全路径（比如`/css/**` ，`/js/**`，`/images/**`，`/webjars/**` 和`**/favicon.ico` 等）
* 所有HTTP请求的基本安全
* 默认情况下，Spring Security的低级别安全选项都是打开的\(包括HSTS, XSS, CSRF, caching\)

上面 所有的配置你都可以通过外部的配置文件\(前缀security.\*\)来打开、关闭或修改它们。如果你想覆盖访问规则而不改变其它默认的功能，可以继承`WebSecurityConfigurerAdapter`，并在类上加上`@Order(SecurityProperties.ACCESS_OVERRIDE_ORDER)`,之后就可以配置它们来实现您自己的需求了。

| ![](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/images/note.png "\[Note\]") |
| :--- |
| 默认情况，`WebSecurityConfigurerAdapter`会匹配所有路径，如果你不想匹配所有的路径的话，必须在`WebSecurityConfigurerAdapter`类中显式的配置您需要匹配的路径。 |



