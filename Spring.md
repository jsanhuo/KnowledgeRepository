[TOC]



# Spring	

## 基础

####  Spring介绍：

 Spring诞生：创建Spring的目的就是用来代替更加重量级的企业技术JAVA,

 简化java开发：

   基于POJO轻量级和最小侵入式开发

   通过依赖注入和念想接口实现松耦合

   基于切面和惯例进行声明式编程

   通过切面和模板减少样板式代码

#### IOC控制反转

​    Spring的核心思想之一：Inversion of  Control,控制反转

​    控制反转什么意思？对象的创建交给外部容器完成，这个叫做控制反转

​         1、Spring使用控制反转来实现对象不用在程序中写死

​         2、控制反转解决对象处理问题[把对象交给别人来创建]

​    对象之间的依赖关系Spring值如何做的？

​         依赖注入，Di   **dependency injection.**Spring使用依赖注入来实现对象之间的依赖关系，创建完对象后，对象的关系处理，就叫依赖注入。

#### Spring 容器对象  IOC容器

​      Spring容器不单单只有一个，可以归为两种类型：

 1、Bean工厂，BeanFactory

 2、应用上下文，ApplicaitonContext[功能强大]

#### 通过Resource获取BeanFactory

  1、加载Spring配置文件

  2、通过XMLBeanFactory+配置文件来创建IOC容器

```java
 //加载Spring的资源文件
        Resource resource = new ClassPathResource("applicationContext.xml");

​        //创建IOC容器对象【IOC容器=工厂类+applicationContext.xml】
​        BeanFactory beanFactory = new XmlBeanFactory(resource); 
```



#### 类路径下XML获取ApplicationContext

​      直接通过ClassPathXmlApplicationContext对象来获取

```java
  // 得到IOC容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");

        System.out.println(ac);
```

####    在Spring中总体可以通过三中方式来获取配置对象

​       1、使用XML文件配置

​       2、使用java注解来配置

​       3、使用javaConfig来配置

####  XML配置方式

​     上面已经获得ioc容器，接下来了。接下来就是在applicationContext.xml文件中配置信息【让IOC容器根据applicationContext.xml文件来创建对象】

   首先先有个JavaBean

```java
public class User {

    private String id;
    private String username;


    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

 以前我们是通过new User的方法来创建对象的

 现在我们有了IOC容器，可以让IOC容器帮我们创建对象了。在applicationContext.xml文件中配置对应的信息就行了

```java
   <!--
        使用bean节点来创建对象
            id属性标识着对象
            name属性代表着要创建对象的类全名
        -->
    <bean id="user" class="User"/>
```

​        通过IOC容器对象获取对象，在外界通过IOC对象得到User对象

```java
   // 得到IOC容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");

        User user = (User) ac.getBean("user");

        System.out.println(user);
```

   上面我们使用的是IOC通过无参构造函数来创建对象，我们来回顾一下一般有几种创建对象的方式：

- 无参构造函数创建对象

- 带参数的构造函数创建对象

- 工厂创建对象

- - 静态方法创建对象
  - 非静态方法创建对象

使用无参的构造函数创建对象我们已经会了，接下来我们看看使用剩下的IOC容器是怎么创建对象的。

#### 带参数的构造函数创建对象

 首先，JAVABean就要提供带参构造函数

```java
public User(String id, String username) {
        this.id = id;
        this.username = username;
    }
```

接下来，就是配置applicationContext.xml文件了

```java
<bean id="user" class="User">

        <!--通过constructor这个节点来指定构造函数的参数类型、名称、第几个-->
        <constructor-arg index="0" name="id" type="java.lang.String" value="1"></constructor-arg>
        <constructor-arg index="1" name="username" type="java.lang.String" value="zhongfucheng"></constructor-arg>
    </bean>
```

 在constructor上**如果构造函数的值是一个对象，而不是一个普通类型的值，我们就需要用到ref属性了，而不是value属性**

比如说：**我在User对象上维护了Person对象的值，想要在构造函数中初始化它**。因此，就需要用到ref属性了

```java
<bean id="person" class="Person"></bean> 

    <bean id="user" class="User" >

        <!--通过constructor这个节点来指定构造函数的参数类型、名称、第几个-->
        <constructor-arg index="0" name="id" type="java.lang.String" value="1"></constructor-arg>
        <constructor-arg index="1" name="username" type="java.lang.String" ref="person"></constructor-arg>
    </bean>
```

#### 工厂静态方法创建对象

 首先，使用一个工厂的静态方法返回一个对象

```java
public class Factory {

    public static User getBean() {

        return new User();
    }

}
```

在配置文件中使用工厂的静态方法返回对象

```
   <!--工厂静态方法创建对象，直接使用class指向静态类，指定静态方法就行了-->
    <bean id="user" class="Factory" factory-method="getBean" >

    </bean>
```

#### 工厂非静态方法创建对象

```java
public class Factory {


    public User getBean() {

        return new User();
    }


```

配置文件中使用工厂的非静态方法返回对象

```
  <!--首先创建工厂对象-->
    <bean id="factory" class="Factory"/>

