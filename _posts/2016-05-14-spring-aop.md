---
layout: post
title: Spring AOP 知识整理
subtitle: 
categories: blog
tags: [JavaEE, Spring]
description: Spring AOP 知识整理
---

　　通过一个多月的 Spring AOP 的学习，掌握了 Spring AOP 的基本概念。AOP 是面向切面的编程（Aspect-Oriented Programming），是基于 OOP（面向对象的编程，Object-Oriented Programming）开发的一套程序架构。<br/>
　　个人粗浅的认为，在企业级的软件架构中，有许多分布式的软件架构。这就要求一段通用性的事务使用在软件中的许多模块。我所能想到的有交易模块、测试模块、日志模块等。以日志模块举例，根据 OOP 的思想，我可以建立一个日志类，然后在每一个需要记录日志的类中初始化日志类，达到日志记录的目的。然而这多个被初始化的日志类增加了系统的开销。如果采用 AOP 的思想，可以有一个日志类，然后使用代理的方式将这个日志类切入到需要记录日志的类中。这样做，就可以降低系统的开销，并且在代码组织的层面也显得较易维护。<br/>
　　在实践的过程中，使用 Spring AOP 的思想，为 PressSystem 项目加入了日志记录模块。日志采用 MySQL 数据库表进行记录，数据库连接采用 MyBatis。

## AOP 基本概念

　　为了加深理解，摘录 Spring 官方文档 Aspect Oriented Programming with Spring 的原文。每一个概念之后再加上自己的理解。

- **Aspect** : a modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented using regular classes (the schema-based approach) or regular classes annotated with the @Aspect annotation (the @AspectJ style).

- **Julius: Aspect（切面）**：一种切入多个类的模块化机制。Spring AOP 中，aspect 有两种实现方式：<br>
使用常规的类，然后使用 基于 XML Schema 方式<br>
使用常规的类，然后加上 @Aspect 注解，也就是 @AspectJ 方式<br>

- **Join point** : a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.

- **Julius：JoinPoint（连接点）**：可以理解为一个方法，就是切面要切入的那个方法。

- **Advice** : action taken by an aspect at a particular join point. Different types of advice include "around," "before" and "after" advice. (Advice types are discussed below.) Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.

- **Julius：Advice（通知）**：通知就是切面中的方法，这个方法将从JoinPoint切入。

- **Pointcut** : a predicate that matches join points. Advice is associated with a pointcut expression and runs at any join point matched by the pointcut (for example, the execution of a method with a certain name). The concept of join points as matched by pointcut expressions is central to AOP, and Spring uses the AspectJ pointcut expression language by default.

- **Julius：Pointcut（切点）**：切点就是表达式，通知将会切入符合切点表达式的JoinPoint 中。

- **Introduction** : declaring additional methods or fields on behalf of a type. Spring AOP allows you to introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an IsModified interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)

- **Julius：Introduction（引入）**：引入机制可以向一个能被切入的对象发明新的接口，并实现它。<br>
**Julius**：其实就是把能被切入的对象强制转换成另一个类，这样就可以执行另一个类的方法了。<br>

- **Target object** : object being advised by one or more aspects. Also referred to as the advised object. Since Spring AOP is implemented using runtime proxies, this object will always be a proxied object.

- **Julius：Target Object（目标对象）**：就是能被切入的对象。Spring AOP 中，目标对象永远是可被代理的对象。

- **AOP proxy** : an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.

- **Julius：AOP Proxy（AOP 代理）**：AOP Proxy 就是 通过 AOP 框架生成出来的一个对象，这个对象实现了切面的功能，是目标类被切入以后的结果。Spring AOP 代理分两种：<br>
JDK 动态代理<br>
CGLIB 代理<br>

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

## PressSystem 项目中 Spring AOP 的基本组件

> 以下提到的组件，是实现 Spring AOP 的组件最小子集。

- **XuanTiController.java**<br>
　　在 PressSystem 项目中，用户操作的是 jsp 页面，jsp 页面会发送一个异步请求给 XuanTiController，在 XuanTiController 中，会调用切入点的方法。所以可以把 XuanTiController 看作入口。

- **XuanTiService.java**<br>
　　采用接口与实现分离的设计原则，XuanTiService 就是接口，它的实现是 XuanTiServiceImpl，XuanTiServiceImpl 就是切入点。

- **XuanTiServiceImpl.java**<br>
　　XuanTiServiceImpl 就是切入点，其中的函数被通知切入了。

- **LogAspect.java**<br>
　　LogAspect 就是切面，其中有一些通知，这些通知将会切入到切入点中。

- **spring-common.xml**<br>
　　这是切入点和切面的配置文件。

## PressSystem 项目中 Spring AOP 的切入机制

1. 总的来说，是 LogAspect 这个 bean 切入了 XuanTiServiceImpl 这个 bean

