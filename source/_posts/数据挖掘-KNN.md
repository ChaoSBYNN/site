---
title: 数据挖掘-KNN
date: 2017-03-06 21:53:20
tags: DataMining
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/datamining.jpeg"
---

> KNN 近邻算法：测量不同特征值之间距离进行分类

|属性|描述|
|:---:|:---:|
|优点|精度高，对异常值不敏感，无数据输入假定|
|缺点|计算复杂度高，空间复杂度高|
|适用|数值型，标称型|

# 步骤

1. 存在一个训练集，并且每个数据都存在标签
2. 输入没有标签的新数据
3. 将新数据与训练集特征进行比较
4. 提取训练集中特征最相似的分类标签分类


	一般只选择训练集中的前 K 个最相似数据