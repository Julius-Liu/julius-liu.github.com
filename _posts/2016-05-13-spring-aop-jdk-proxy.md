---
layout: post
title: Spring AOP 中的 JDK 动态代理
subtitle: 
categories: blog
tags: [JavaEE, Spring]
description: Spring AOP 中的 JDK 动态代理
---

　　本文将基于自己写的代码 package com.spring.proxy 来讲解 JDK 动态代理的实现过程。之后会给出一张时序图，更清晰地呈现调用过程。<br>
　　JDK的代理方式主要就是通过反射跟动态编译来实现的，主要涉及到java.lang.reflect包中的两个类：`Proxy` 和 `InvocationHandler`。其中 `InvocationHandler` 是一个接口，可以通过实现该接口定义横切逻辑，在并通过反射机制调用目标类的代码，动态将横切逻辑和业务逻辑编织在一起。<br>
<br>
　　首先把一些思想记录下来，之后将会整理。<br>
<br>
　　主要看了以下两篇代码，作为互相参考：

- com.spring.proxy.Proxy —— 这是自己写的 demo 代码
- java.lang.reflect.Proxy —— 这是 java library 中自带的库文件，可以视作高级版本

　　想要说的是，跟着 com.spring.proxy.Proxy 的代码，可以清晰地看到：

1. 首先通过反射获取了类的方法

1. 然后创建了一个代理的 java 源文件

1. 编译这个源文件，得到 class 文件

1. 在需要使用的时候，加载这个 class 文件到内存中，这样就可以使用了

1. 使用的时候，需要传入两个参数：被代理的接口类和代理类

1. 在代理类中，学名叫做 InvocationHandler 中，可以加入自己的方法，可以将这些方法放在 invoke 的前面或者后面，这样就好像对代理的接口类注入了方法一样

　　为了讲解清楚，附上时序图一张。需要注意的是，这张时序图是从其他项目中摘取的，只能借鉴想法。本项目的时序图将在后期给出。

<center>
  <p><img src="/images/jdk-proxy/071311285.gif" align="center"></p>
</center>

##写在后面的想法

　　有关 `org.springframework.aop.framework.ProxyFactory`，发现这个类是一个很强大的类

- 可以通过 setTarget 来指定需要代理的类
- 可以通过 addAdvice 来为需要代理的类注入通知

　　可以这么说，如果我不加 addAdvice，那么就是一个纯的代理。与之前的 InvocationHandler 做比较，就可以发现如果我在 InvocationHandler 中不加入自己的方法，那么就是一个纯的动态代理。<br>
　　那么问题来了，

1. `org.springframework.aop.framework.ProxyFactory` 的代理方式和 `com.spring.proxy.Proxy`，或者说 `java.lang.reflect.Proxy` 的代理方式是一样的吗？

1. `org.springframework.aop.framework.ProxyFactory` 中的 addAdvice 的实现方式和 `com.spring.proxy.Proxy` 中的 InvocationHandler 中的自定义方法是一样的吗？

　　`org.springframework.aop.framework.ProxyFactory` 的源代码看起来太累了，因为它是很上层的封装了，要慢慢地一层一层的剥下去。<br>
　　突然想到，有可能又要去回忆各种 Advice 的实现方式了，因为可以帮助解决上面的第二个问题。