---
title: Java 多线程实现方式
date: 2019-07-13 19:52:35
tags: 
    - "Java"
    - "多线程"
cover: "/images/java.png"
---

### 继承 Thread
```
    ... extends Thread {
        @Override
        public void run() {
            // run 
        }
    }
```
```
    class.start();
```

### 实现 Runnable
```
    ... implements Runnable {
        @override
        public void run() {
            // run
        }
    }
```
```
    class.start();
```

### 使用 Executor (ExecutorService Callable Future)
