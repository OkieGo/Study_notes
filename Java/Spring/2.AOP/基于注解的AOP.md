## 初步实现

### ①创建切面类

- 前置通知
- 目标操作
- 返回通知或异常通知
- 后置通知

```java
// @Aspect表示这个类是一个切面类
@Aspect
// @Component注解保证这个切面类能够放入IOC容器
@Component
public class LogAspect {
    
    //重用切入点表达式，同一个类中直接用"PointCut()"方法名引用,不同类用"全类名+方法名()"引用
    @Pointcut("execution(* com.atguigu.*.*(..)))")
    public void PointCut() {}
        
    // value属性：指定切入点表达式，由切入点表达式控制当前通知方法要作用在哪一个目标方法上
    @Before(value = "execution(public int com.atguigu.Calculator.add(int,int))")
    public void printLogBeforeCore() {
        System.out.println("[AOP前置通知] 方法开始了");
    }
    
    @AfterReturning(value = "PointCut()")
    public void printLogAfterSuccess() {
        System.out.println("[AOP返回通知] 方法成功返回了");
    }
    
    @AfterThrowing(value = "com.atguigu.spring.aop.test.declarPointCut()")
    public void printLogAfterException() {
        System.out.println("[AOP异常通知] 方法抛异常了");
    }
    
    @After(value = "execution(public int com.atguigu.add(int,int))")
    public void printLogFinallyEnd() {
        System.out.println("[AOP后置通知] 方法最终结束了");
    }
    
}
```
- 环绕通知

```java
@Around(value = "com.atguigu.aop.aspect.AtguiguPointCut.transactionPointCut()")
public Object manageTransaction(ProceedingJoinPoint joinPoint) {
    
    Object[] args = joinPoint.getArgs();
    Signature signature = joinPoint.getSignature();
    String methodName = signature.getName();
    
    Object ReturnValue = null;
    
    try {
        System.out.println("[AOP 前置通知]");

        //proceed()方法不传入参数会用原本的参数，传入args代表出入修改过的参数
        ReturnValue = joinPoint.proceed();
        ReturnValue = joinPoint.proceed(args);
        
        System.out.println("[AOP 返回通知]");
        
    }catch (Throwable e){ 
        System.out.println("[AOP 异常通知]);
    }finally {
        System.out.println("[AOP 后置通知]);
    }
    return ReturnValue;
}
```

### ②加入到IOC容器，开启注解AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns=...>
    
    <!-- 开启基于注解的AOP功能 -->
    <aop:aspectj-autoproxy/>
    
    <!-- 配置自动扫描的包 -->
    <context:component-scan base-package="com.atguigu.aop"/>
    
</beans>
```

## 获取连接点信息

### JoinPoint接口

- `JoinPoint.getSignature()`获取目标方法签名对象`Signature` 

- `Signature.getName()`获取方法名
- `JoinPoint.getArgs()`获取实参数组

```java
@Before(value = "execution(public int com.atguigu.dd(int,int))")
public void printLogBeforeCore(JoinPoint joinPoint) {
    Signature signature = joinPoint.getSignature();
    String methodName = signature.getName();
    Object[] args = joinPoint.getArgs();
}
```

### 连接点方法返回值

```java
@AfterReturning(value = "pointCut()",returning = "ReturnValue")
public void printLogAfterCoreSuccess(JoinPoint joinPoint, Object ReturnValue) {
    
    String methodName = joinPoint.getSignature().getName();
    
    System.out.println("[AOP返回通知] "+methodName+"方法返回值是：" + ReturnValue);
}
```

### 连接点方法抛出异常

```java
@AfterReturning(value = "pointCut()",throwing = "exception")
public void printLogAfterCoreSuccess(JoinPoint joinPoint, Throwable  exception) {
    
    String methodName = joinPoint.getSignature().getName();
    
    System.out.println("[AOP返回通知] "+methodName+"方法抛出异常是：" + exception);
}
```

## 切面优先级

![image-20221106202809575](D:\Documents\Java\Spring\assets\切面优先级.png)
    
```java
@Component
@Aspect
@Order(1)
public class LogAspect {
	....
}
```

