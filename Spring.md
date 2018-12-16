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

### 