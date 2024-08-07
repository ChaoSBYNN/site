---
title: 数据挖掘-MapReduce
date: 2017-03-14 22:31:42
tags: DataMining
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/datamining.jpeg"
---

> 可以通过分布式进行大数据量的计算的软件框架

|属性|描述|
|:---:|:---:|
|优点|短时间内完成大量工作|
|缺点|算法必须经过重写，需要对系统工程有一定理解|
|适用|数值型，标称型|

MapReduce 在大量节点组成的集群上运行（分布式计算）。

# 流程

1. map阶段 : 单个作业被分成很多小分，输入数据也被切片分发到每个节点，各个节点只在本地数据上运算 对应运算代码称为mapper
2. sort/combine阶段 ： 对mapper进行排序或组合
3. shuffle阶段：将map的输出作为reduce的输入的过程，MapReduce主要在这个地方优化
4. reduce阶段 : 每个mapper的输出通过某种方式组合（一般包括排序），排序后再被分成小份分发到各个节点进行下一步处理工作。对应运算代码称为reducer

------

* 主节点控制 **MapReduce** 的作业流程
* **MapReduce** 的作业可以分成 **map** 任务和 **reduce** 任务
* **map** 任务之间不做数据交流，**reduce** 任务也一样（每个节点只处理自己的事务）
* 在 **map** 和  **reduce** 阶段之间，有一个 **sort** 或 **combine** 阶段
* 数据被重复存放在不同的机器上，以防某个机器失效
* **mapper** 和 **reducer** 传输的数据形式为 **key/value** 对