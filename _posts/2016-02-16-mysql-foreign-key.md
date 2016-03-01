---
layout: post
title: MySQL 的外键约束
categories: blog
tags: [数据库&SQL]
description: MySQL 的外键约束
---

外键是指一个表中的一个字段（或者一组字段），它（它们）可以唯一标识另一个表的一条记录。有外键定义的表通常被称为**“子表”**，被外键标识的表叫做**“父表”**，“父表”中被外键标识的字段通常是主键。

**外键取值规则：空值或参照的主键值。**

**外键具有如下属性：**

1. 尝试向外键插入非空值时，如果对应的主键表中没有这个值，则不能插入。
1. 记录更新时，外键的值不能改为主键表中没有的值。
1. 删除主键表记录时，你可以在建外键时选定外键记录的级联操作，是“级联删除”还是“拒绝删除”。
1. 更新主键记录时，同样有“级联更新”和“拒绝执行”的选择。

### 添加外键的两种方法：###

1) 在**创建表**的时候

    CREATE TABLE table_name (
      column_name1 data_type
	  column_name2 data_type
      KEY index_name (index_column),
      CONSTRAINT foreign_key_name
	    FOREIGN KEY (foreign_key_column) 
		REFERENCES table_name (table_column)
		[ON DELETE reference_option]
		[ON UPDATE reference_option]
    )

	reference_option:
    	RESTRICT | CASCADE | SET NULL | NO ACTION

2) 在**修改表**的时候

    ALTER TABLE table_name ADD INDEX index_name ( index_column );
    ALTER TABLE table_name ADD CONSTRAINT foreign_key_name
      FOREIGN KEY ( foreign_key_column ) 
      REFERENCES table_name ( table_column ) 
      [ON DELETE reference_option]
      [ON UPDATE reference_option]

	reference_option:
    	RESTRICT | CASCADE | SET NULL | NO ACTION

### 查看外键的方法：

	SHOW CREATE TABLE table_name

这条语句将会创建完整的创建表的 sql 命令，从中可以查看所有外键和它们的名称。

### 修改外键的方法：

无法修改已存在的外键约束，只有通过删除已有的，再次添加的方法来修改。

### 删除外键的方法：

	ALTER TABLE table_name DROP FOREIGN KEY foreign_key_name

这条语句将会删除名为 `foreign_key_name` 的外键。

### reference_option 说明：

- **CASCADE：**删除或更新父表中的数据，同时自动删除或更新子表中的级联数据。
- **RESTRICT：**拒绝删除或更新父表中的数据。省略 `ON DELETE` 或者 `ON UPDATE` 关键字，将和使用 `RESTRICT` 有相同的效果。即，`RESTRICT` 是 MySQL 中的缺省配置。
- **NO ACTION：**从标准 SQL 中衍生的关键字。在 MySQL 中，`NO ACTION` 和 `RESTRICT` 相同。
- **SET NULL：**删除或更新父表中的数据，同时将子表中相关数据的级联字段设置为 `NULL`。当父表中的记录被删除，而子表中的数据应该不受影响的时候，`SET NULL` 非常管用。

### Foreign Key 实践

创建如下两张表格，`customer` 和 `contact`

父表 `customer`

![customer](/images/mysql-foreign-key/pic01.PNG)

子表 `contact`

![contact](/images/mysql-foreign-key/pic02.PNG)

> 说明：`customer` 中的 `id` 和 `contact` 中的 `customer_id` 有主外键关联。

1) 尝试向 `contact` 中插入 `customer` 中不存在的记录时，将会出错；同理，将 `contact` 中的原有记录改为 `customer` 中不存在的记录时，也会出错。

&nbsp;&nbsp;错误显示如下：

	Cannot delete or update a child row: a foreign key constraint fails

2) `ON UPDATE CASCADE` 情况下，更新 `customer` 中数据会导致 `contact` 级联更新。

	update customer set id = 9 where name = '李易'

更新前如上两图所示，更新后如下所示：

父表 `customer`

![customer](/images/mysql-foreign-key/pic03.PNG)

子表 `contact`

![contact](/images/mysql-foreign-key/pic04.PNG)

3) `ON DELETE CASCADE` 情况下， 删除 `customer` 中数据会导致 `contact` 级联删除。

举例略。

4) `RESTRICT` 或 `NO ACTION` 情况下，更新或是删除 `customer` 中的数据将被禁止！会收到如下的错误消息：

	Cannot delete or update a parent row: a foreign key constraint fails

5) `ON UPDATE SET NULL` 情况下，更新 `customer` 中数据将会导致 `contact` 中相关数据设置为 `NULL`，如下所示：

更新前的父表 `customer`

![customer](/images/mysql-foreign-key/pic05.PNG)

更新前的子表 `contact`

![contact](/images/mysql-foreign-key/pic06.PNG)

执行如下语句：

	update customer set id = 9 where `name` = '李易'

更新后的父表 `customer`

![customer](/images/mysql-foreign-key/pic07.PNG)

更新后的子表 `contact`

![contact](/images/mysql-foreign-key/pic08.PNG)

6) `ON DELETE SET NULL` 情况下，删除 `customer` 中数据将会导致 `contact` 中相关数据设置为 `NULL`。

举例略。

### 外键的优点和缺点

> 以下回答来自“知乎”社区，点击结尾的链接查看详细回答

外键是否采用看业务应用场景，以及开发成本的，大致列下什么时候适合，什么时候不适合使用。<br/>
互联网行业应用不推荐使用外键，原因有以下两点：

1. 数据库的诸多设计，帐号，权限，约束，触发器，都是为 C/S 结构设计的，是以 C 端不可信做为假设前提的。B/S 模式安全边界前移到 web 服务层，应用与数据库之间是可信的，应用自行完成这些功能更加灵活。
1. 互联网应用中，用户量大，并发度高，为此数据库服务器很容易成为性能瓶颈，尤其受IO能力限制，且不能轻易地水平扩展；若是把数据一致性的控制放到事务中，也即让应用服务器承担此部分的压力，而引用服务器一般都是可以做到轻松地水平的伸缩。

传统行业中，可以使用外键约束。原因也有两点：

1. 软件应用的人数有限，换句话说是可控的。
1. 数据库服务器的数据量也一般不会超大，且活跃数据有限。

所以，在传统行业中，数据库服务器的性能不是问题，所以不用过多考虑性能的问题；另外，使用外键可以降低开发成本，借助数据库产品自身的触发器可以实现表与关联表之间的数据一致性和更新；最后一点，使用外键的方式，还可以做到开发人员和数据库设计人员的分工，可以为程序员承担更多的工作量。

**为什么外键约束会影响性能？**

1. 数据库需要维护外键的内部管理。
1. 外键等于把数据的一致性事务实现，全部交给数据库服务器完成。
1. 有了外键，当做一些涉及外键字段的增，删，更新操作之后，需要触发相关操作去检查，而不得不消耗资源。
1. 外键还会因为需要请求对其他表内部加锁而容易出现死锁情况。

**知乎申明！！**
著作权归作者所有。
商业转载请联系作者获得授权，非商业转载请注明出处。
作者：响马
链接：<https://www.zhihu.com/question/19600081/answer/12363985>
来源：知乎
作者：mysqlops
链接：<https://www.zhihu.com/question/19600081/answer/13295957>
来源：知乎

以上。

<br/>

<div align="right">2/17/2016 2:54:22 PM </div>