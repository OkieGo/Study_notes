## 过滤器

@WebFilter()

### Filter的生命周期

| 生命周期阶段         | 执行时机         | 执行次数 |
| -------------------- | ---------------- | -------- |
| 创建对象             | Web应用启动时    | 一次     |
| 初始化  init()       | 创建对象后       | 一次     |
| 拦截请求  doFilter() | 接收到匹配的请求 | 多次     |
| 销毁  destory()      | Web应用卸载前    | 一次     |

### 过滤器链

- Filter链执行顺序是由web.xml中filter-mapping配置的顺序决定的。
- 如果是注解配置，则由全类名的顺序决定。

### 过滤器匹配规则

#### 1、精确匹配

```xml
<filter-mapping>
    <filter-name>Target01Filter</filter-name>
    <url-pattern>/Target01Servlet</url-pattern>
</filter-mapping>
```



#### 2、模糊匹配

*①前杠后星*

```xml
<filter-mapping>
    <filter-name>Target02Filter</filter-name>
    <url-pattern>/user/*</url-pattern>
</filter-mapping>
```

*②前星后缀*

```xml
<filter-mapping>
    <filter-name>Target04Filter</filter-name>
    <url-pattern>*.png</url-pattern>
</filter-mapping>
```

③**不允许**前杠后缀，星号在中间

```xml
<url-pattern>/*.png</url-pattern>   //错误示范
```



#### 3、根据Servlet名称匹配

```xml
<filter-mapping>
    <filter-name>Target05Filter</filter-name>
    <!-- 根据Servlet名称匹配 -->
    <servlet-name>Target01Servlet</servlet-name>
</filter-mapping>
```