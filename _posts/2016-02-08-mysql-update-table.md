---
layout: post
title: 使用原表的信息更新表中的数据
categories: blog
tags: [数据库&SQL]
description: 使用原表的信息更新表中的数据
---

在 MySQL 中，当我尝试执行如下 sql 语句：

    update TABLE_NAME set COLUMN_NAME = 'value' where PRIMARY_COLUMN in (
        select PRIMARY_COLUMN from TABLE_NAME
    )

遇到了错误：<br/>

    [Err] 1093 - You can't specify target table TABLE_NAME for update in FROM clause

原因是在尝试更新这个表的数据的时候，又查询了表的数据，而这些查询的数据，是要执行更新操作的。

**解决方法如下：**

1. 将要更新的数据的参考字段，临时抽取出来，成为另外一张表，比如形成下面的表 b：

	    update TABLE_NAME a SET a.COLUMN_NAME = 'value' WHERE a.PRIMARY_COLUMN in (
	        select b.PRIMARY_COLUMN from (
	    	    SELECT PRIMARY_COLUMN from TABLE_NAME
	    	) b
	    )

2. 新建一张临时表保存要查询的数据，然后筛选更新，最后删除临时表。

推荐使用第一种方法，不用额外新建一张临时表。

<br/>

<div align="right">2/8/2016 12:55:02 PM </div>
