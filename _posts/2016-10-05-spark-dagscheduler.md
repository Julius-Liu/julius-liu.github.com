---
layout: post
title: Spark - DAGScheduler的工作原理
categories: blog
tags: [Spark]
description: Spark - DAGScheduler的工作原理
---

> 从 Spark：大数据的“电光石火” 中摘录。

## 摘要 ##

　　Spark Framework中，Stage和Task的调度由DAGScheduler来管理，下面详解它的工作原理。

## 正文 ##

　　RDD的数据结构里很重要的一个依赖关系是对父RDD的依赖。如下图所示，有两类依赖：窄（Narrow）依赖和宽（Wide）依赖。

<center>
  <p><img src="/images/spark-dagscheduler/dagscheduler.png" align="center"></p>
</center>

　　**窄依赖：**指父RDD的每一个分区最多被一个子RDD的分区所用，表现为一个父RDD的分区对应于一个子RDD的分区，和两个父RDD的分区对应于一个子RDD的分区。上图中，map/filter和union属于第一类窄依赖，对输入进行协同划分（co-partitioned）的join属于第二类窄依赖。

　　**宽依赖：**指子RDD的分区依赖于父RDD的所有分区，这是因为shuffle类操作，如上图中的groupByKey和未经协同划分的join。

　　窄依赖对优化很有利。逻辑上，每个RDD的算子都是一个fork/join（说明：此join非上文的join算子，而是指同步多个并行任务的barrier）。Spark Framework把计算fork到每个Partition，算完后join，然后fork/join下一个RDD的算子。

　　如果直接翻译到物理实现，是很不经济的：

1. 每一个RDD（即使是中间结果）都需要物化到内存或存储中，费时费空间。
2. join作为全局的barrier，是很昂贵的，会被最慢的那个节点拖死。

　　如果子RDD的Partition到父RDD的Partition是窄依赖，就可以实施经典的fusion优化，把两个fork/join合为一个；如果连续的变换算子序列都是窄依赖，就可以把很多个 fork/join并为一个，不但减少了大量的全局barrier，而且无需物化很多中间结果RDD，这将极大地提升性能。Spark把这个叫做**流水线（pipeline）优化**。

　　Transformation级别的算子序列一碰上shuffle类操作，宽依赖就发生了，流水线优化终止。在具体实现中，DAGScheduler从当前算子往前回溯依赖图，一碰到宽依赖，就生成一个stage来容纳已遍历的算子序列。在这个stage里，可以安全地实施流水线优化。然后，又从那个宽依赖开始继续回溯，生成下一个stage。

　　有两个问题需要深究：

1. 分区如何划分
2. 分区该放到集群内哪个节点

　　这正好对应于RDD结构中另外两个域：分区划分器（partitioner）和首选位置（preferred locations）。

　　**分区划分**对于shuffle类操作很关键，它决定了该操作的父RDD和子RDD之间的依赖类型。上文提到，同一个join算子，如果协同划分的话，两个父RDD之间、父RDD与子RDD之间能形成一致的分区安排，**即同一个key的键值对保证被映射到同一个分区**，这样就能形成**窄依赖**。反之，如果没有协同划分，导致**宽依赖**。

　　所谓**协同划分**，就是指定partitioner以产生前后一致的分区安排。Pregel和HaLoop把这个作为系统内置的一部分；而Spark默认提供两种划分器：HashPartitioner和RangePartitioner，允许程序通过partitionBy算子指定。

　　注意，HashPartitioner能够发挥作用，要求key的hashCode是有效的，即同样内容的key产生同样的hashCode。这对String是成立的，但对数组就不成立（因为数组的hashCode是由它的标识生成，而非由内容生成）。这种情况下，Spark允许用户自定义ArrayHashPartitioner。

　　第二个问题是分区放置的节点，这关乎数据本地性：本地性好，网络通信就少。

1. 有些RDD产生时就有首选位置，如HadoopRDD分区的首选位置就是HDFS块所在的节点。有些RDD或分区被缓存了，那计算就应该送到缓存分区所在的节点进行。
2. 如果本身没有首选位置，就回溯RDD的lineage一直找到具有首选位置属性的父RDD，并据此决定子RDD的放置。

　　窄依赖和宽依赖的概念不止用在调度中，对容错也很有用。如果一个节点宕机了，而且运算是窄依赖，那只要把丢失的父RDD分区重算即可，跟其他节点没有依赖。而宽依赖需要父RDD的所有分区都存在，重算就很昂贵了。<br/>
　　所以如果使用checkpoint算子来做检查点，不仅要考虑lineage是否足够长，也要考虑是否有宽依赖，对宽依赖加检查点是最物有所值的。

<br/>

<div align="right">10/5/2016 11:43:19 AM </div>