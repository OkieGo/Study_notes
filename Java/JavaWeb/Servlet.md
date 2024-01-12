## Servlet

**Servlet 虚拟路径的匹配优先级顺序为：全路径匹配（精确匹配）> 目录匹配/* > 扩展名匹配*.do > 缺省匹配（默认匹配）。**

@WebServlet("/hello")

### Servlet、ServletConfig、HttpServlet

![./images](D:\Documents\Java\JavaWeb\assets\img025.6ea85941.png)

### ServletContext接口

- 代表：整个Web应用
- 是否单例：是
- 典型的功能：
  - 获取某个资源的真实路径：getRealPath()
  - 获取整个Web应用级别的初始化参数：getInitParameter()
  - 作为Web应用范围的域对象
    - 存入数据：setAttribute()
    - 取出数据：getAttribute()

#### [#](http://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter07/verse04.html#_1-配置web应用级别的初始化参数)[1]配置Web应用级别的初始化参数

```xml
    <!-- 配置Web应用的初始化参数 -->
    <context-param>
        <param-name>handsomeMan</param-name>
        <param-value>alsoMe</param-value>
    </context-param>
```

## Servlet生命周期

| 名称       | 时机                                                         | 次数 |
| ---------- | ------------------------------------------------------------ | ---- |
| 创建对象   | 默认情况：接收到第一次请求 修改启动顺序后：Web应用启动过程中 | 一次 |
| 初始化操作 | 创建对象之后                                                 | 一次 |
| 处理请求   | 接收到请求                                                   | 多次 |
| 销毁操作   | Web应用卸载之前                                              | 一次 |

### Servlet容器功能

容器会管理内部对象的整个生命周期。对象在容器中才能够正常的工作，得到来自容器的全方位的支持。

- 创建对象
- 初始化
- 工作
- 清理

### 典型Servlet容器产品举例

- Tomcat
- jetty
- jboss
- Weblogic
- WebSphere
- glassfish

## Request、Response