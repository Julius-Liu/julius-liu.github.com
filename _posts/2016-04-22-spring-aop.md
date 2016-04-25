---
layout: post
title: Spring AOP 知识整理
subtitle: 
categories: blog
tags: [JavaEE]
description: Spring AOP 知识整理
---

　　通过一个多月的 Spring AOP 的学习，掌握了 Spring AOP 的基本概念。AOP 是面向切面的编程（Aspect-Oriented Programming），是基于 OOP（面向对象的编程，Object-Oriented Programming）开发的一套程序架构。
　　个人粗浅的认为，在企业级的软件架构中，有许多分布式的软件架构。这就要求一段通用性的事务使用在软件中的许多模块。我所能想到的有交易模块、测试模块、日志模块等。以日志模块举例，根据 OOP 的思想，我可以建立一个日志类，然后在每一个需要记录日志的类中初始化日志类，达到日志记录的目的。然而这多个被初始化的日志类增加了系统的开销。如果采用 AOP 的思想，可以有一个日志类，然后使用代理的方式将这个日志类切入到需要记录日志的类中。这样做，就可以降低系统的开销，并且在代码组织的层面也显得较易维护。
　　在实践的过程中，使用 Spring AOP 的思想，为 PressSystem 项目加入了日志记录模块。日志采用 MySQL 数据库表进行记录，数据库连接采用 MyBatis。

## AOP 基本概念

　　为了加深理解，摘录 Spring 官方文档 Aspect Oriented Programming with Spring 的原文。每一个概念之后再加上自己的理解。

- **Aspect** : a modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented using regular classes (the schema-based approach) or regular classes annotated with the @Aspect annotation (the @AspectJ style).
- **Julius: Aspect（切面）**：一种切入多个类的模块化机制。Spring AOP 中，aspect 有两种实现方式：
使用常规的类，然后使用 基于 XML Schema 方式
使用常规的类，然后加上 @Aspect 注解，也就是 @AspectJ 方式
- **Join point** : a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
- **Julius：JoinPoint（连接点）**：可以理解为一个方法，就是切面要切入的那个方法。
- **Advice** : action taken by an aspect at a particular join point. Different types of advice include "around," "before" and "after" advice. (Advice types are discussed below.) Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.
- **Julius：Advice（通知）**：通知就是切面中的方法，这个方法将从JoinPoint切入。
- **Pointcut** : a predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.
- **Julius：Pointcut（切点）**：切点就是表达式，通知将会切入符合切点表达式的JoinPoint 中。
- **Introduction** : declaring additional methods or fields on behalf of a type. Spring AOP allows you to introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an IsModified interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)
- **Julius：Introduction（引入）**：引入机制可以向一个能被切入的对象发明新的接口，并实现它。
**Julius**：其实就是把能被切入的对象强制转换成另一个类，这样就可以执行另一个类的方法了。
- **Target object** : object being advised by one or more aspects. Also referred to as the advised object. Since Spring AOP is implemented using runtime proxies, this object will always be a proxied object.
- **Julius：Target Object（目标对象）**：就是能被切入的对象。Spring AOP 中，目标对象永远是可被代理的对象。
- **AOP proxy** : an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.
- **Julius：AOP Proxy（AOP 代理）**：AOP Proxy 就是 通过 AOP 框架生成出来的一个对象，这个对象实现了切面的功能，是目标类被切入以后的结果。Spring AOP 代理分两种：
JDK 动态代理
CGLIB 代理
- **Weaving** : linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.
- **Julius：Weaving（织入）**：织入就是把切面与目标类连接的过程。过程的产物就是一个被切入的目标。

## 通知的类型

　　简单地来说，通知可以有前置通知、后置通知、环绕通知等等。比如，前置通知就是通知在切入点执行之前执行；后置通知就是在切入点执行之后执行。在我的 Spring AOP 示例程序中，列举了以下几个通知的实现方式。

