---
layout: post
title: MySQL 的外键约束
categories: blog
tags: [数据库&SQL]
description: MySQL 的外键约束
---

外键是指一个表中的一个字段（或者一组字段），它（它们）可以唯一标识另一个表的一条记录。有外键定义的表通常被称为“子表”，被外键标识的表叫做“父表”，“父表”中被外键标识的字段通常是主键。

**外键取值规则：空值或参照的主键值。**

**外键具有如下属性：**
1. 尝试向外键插入非空值时，如果对应的主键表中没有这个值，则不能插入。
1. 记录更新时，外键的值不能改为主键表中没有的值。
1. 删除主键表记录时，你可以在建外键时选定外键记录的级联操作，是“级联删除”还是“拒绝删除”。
1. 更新主键记录时，同样有“级联更新”和“拒绝执行”的选择。

添加外键的两种方法：

1) 在创建表的时候

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

2) 在修改表的时候

    ALTER TABLE table_name ADD INDEX index_name ( index_column );
    ALTER TABLE table_name ADD CONSTRAINT foreign_key_name
      FOREIGN KEY ( foreign_key_column ) 
      REFERENCES table_name ( table_column ) 
      [ON DELETE reference_option]
      [ON UPDATE reference_option]

	reference_option:
    	RESTRICT | CASCADE | SET NULL | NO ACTION

