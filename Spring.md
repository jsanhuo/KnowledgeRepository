[TOC]



# Spring	

## 基础

### 一，Spring的核心模块

- Core：提供Spring的基本功能。Spring用bean来管理组件，使用BeanFactory来产生和管理Bean。它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。
- Context：Spring上下文是一个配置文件，向Spring框架提供上下文信息。
- AOP：通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring框架中。所以，可以很容易地使 Spring框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。
- DAO：JDBC、DAO的抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理，和不同数据库供应商所抛出的错误信息。异常层次结构简化了错误处理，并且极大的降低了需要编写的代码数量，比如打开和关闭链接 。Spring jdbctemplate（jdbc模板编程）
- ORM：Spring框架和其他ORM框架的整合
- Web：Web上下文模块
- Web MVC：Model+View+Controller 模式，使得系统之间耦合降低

### 二，SpringBean的生命周期

1．Spring对bean进行实例化； 

2．Spring将值和bean的引用注入到bean对应的属性中； 

3．如果bean实现了BeanNameAware接口，Spring将bean的ID传递给 setBean-Name()方法； 

4．如果bean实现了BeanFactoryAware接口，Spring将调 用setBeanFactory()方法，将BeanFactory容器实例传入； 

5．如果bean实现了ApplicationContextAware接口，Spring将调 用setApplicationContext()方法，将bean所在的应用上下文的 引用传入进来； 

6．如果bean实现了BeanPostProcessor接口，Spring将调用它们 的post-ProcessBeforeInitialization()方法； 

7．如果bean实现了InitializingBean接口，Spring将调用它们的 after-PropertiesSet()方法。类似地，如果bean使用initmethod声明了初始化方法，该方法也会被调用； 

8．如果bean实现了BeanPostProcessor接口，Spring将调用它们 的post-ProcessAfterInitialization()方法； 

9．此时，bean已经准备就绪，可以被应用程序使用了，它们将一直 驻留在应用上下文中，直到该应用上下文被销毁； 

10．如果bean实现了DisposableBean接口，Spring将调用它的 destroy()接口方法。同样，如果bean使用destroy-method声明 了销毁方法，该方法也会被调用。

### 三，SpringAOP的使用

1. 注解型

   在Spring的配置文件中加入

   ```xml
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   ```

   或者在配置类上加入注解

   ```java
   //启动aspectJ代理，识别注解
   @EnableAspectJAutoProxy
   ```

   然后在切面类中加入注解

   ```java
   @Aspect
   @Component
   public class aspect {
       //定义切点
       @Pointcut("execution(public * cn.chenmixuexi.service.impl.*.queryUserById(..))")
       public void point1(){}
   
       /*前置通知*/
       @Before("point1()")
       public void logBefore(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logBefore => begin call " + name);
       }
       /*后置通知*/
       @After("point1()")
       public void logAfter(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logAfter => over call " + name);
       }
       /*后置返回通知*/
       @AfterReturning(value = "point1()",returning = "retval")
       public void logAfterReturning(JoinPoint joinPoint, User retval) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logAfterReturning => ret val:" + retval);
       }
       /*后置异常通知*/
       @AfterThrowing("point1()")
       public void logAfterThrowing(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println(name + "方法抛出异常了");
       }
       /*
       环绕通知
        */
       @Around("point1()")
       public Object logAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           String name = proceedingJoinPoint.getSignature().getName();
           System.out.println("logAroundBefore => begin call " + name);
           // 手动调用连接点的方法（目标方法）
           Object object = proceedingJoinPoint.proceed();
           System.out.println("logAroundAfter => over call " + name);
           return object;
       }
   }
   
   ```

2. XML配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:p="http://www.springframework.org/schema/p" xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
       <bean name="logAdvice" class="cn.chenmixuexi.aop.LogAdvice"></bean>
       <aop:config>
           <!--定义logAdvice这个bean是一个切面类-->
           <aop:aspect ref="logAdvice">
               <!--切面 = 切点 + 一组通知-->
               <aop:pointcut id="pointcut01"
                             expression="execution(public * cn.chenmixuexi.service.impl.*.queryUserById(..))"></aop:pointcut>
               <!--定义了一个aop的前置通知-->
               <aop:before method="logBefore" pointcut-ref="pointcut01"></aop:before>
               <aop:after method="logAfter" pointcut-ref="pointcut01"></aop:after>
               <aop:after-returning method="logAfterReturning" returning="retval" pointcut-ref="pointcut01"></aop:after-returning>
               <aop:after-throwing method="logAfterThrowing" pointcut-ref="pointcut01"></aop:after-throwing>
               <!--<aop:around method="logAround" pointcut-ref="pointcut01"></aop:around>-->
           </aop:aspect>
       </aop:config>
   </beans>
   ```

   切点类

   ```java
   public class LogAdvice {
       /*前置通知*/
       public void logBefore(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logBefore => begin call " + name);
       }
       /*后置通知*/
       public void logAfter(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logAfter => over call " + name);
       }
       /*后置返回通知*/
       public void logAfterReturning(JoinPoint joinPoint, User retval) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println("logAfterReturning => ret val:" + retval);
       }
       /*后置异常通知*/
       public void logAfterThrowing(JoinPoint joinPoint) throws Throwable {
           String name = joinPoint.getSignature().getName();
           System.out.println(name + "方法抛出异常了");
       }
       /*
       环绕通知
        */
       public Object logAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           String name = proceedingJoinPoint.getSignature().getName();
           System.out.println("logAroundBefore => begin call " + name);
           // 手动调用连接点的方法（目标方法）
           Object object = proceedingJoinPoint.proceed();
           System.out.println("logAroundAfter => over call " + name);
           return object;
       }
   }
   ```

### 四，Spring框架如何处理事务

Spring本身不提供事务处理能力，只是给所有的底层事务处理框架，提供统一的编程接口，让在Spring框架上进行事务处理的代码保持规范统一。

通过AOP进行应用：前置通知开启事务，后置通知提交事务，异常通知回滚。

Spring提供DataSourceTransactionManager

### 五，Spring支持的事务类型

- **编程式事务管理：**这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。
- **声明式事务管理：**这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。

### 六，Spring AOP的实现原理

通过代理模式，将各个时期运行的代码织入到原功能的前后。

### 六，SpringAplicationContext的初始化

- AnnotationConfigApplicationContext：从一个或多个Config类中加载Spring应用上下文
- AnnotationConfigWebApplicationContext：从一个或 多个基于Config类中加载Spring Web应用上下文
- ClassPathXmlApplicationContext：从类路径下的一个或 多个XML配置文件中加载上下文
- FileSystemXmlapplicationcontext：从文件系统下的一 个或多个XML配置文件中加载上下文 
- XmlWebApplicationContext：从Web应用下的一个或多个 XML配置文件中加载上下文

### 七，Spring容器都有哪些

- bean工厂，提供基本的DI支持
- 应用上下文，基于BeanFactory构建，提供框架级别的服务

## SpringMVC

### 一，请求是怎么派发的



### 二，前端控制器怎么分发请求到不同的Controller（action方法）上

HandlerMapping(处理器映射)和HandlerAdapter（处理器适配）

### 三，Controller怎么寻找View视图

viewResolver 视图解析器

### 四，请求递交到Controller的action方法中数据怎么传递，怎么获取？

只需要给action方法添加相应的形参变量

### 五，怎么传递数据到View视图进行页面渲染？

给action方法添加一个Model参数，这个Model相当于Map表，addAttribute添加数据，作用域：一次请求响应的作用域当中

### 六，异常处理

### 七，文件长传和下载