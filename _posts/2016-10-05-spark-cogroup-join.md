---
layout: post
title: Spark中的函数 - cogroup和join
categories: blog
tags: [Spark]
description: Spark中的函数 - cogroup和join
---

## 一、cogroup ##

1.处理两个RDD中的Key-Value元素，每个RDD中相同Key中的元素分别聚合成一个集合。与reduceByKey不同的是针对两个RDD中相同的key的元素进行合并。

2.cogroup的参数可以是1个或者多个RDD。

例如：

    var rdd3 = rdd1.cogroup(rdd2)
    var rdd4 = rdd1.cogroup(rdd2,rdd3)

3.cogroup相当于SQL中的全外关联**full join（full outer join）**，返回左右RDD中的记录，关联不上的为空。

### 测试例子 ###

    val DBName=Array(  
      Tuple2(1,"Spark"),  
      Tuple2(2,"Hadoop"),  
      Tuple2(3,"Kylin"),  
      Tuple2(4,"Flink")
    )  
    
    val numType=Array(  
      Tuple2(1,"String"),  
      Tuple2(2,"int"),  
      Tuple2(3,"byte"),  
      Tuple2(4,"boolean"),  
      Tuple2(5,"float"),  
      Tuple2(1,"34"),  
      Tuple2(1,"45"),  
      Tuple2(2,"47"),  
      Tuple2(3,"75"),  
      Tuple2(4,"95"),  
      Tuple2(5,"16"),  
      Tuple2(1,"85")  
    )  
    
    val names=sc.parallelize(DBName)  
    val types=sc.parallelize(numType)  
    val nameAndType=names.cogroup(types)  //基于Key进行join, 结果并没有顺序  
    nameAndType.collect.foreach(println)

输出的结果：

(4,(CompactBuffer(Flink),CompactBuffer(boolean, 95)))  
(1,(CompactBuffer(Spark),CompactBuffer(String, 34, 45, 85)))  
(3,(CompactBuffer(Kylin),CompactBuffer(byte, 75)))  
(5,(CompactBuffer(),CompactBuffer(float, 16)))    // DBName中**没有**Key为5的Tuple，numType中**有**Key为5的Tuple
(2,(CompactBuffer(Hadoop),CompactBuffer(int, 47)))

## 二、join ##

**1.只返回两个RDD根据Key可以关联上的结果。**
2.join只能用于两个RDD之间的关联，如果要多个RDD关联，多关联几次即可。
3.join相当于SQL中的内关联**inner join**。

### 测试例子 ###

    var rdd1 = sc.makeRDD(Array(("A","1"),("B","2"),("C","3")),2)
    var rdd2 = sc.makeRDD(Array(("A","a"),("C","c"),("D","d")),2)
     
    scala> rdd1.join(rdd2).collect

输出的结果：

res10: Array[(String, (String, String))] = Array((A,(1,a)), (C,(3,c)))

<br/>

<div align="right">10/5/2016 10:34:30 AM </div>