2. 在 **spring-common.xml** 配置文件中，声明了 **XuanTiServiceImpl** 和 **LogAspect** 2个 bean<br>
`<bean class="com.tgb.service.impl.XuanTiServiceImpl" />`<br>
`<bean id="logAspect" class="com.tgb.utils.LogAspect" />`

3. 在 LogAspect 中，有很多配置，看如下的3行。这些配置指定了<br>
**saveLogInsert**<br>
**updateLogInsert**<br>
**deleteLogInsert**<br>
这三个通知要切入 **XuanTiServiceImpl** <br>
`@AfterReturning(pointcut="execution(* com.tgb.service.impl.*.save(..))", argNames="returnValue", returning="returnValue")` <br>
`@AfterReturning(pointcut="execution(* com.tgb.service.impl.*.update(..))", argNames="returnValue", returning="returnValue")` <br>
`@Around("execution(* com.tgb.service.impl.*.delete(..)) && args(id, table_name)")` <br>
注意，每个通知，有不同类型的通知的 @AspectJ 方式的标记，例如 @AfterReturning, @Around <br>
另外，每个通知，可以有 pointcut 配置，即每个通知要切入到哪里

4. AOP 的实现方式是通过代理实现的。所以在 **spring-common.xml** 中指定了 autoproxy，为 **XuanTiServiceImpl** 创建代理，如下所示：<br>
`<aop:aspectj-autoproxy />`<br>
注释：如果Spring决定一个bean要被切入，那么Spring就会为这个 bean 自动生成代理来插入外来的方法，保证通知被执行

5. 在 XuanTiServiceImpl 中有如下3个方法，这3个方法符合 pointcut 的描述，因此具体地来说，这3个方法就是**切入点**，它们被顺利地切入了 <br>
`public void save(XuanTi xuanTi) {...}` <br>
`public boolean update(XuanTi xuanTi) {...}` <br>
`public boolean delete(String id, String table_name) {...}`

## PressSystem 项目中 Spring AOP 机制的调用过程

> 以下描述，主要涉及2点，可以概括为 @Aspect 和 @Autowired<br>
> @Aspect 就是“切入机制”，@AfterReturning, @Around, pointcut, `<aop:aspectj-autoproxy />` 等知识点都属于这个知识体系<br>
> @Autowired 是“自动绑定机制”，@Component 注解，广义的 bean 等知识点属于 @Autowired 的基础知识体系<br>
> 说明：以上3句话，是作者的粗浅认知<br>
> 题外话：下面所提到的调用过程，是 Spring AOP 相关的过程最小子集

1. XuanTiController 被调用了，具体来说，其中的 `xuanTiService.save(xuanTi);` 被调用了

2. 因为在 XuanTiController 有标注为 @Autowired 的 xuanTiService，所以 Application 会去查找 xuanTiService 这个 bean

3. 至于为什么会去查找 xuanTiService 这个 bean 呢，是因为 **spring-common.xml** 中的配置 <br>
`<context:annotation-config />`<br>
注释：启用注解配置方式，比如说启用了 @Autowired 的识别。

4. XuanTiService 只是一个 interface，实现它的是 XuanTiServiceImpl

5. XuanTiServiceImpl 这个 bean 找到了，因为在 **spring-common.xml** 中有配置 <br>
`<bean class="com.tgb.service.impl.XuanTiServiceImpl" />`

6. xuanTiService.save(xuanTi); 被执行了，具体来说，是 XuanTiServiceImpl 中的  public void save(XuanTi xuanTi) {...} 被执行了

7. 根据上一章节“切入机制”的描述，在执行 `public void save(XuanTi xuanTi) {...}` 的时候，会伴随着执行 LogAspect 中的 saveLogInsert 方法

8. 在 LogAspect 中，有标注为 @Autowired 的 logService，所以 Application 会去查找 logService 这个 bean

9. 查找的原因，就是第3点所描述的

10. LogService 只是一个 interface，实现它的是 LogServiceImpl

11. XuanTiServiceImpl 这个 bean 找到了，因为在 **spring-common.xml** 中有配置 <br>
`<bean id="LogService" class="com.tgb.service.impl.LogServiceImpl" />` <br>
注释：日志记录业务逻辑对象

12. logService.log(log); 被执行了，具体来说，是 LogServiceImpl 中的 public void log(Log log) {...} 被执行了

## 知识拓展

　　在本章节，我将会介绍另外一些 Spring AOP Docs 中提到的基础知识。这些基础知识没有使用在 PressSystem 项目中，但是作为 Spring AOP Docs 中的基础知识，应当有所了解。

### 五种类型的通知的实现方式

　　在前面的章节中，提到过五种类型的通知。这五种类型的通知，在我的 aop_demo 示例程序中都有使用。注意，声明通知的方法有以下两种方式：

