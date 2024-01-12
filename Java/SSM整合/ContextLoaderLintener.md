## ContextLoaderListener

Spring提供了监听器ContextLoaderListener，实现ServletContextListener接口，可监听

ServletContext的状态，在web服务器的启动，读取Spring的配置文件，创建Spring的IOC容器。web

应用中必须在web.xml中配置

```xml
<listener>
    <!--
        配置Spring的监听器，在服务器启动时加载Spring的配置文件
        Spring配置文件默认位置和名称：/WEB-INF/applicationContext.xml
        可通过上下文参数自定义Spring配置文件的位置和名称
    -->
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!--自定义Spring配置文件的位置和名称-->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring.xml</param-value>
</context-param>
```
