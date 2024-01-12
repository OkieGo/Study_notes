# 注解配置SpringMVC

使用注解类代替Web.xml和SpringMVC.xml配置文件

## 初始化类代替Web.xml

再Servlet3.0环境中容器会在类路径中查找实现javax.servlet.ServletContainerInitializer接口的类，如果找到就用它来配置Servlet容器。

Spring提供了这个接口的实现：SpringServletContainerInitializer，这个类反过来又会查找实现WebApplicationInitializer的类并将配置的任务交给它们来完成。

WebApplicationInitializer的实现:`AbstractAnnotationConfigDispatcherServletInitializer`当我们的类继承了这个长类并将其部署到Servlet3.0容器的时候，容器自动发现它，并用它来配置Servlet上下文。 

```java
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    //设置Spring的配置类
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    //设置SpringMVC的配置类
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }

    //设置DispatcherServlet的<url-pattern>
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //设置过滤器
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceEncoding(true);
        return new Filter[]{characterEncodingFilter};
    }
}
```

## WebConfig类代替SpringMVC.xml

```java
//扫描组件、mvc注解驱动、默认Servlet、拦截器、
// 视图控制器、视图解析器、文件上传解析器、异常解析器

@Configuration//表明配置类
@ComponentScan({"com.why.controller"})//1.扫描组件
@EnableWebMvc//2.开启mvc注解驱动
public class WebConfig extends WebMvcConfigurationSupport {

    //3.默认Servlet处理静态资源
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    //4.配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        HandlerInterceptor firstInterceptor = new FirstInterceptor();
        registry.addInterceptor(firstInterceptor).addPathPatterns("/").excludePathPatterns("hello");
    }

    //5.视图控制器
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("/index.html");
    }

    //6.视图解析器
    @Bean
    public InternalResourceViewResolver internalResourceViewResolver(InternalResourceViewResolver viewResolver){
        viewResolver.setPrefix("/Html/");
        viewResolver.setSuffix(".html");
        return viewResolver;
    }

    //7.文件上传解析器
    @Bean
    public CommonsMultipartResolver multipartResolver(){
        CommonsMultipartResolver resolver = new CommonsMultipartResolver();
        resolver.setDefaultEncoding("UTF-8");
        return resolver;
    }

    //8.xml形式的异常解析器2种写法
    @Override
    protected void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers) {
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        exceptionResolver.setExceptionMappings((Properties) new Properties().setProperty("java.lang.NullPointerException","/index.html"));
        exceptionResolver.setExceptionAttribute("e");
        exceptionResolvers.add(exceptionResolver);
    }
    @Bean
    public SimpleMappingExceptionResolver simpleMappingExceptionResolver(){
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        exceptionResolver.setExceptionMappings((Properties) new Properties().setProperty("java.lang.NullPointerException","/index.html"));
        exceptionResolver.setExceptionAttribute("e");
        return exceptionResolver;
    }

}
```