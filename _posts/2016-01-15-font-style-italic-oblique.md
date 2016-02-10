---
layout: post
title: CSS 中 font-style 的 italic 和 oblique 的区别
categories: blog
tags: [冷知识, 前端开发]
description: CSS 中 font-style 的 italic 和 oblique 的区别
---

W3School 有关 CSS 中 font-style 的描述是这样的：

值 | 描述
:--- | :---
normal | 默认值。浏览器显示一个标准的字体样式。
**italic** | 浏览器会显示一个**斜体**的字体样式。
**oblique** | 浏览器会显示一个**倾斜**的字体样式。
inherit | 规定应该从父元素继承字体样式。

> 点击[此处]()，查看 W3School 上的原文。

**那么，italic 和 oblique 的区别是什么呢？**

首先要理解字体的属性。
一种字体有**粗体**、**斜体**、**下划线**、**删除线**等诸多属性。但是并不是所有字体都实现了这些属性，一些不常用的字体，或许就只有标准体，如果你用**italic**属性，就没有效果了。
此时使用**oblique**属性，就可以有**倾斜**的显示效果。<br/>

可以这样理解，italic属性是使用文字的斜体，oblique是让没有斜体属性的文字倾斜。

<br/>

<div align="right">2016-02-10 星期三 7:07:06 PM</div>