- @Aspectj
- Schema-based

　　在我的示例程序中，都有展示。

### 通知参数的使用方法

　　声明一个通知的时候，是可以传入切入点的参数，在通知中使用这个参数的，例如上文中提到的 delete 通知：<br>
　　`@Around("execution(* com.tgb.service.impl.*.delete(..)) && args(id, table_name)")` <br>
　　当我传入了 `(id, table_name)` 参数后，可以在通知中使用 id 和 table_name。delete 通知的目的是，当 delete 方法执行的时候，写下日志。在日志中我要知道删除的 id 和 table_name。传入了这两个参数之后，就可以记录了。<br>
　　通知的参数可以很复杂，用于满足实际需要。想要了解更多通知参数的使用方法，可以查看 Spring AOP Docs。

### Pointcut - 切点表达式的使用

　　`Pointcut` 是 `Advice` 的具体配置，指定了一个 Advice 将会切入到哪些切入点中，这是由切点表达式决定的。Pointcut 是一种十分重要的机制，在 PressSystem 的 LogAspect 中有简单的应用。想要知道更多使用方法，可以参考 Spring AOP Docs。

### Introductions 简单介绍

　　以下是 Spring AOP Docs 中的原版描述：<br>
　　Introductions (known as inter-type declarations in AspectJ) enable an aspect to declare that advised objects implement a given interface, and to provide an implementation of that interface on behalf of those objects.<br>
　　相关的代码，可以参看 com.spring.demo08 和 com.spring demo14。其中：

- com.spring.demo08 是 Schema 方式实现的
- com.spring.demo14 是 @AspectJ 方式实现的

　　根据我的理解，Introduction 的意义就是接口类型强转，但是看到一句注释说，这是**“接口动态实现”**，觉得里面还有其他的花头我没有理解。。。

### AOP Proxy 原理简介

　　Spring 的动态代理有两种：一是 JDK 的动态代理；另一个是 cglib 动态代理（通过修改字节码来实现代理）。可以以 JDK 动态代理的方式为例，来粗浅地探知 AOP Proxy 的究竟。<br>
　　JDK的代理方式主要就是通过反射跟动态编译来实现的，主要涉及到java.lang.reflect包中的两个类：`Proxy` 和 `InvocationHandler`。其中 `InvocationHandler` 是一个接口，可以通过实现该接口定义横切逻辑，在并通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编织在一起。
　　将会在另一篇文章中，重点来谈一下 Spring AOP 的底层实现技术：JDK 动态代理。

## 小结

　　这篇博客写了很久，一方面是由于差不多是利用业余时间在学习，另一方面是由于 Spring AOP 的知识点有很多。<br>
　　在面向切面的编程的概念中，比较重要的是切面类和目标类。切面类中有切面方法，目标类中有切入点。通过正确的配置，可以使切面方法切入到目标类的切面点中。<br>
　　这篇博客首先介绍了 AOP 的概念，然后介绍了通知的5种类型。接下来，以 PressSystem 为例，讲述了 PressSystem 中的 AOP 的基本组件，切入机制和调用过程。<br>
　　最后，在知识拓展中，十分简单地提到了五种类型的通知的实现方式，通知参数的使用方法，Pointcut - 切点表达式的使用，Introductions 简单介绍。最重要的一点，我认为是 AOP Proxy 原理简介。有关 AOP Proxy 原理简介，请参看另一篇博客：Spring AOP 中的 JDK 动态代理。

## 参考资料

1. [10. Aspect Oriented Programming with Spring](http://docs.spring.io/spring-framework/docs/current/spring-framework-reference/html/aop.html#aop-schema-advice-before) （这是总纲，可以作为知识点的引入）
1. [AOP 那点事儿](http://my.oschina.net/huangyong/blog/161338?fromerr=P1lV8UQl) （这篇文章写得并不好，太随性，思路不清楚）
1. [Spring AOP Example Tutorial – Aspect, Advice, Pointcut, JoinPoint, Annotations, XML Configuration](http://www.journaldev.com/2583/spring-aop-example-tutorial-aspect-advice-pointcut-joinpoint-annotations-xml-configuration) （Spring AOP demo）
2. [Spring AOP 完成日志记录](http://hotstrong.iteye.com/blog/1330046) （PressSystem 的日志功能实现，参考了这篇文章）
3. [Spring 容器AOP的实现原理——动态代理](http://wiki.jikexueyuan.com/project/ssh-noob-learning/dynamic-proxy.html) （Spring AOP 动态代理的学习）
4. [Spring AOP的底层实现技术---JDK动态代理 ](http://blog.csdn.net/arthur0088/article/details/5377736) （Spring AOP 动态代理的学习）

<br/>

<div align="right">2016-05-14 星期六 6:32:24 PM</div>