    <!--指定工厂对象和工厂方法-->
    <bean id="user" class="User" factory-bean="factory" factory-method="getBean"/>
```

#### 注解方式

   **通过注解来配置信息就是为了简化IOC容器的配置，注解可以把对象添加到IOC容 器中、处理对象依赖关系**   ，我们来看看怎么用吧

 1）首先引入context名称空间

​        xmlns:context="http://www.springframework.org/schema/context"

2）开启注解扫描

​       <context:component-scan base-package=""></context:component-scan>

​      第二种方法：可以通过自定义扫描类@CompoentScan修饰来扫描IOC容器的bean对象。。如下代码：

```java
//表明该类是配置类
@Configuration

//启动扫描器，扫描bb包下的
    //也可以指定多个基础包
    //也可以指定类型
@ComponentScan("bb")
public class AnnotationScan {

}
```

在使用ComponentScan（）这个注解，在测试类需要加上@ContextConfiguration这个注解来加载配置类…

 创建对象以及对象之间的依赖关系，相关的注解

```java
@ComponentScan扫描器
@Configuration表明该类是配置类
@Component   指定把一个对象加入IOC容器--->@Name也可以实现相同的效果【一般少用】
@Repository   作用同@Component； 在持久层使用
@Service      作用同@Component； 在业务逻辑层使用
@Controller    作用同@Component； 在控制层使用
@Resource  依赖关系
如果@Resource不指定值，那么就根据类型来找，相同的类型在IOC容器中不能有两个
如果@Resource指定了值，那就根据名字来找
```

  测试代码；

  UserDao

```java
package aa;

import org.springframework.stereotype.Repository;

/**
 * Created by ozc on 2017/5/10.
 */

//把对象添加到容器中,首字母会小写
@Repository
public class UserDao {

    public void save() {
        System.out.println("DB:保存用户");
    }


}
```

userService

```Java
package aa;


import org.springframework.stereotype.Service;

import javax.annotation.Resource;


//把UserService对象添加到IOC容器中,首字母会小写
@Service
public class UserService {

    //如果@Resource不指定值，那么就根据类型来找--->UserDao....当然了，IOC容器不能有两个UserDao类型的对象
    //@Resource

    //如果指定了值，那么Spring就在IOC容器找有没有id为userDao的对象。
    @Resource(name = "userDao")
    private UserDao userDao;

    public void save() {
        userDao.save();
    }
}
```

userAction 

```java
package aa;

import org.springframework.stereotype.Controller;

import javax.annotation.Resource;

/**
 * Created by ozc on 2017/5/10.
 */

//把对象添加到IOC容器中,首字母会小写
@Controller
public class UserAction {

    @Resource(name = "userService")
    private UserService userService;

    public String execute() {
        userService.save();
        return null;
    }
}
```

#### Scope属性

指定scope属性，IOC容器就知道创建对象的时候是单利还是多利了。

属性值只有两个：单利/多利

**当我们使用singleton【单例】的时候，从IOC容器获取的对象都是同一个**

**当我们使用prototype【多例】的时候，从IOC容器获取的对象都是不同的**

#### lazy-init 属性

​    只对singleton单利的对象有效 -----默认为false

​     有时候，我们想要对象在使用的时候才创建，我们将lazy-init设置为true

#### init-method和destory-method

 如果我们想要对象创建后，执行某个方法，我们指定为init-method.

 如果我们想要ioc容器销毁后，执行某个方法，我们执行destory-method.

#### Bean创建细节总结：

  1)对象创建：单例/多例

​     scope="singleton",默认值，即默认是单例。

​     scope="prototype",多例;

   2)什么时候创建？

​        scope="singleton"，在启动(容器初始化之前)，就已经创建了bean,而且只有一个。

​        scope="prototype"，在用到对象的时候，才创建对象。

  3)是否延迟加载

​        lazy-init="false",默认为false,不延迟加载，即在启动的时候创建对象。

​        lazy-init-"true",延迟初始化，在用到对象的时候才创建。(只对单利有效)。

  4)创建对象之后，初始化/销毁

​       init-method="init_user"  [对应对象的init_user方法，在对象创建之后就能]

​       destory-method="destory_user" [在调用容器对象的destory方法]

### 一，Spring的核心模块

- Core：提供Spring的基本功能。Spring用bean来管理组件，使用BeanFactory来产生和管理Bean。它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。<u>IOC容器，解决对象创建之间的依赖关系</u>
- Context：Spring上下文是一个配置文件，向Spring框架提供上下文信息。
- AOP：通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring框架中。所以，可以很容易地使 Spring框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。
- DAO：JDBC、DAO的抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理，和不同数据库供应商所抛出的错误信息。异常层次结构简化了错误处理，并且极大的降低了需要编写的代码数量，比如打开和关闭链接 。Spring jdbctemplate（jdbc模板编程） <u>Spring 对jdbc操作的支持  【JdbcTemplate模板工具类】</u>
- ORM：Spring框架和其他ORM框架的整合
- Web：Web上下文模块  <u>Spring对Web模块的支持，
- ​     可以与Struts结合，让Struts的Action创建交给Spring
- ​      Spring MVC</u>
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

### 八，Spring配置文件中，配置的对象有哪些？

1. InternalResourceViewReslover	视图解析器
2. 处理器映射
3. 处理器适配器
4. bean对象
5. SpringMVC所需要的bean对象
6. 应用程序用的bean对象

### 九，启动Spring MVC模块接受用户请求



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