- **Before advice** : Advice that executes before a join point, but which does not have the ability to prevent execution flow proceeding to the join point (unless it throws an exception).
- **Julius：前置通知**：在连接点之前执行。除非是前置通知抛出了异常，否则前置通知没有能力影响执行流流向连接点。
- **After returning advice** : Advice to be executed after a join point completes normally: for example, if a method returns without throwing an exception.
- **Julius：返回后通知**：在连接点的方法正常执行完之后再执行的通知。所谓的正常执行完，比如说连接点方法没有抛出异常。
- **After throwing advice** : Advice to be executed if a method exits by throwing an exception.
- **Julius：抛出后通知**：如果方法因为抛出了异常而终止，那么就执行抛出后通知。
- **After (finally) advice** : Advice to be executed regardless of the means by which a join point exits (normal or exceptional return).
- **Julius：后置通知**：不管连接点方法的退出情况如何，是正常还是有异常，后置通知都会执行。
- **Around advice** : Advice that surrounds a join point such as a method invocation. This is the most powerful kind of advice. Around advice can perform custom behavior before and after the method invocation. It is also responsible for choosing whether to proceed to the join point or to shortcut the advised method execution by returning its own return value or throwing an exception.
- **Julius：环绕通知**：环绕通知是包围住连接点的通知，可以在连接点的前面或后面执行自定义的方法。环绕通知可以决定是否执行连接点的方法，或者绕过连接点的方法。绕过的方式是返回环绕通知自己的值或者抛出一个异常。

## PressSystem 项目中 Spring AOP 需要的组件

> 以下提到的组件，是实现 Spring AOP 的组件最小子集

- **XuanTiController.java** 
@Autowired
private XuanTiService xuanTiService;
下面有：
xuanTiService.save(xuanTi);
xuanTiService.update(xuanTi);
xuanTiService.delete(id, "XuanTi");
- **XuanTiService.java**
这个只是 interface
有如下的接口：
void save(XuanTi xuanTi);
boolean update(XuanTi xuanTi);
boolean delete(String id, String table_name);
- **XuanTiServiceImpl.java**
实现了 XuanTiService 接口
有如下实现：
public void save(XuanTi xuanTi) {...}
public boolean update(XuanTi xuanTi) {...}
public boolean delete(String id, String table_name) {...}
- **LogAspect.java**
有 @Aspect 标记
@Autowired
private LogService logService;
有多个通知
每个通知，有不同类型的通知的 @AspectJ 方式的标记，例如 @AfterReturning, @Around
每个通知，有 pointcut 配置，即每个通知要切入到哪里
- **spring-common.xml**
`<aop:aspectj-autoproxy />`
注释：如果Spring决定一个bean要被切入，那么Spring就会为这个bean自动生成代理来插入外来的方法，保证通知被执行
`<bean class="com.tgb.service.impl.XuanTiServiceImpl" />`
`<bean class="com.tgb.service.impl.BookServiceImpl" />`
注释：要切入的逻辑对象，这些是连接点
`<bean id="logAspect" class="com.tgb.utils.LogAspect" />`
注释：日志 AOP
`<bean id="LogService" class="com.tgb.service.impl.LogServiceImpl" />`
注释：日志记录业务逻辑对象
`<context:annotation-config />`
注释：启用注解配置方式，比如说启用了 @Autowired

## PressSystem 项目中 Spring AOP 的切入机制

