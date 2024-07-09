---
title: Java 引用类型 Reference
date: 2024-07-08 09:02:31
tags: 
    - "Java"
categories:
    - "Program"
    - "Java"
cover: "/images/java.jpg"
excerpt: "强引用, 软引用, 弱引用, 虚引用"
---

![Reference](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/reference/reference_1.webp)
![Reference](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/reference/reference_2.webp)

## 引用

> 每一种语言都有着自己操作内存元素的方式，C语言通过指针，而java就是通过引用。作为一门面向对象的语言，在java中世事万物皆对象。但是我们操作的标识符实际上是对象的一个引用 reference。今天我们来分析一下java中的四种引用。

### 引用的历史

在Java中，我们的垃圾回收机制回收垃圾对象的时候就会依据对象的引用。比如说通过不同的垃圾回收算法，这里有两种：引用计数法和可达性分析法

1. 引用计数法：为每个对象添加一个引用计数器，每当有一个引用指向它时，计数器就加1，当引用失效时，计数器就减1，当计数器为0时，则认为该对象可以被回收。
![Reference](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/reference/reference_3.webp)
2. 可达性分析算法：从一个被称为 GC Roots 的对象开始向下搜索，如果一个对象到GC Roots没有任何引用链相连时，则说明此对象不可用。
![Reference](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/reference/reference_4.webp)

目前的垃圾回收基本上采用第二种方式，第一种方式基本上已经被舍弃了。
在jdk1.2之前，java对引用的概念只有“已被引用”和"未被引用"两种状态。后来所以在 JDK.1.2 之后，Java 对引用的概念进行了扩充，将引用分为了：强引用、软引用、弱引用、虚引用4 种，也就是我们今天所讲的主题。这 4 种引用的强度依次减弱。

## Java 有四种引用类型：

1. 强引用
2. 软引用
3. 弱引用
4. 虚引用

四种引用的级别由高到低依次为：强引用 > 软引用 > 弱引用 > 虚引用。

### 强引用

在 Java 中最常见的就是强引用， 把一个对象赋给一个引用变量，这个引用变量就是一个强引用。当一个对象被强引用变量引用时，它处于可达状态，它是不可能被垃圾回收机制回收的，即使该对象以后永远都不会被用到 JVM 也不会回收，因此强引用是造成 Java 内存泄漏的主要原因之一。所以要想回收该对象，则应该将指向该对象的变量显示设为 null，这样该对象就由强引用转变为无引用了。

```java
// 强引用
String str = new String("abc"); 

// 帮助垃圾收集器回收此对象
str = null;
```

###  软引用

软引用需要用 SoftReference 类来实现，对于只有软引用的对象来说，当系统内存足够时它不会被回收，当系统内存空间不足时它会被回收。软引用通常用在对内存敏感的程序中。

```java
String str = new String("abc"); 
// 软引用
SoftReference<String> softRef = new SoftReference<String>(str);
```

### 弱引用

弱引用需要用 WeakReference 类来实现，它比软引用的生存期更短，对于只有弱引用的对象来说，只要垃圾回收机制一运行，不管 JVM 的内存空间是否足够，总会回收该对象占用的内存。弱引用也可以用于缓存，可以参考 WeakHashMap 类。

```java
String str = new String("abc");
// 弱引用
WeakReference<String> weakRef = new WeakReference<String>(str);
```

### 虚引用

虚引用需要 PhantomReference 类来实现，它不能单独使用，必须和引用队列联合使用。 虚引用的主要作用是跟踪对象被垃圾回收的状态。

```java
String str = new String("abc");
//创建引用队列
ReferenceQueue<String> refQueue = new ReferenceQueue();
//创建的虚引用
PhantomReference<String> phantomRef = new PhantomReference<>(str, refQueue);
//phantomRef.get()总是返回null
```

---

|引用类型|阻止GC?|回收时机|用途|
|---|---|---|----|
|强引用|	是|	显式null化后|	普通对象引用|
|软引用|	否|	内存不足时|	内存敏感的缓存|
|弱引用|	否|	随时可能|	非必须的对象引用，允许随时回收|
|虚引用|	否|	随时可能，对象即将被回收|	跟踪对象被垃圾收集器回收的活动|
|GC Roots|	否|	从不回收|	作为垃圾收集器的起始点|