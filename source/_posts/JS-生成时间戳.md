---
title: JS 生成时间戳
date: 2017-02-22 19:53:06
tags: JavaScript
---

>JS生成时间戳三种方式

```javascript
	var timestamp1 = Date.parse(new Date());
	var timestamp2 = (new Date()).valueOf();
	var timestamp3 = new Date().getTime()；
```
### 方式一
```javascript
	var timestamp1 = Date.parse(new Date());
```
    指定的日期和时间据 1970/1/1 午夜（GMT 时间）之间的毫秒数。
##### 输出
>1120752000000

### 方式二
```javascript
	var timestamp2 = (new Date()).valueOf();
```
    返回 1970 年 1 月 1 日至今的毫秒数。
##### 输出
>1120752000234

### 方式三
```javascript
	var timestamp3 = new Date().getTime()；
```
    返回 Date 对象的原始值。
##### 输出
>1120752000234
