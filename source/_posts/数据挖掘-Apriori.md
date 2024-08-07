---
title: 数据挖掘-Apriori
date: 2017-03-16 22:13:14
tags: DataMining
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/datamining.jpeg"
---

> 从海量数据中挖掘可信频繁项集

|属性|描述|
|:---:|:---:|
|优点|易编码实现|
|缺点|大数据集运行缓慢|
|适用|数值型，标称型|

# 名词概念

|名词|意义|
|:---:|:---:|
|association analysis|关联分析|
|association rule learning|关联规则学习|
|frequent item set|频繁项集|
|association rule|关联规则|
|frequent|频繁|
|support|支持度|
|confidence|可信度|

	a priori 一个先验 使用知识作为条件进行推断
	a postriori 一个后验 使用结果作为条件检测

# 原理

如果某个项集是频繁项集，那么他的子项集也是频繁项集。反之，如果一个项集是非频繁项集，那么他的所有超集也是非频繁项集。

1. 收集数据
2. 准备数据
3. 分析数据
4. 训练算法
5. 测试算法
6. 使用算法

------

* 首先发现频繁项集
*  然后计算出关联规则

### 生成候选项集

* 记录训练数据集中每一条数据为候选项集
* 检查每一个候选项集属于数据集子集的个数
* 对于候选项集如果最小值不小于支持度，则保留该项集
* 返回所有频繁项集