## MVC-Servelt优化（粗略的springmvc实现）



中央控制器类：`DispatcherServlet`



**`DispatcherServlet`** 主要做两件事：

1. 根据URL获取到对应的Controller和方法。

2. 调用Controller中的方法。
   - 获取参数
     - `paramters`存放参数签名信息：通过getParamters()方法获取参数签名信息
     - `paramterValues`存放参数值： 根据参数签名，通过request.getParatmter()获取
   - 执行方法
     - 反射执行
   - 响应结果