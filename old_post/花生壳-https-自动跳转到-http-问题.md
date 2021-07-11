;---
# 花生壳 https 自动跳转到 http  问题
;date: 2019-02-19 15:36:33
;tags:
;---

最近在花生壳上买了个 带 https 服务的一个域名，由于我启动的服务是 springmvc 框架下建立的工程，在 tomcat 中运行，访问主页的时候因为使用的是 / 自动 redirect:/index 会把 https 请求直接转发到 http 请求上，而 google 认为这样的访问是不安全的，因此不显示任何内容。后来搜索了一下，发现了一个解决方案，代码如下：

```java
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
  <property name="prefix" value="/" />
  <property name="suffix" value=".ftl" />
  <property name="redirectHttp10Compatible" value="false" />
</bean>
```

将 `redirectHttp10Compatible` 设置为 `false` 就可以实现 https 之间的跳转了。至于为什么会是这样，看了一下 spring 的官方论坛，貌似也没有给出确切的原因。

另外，从 http 跳转到 https 需要用其它方式解决，网上看了一下  基本上是用 nigix 做反向代理去解决的。并且使用 nigix 代理的话，也不会出现上述问题



> 参考
>
> https://stackoverflow.com/questions/3401113/spring-mvc-redirect-prefix-always-redirects-to-http-how-do-i-make-it-stay
>
> https://blog.csdn.net/zhuye1992/article/details/80496151
>
> http://forum.spring.io/forum/spring-projects/security/105279-https-switches-to-http-after-redirect