1. 总的来说，是 LogAspect 这个 bean 切入了 XuanTiServiceImpl 这个 bean
2. 在 **spring-common.xml** 配置文件中，声明了 **XuanTiServiceImpl** 和 **LogAspect** 2个 bean
`<bean class="com.tgb.service.impl.XuanTiServiceImpl" />`
`<bean id="logAspect" class="com.tgb.utils.LogAspect" />`
3. 在 LogAspect 中，有很多配置，看如下的3行。这些配置指定了**saveLogInsert**,**updateLogInsert** 和 **deleteLogInsert** 这三个通知要切入 **XuanTiServiceImpl**
有如上所示的3个通知
每个通知，有不同类型的通知的 @AspectJ 方式的标记，例如 @AfterReturning, @Around
每个通知，有 pointcut 配置，即每个通知要切入到哪里
4. AOP 的实现方式是通过代理实现的。所以在 **spring-common.xml** 中指定了 autoproxy，为 **XuanTiServiceImpl** 创建代理，如下所示：
`<aop:aspectj-autoproxy />`
注释：如果Spring决定一个bean要被切入，那么Spring就会为这个 bean 自动生成代理来插入外来的方法，保证通知被执行
5. 在 XuanTiServiceImpl 中有如下3个方法，这3个方法符合 pointcut 的描述，因此具体地来说，这3个方法就是**切入点**，它们被顺利地切入了
public void save(XuanTi xuanTi) {...}
public boolean update(XuanTi xuanTi) {...}
public boolean delete(String id, String table_name) {...}


## PressSystem 项目中 Spring AOP 机制的调用过程

> 以下描述，主要涉及2点，可以概括为 @Aspect 和 @Autowired
> @Aspect 就是“切入机制”，@AfterReturning, @Around, pointcut, `<aop:aspectj-autoproxy />` 等知识点都属于这个知识体系
> @Autowired 是“自动绑定机制”，@Component 注解，广义的 bean 等知识点属于 @Autowired 的基础知识体系
> 说明：以上3句话，是作者的粗浅认知
> 题外话：下面所提到的调用过程，是 Spring AOP 相关的过程最小子集

1. XuanTiController 被调用了，具体来说，其中的 xuanTiService.save(xuanTi); 被调用了
2. 因为在 XuanTiController 有标注为 @Autowired 的 xuanTiService，所以 Application 会去查找 xuanTiService 这个 bean
3. 至于为什么会去查找 xuanTiService 这个 bean 呢，是因为 **spring-common.xml** 中的配置
`<context:annotation-config />`
注释：启用注解配置方式，比如说启用了 @Autowired
4. XuanTiService 只是一个 interface，实现它的是 XuanTiServiceImpl
5. XuanTiServiceImpl 这个 bean 找到了，因为在 **spring-common.xml** 中有配置
`<bean class="com.tgb.service.impl.XuanTiServiceImpl" />`
6. xuanTiService.save(xuanTi); 被执行了，具体来说，是 XuanTiServiceImpl 中的  public void save(XuanTi xuanTi) {...} 被执行了
7. 根据上一章节“切入机制”的描述，在执行 public void save(XuanTi xuanTi) {...} 的时候，会伴随着执行 LogAspect 中的 saveLogInsert 方法
8. 在 LogAspect 中，有标注为 @Autowired 的 logService，所以 Application 会去查找 logService 这个 bean
9. 查找的原因，就是第3点所描述的
10. LogService 只是一个 interface，实现它的是 LogServiceImpl
11. XuanTiServiceImpl 这个 bean 找到了，因为在 **spring-common.xml** 中有配置
`<bean id="LogService" class="com.tgb.service.impl.LogServiceImpl" />`
注释：日志记录业务逻辑对象
12. logService.log(log); 被执行了，具体来说，是 LogServiceImpl 中的 public void log(Log log) {...} 被执行了



AOP Proxies 简单介绍

`<aop:aspectj-autoproxy />`

声明一个 Aspect, Pointcut

声明通知的方法：
分五种不同的通知类型进行举例

- @Aspectj
- Schema-based

Advice Parameters

Introductions 简单介绍

- @Aspectj
- Schema-based

编程式创建 @Aspectj Proxy


## 小结

　　这里是小结

## 附录：这里是附录

　　这里是附录

## 参考资料

　　这里是参考资料

<br/>

<div align="right">4/25/2016 5:26:58 PM </div>