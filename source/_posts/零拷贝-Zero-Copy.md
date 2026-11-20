---
title: 零拷贝 Zero-Copy
date: 2026-11-20 15:45:59
tags: 
    - "算法"
    - "Program"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/helloworld.jpeg"
---

## 一、什么是零拷贝

零拷贝指是一种高效的数据传输技术, 在数据传输过程中,尽量减少或避免 CPU 对数据在用户态与内核态之间的拷贝,从而降低 CPU 开销、减少内存带宽占用、降低延迟、提高吞吐量。

这里的"零"并不是真的"一次都不拷贝",而是指减少不必要的 CPU 参与的拷贝,把数据搬移的工作尽量交给 DMA（直接内存访问）或硬件去做。

## 二、为什么会有这个问题（传统读取的拷贝开销）

以"从磁盘读文件并发送到网络"为例,传统方式（read() + write()）通常经历 4 次拷贝 + 4 次上下文切换：

```
磁盘 → ①DMA     → 内核缓冲区（page cache）
内核缓冲区 → ②CPU → 用户缓冲区
用户缓冲区 → ③CPU → 内核 socket 缓冲区
内核 socket 缓冲区 → ④DMA → 网卡
```

* ①②③④ 四次数据拷贝,其中 ② 和 ③ 是 CPU 全程参与的拷贝,非常昂贵；
* 用户态/内核态之间来回切换 4 次。

零拷贝的目标,就是去掉 ②③ 这两次 CPU 拷贝。

## 三、实现原理

### 1. mmap + write

用 `mmap()` 把内核缓冲区和用户进程的地址空间映射到同一块物理内存,省掉"内核 → 用户"的 CPU 拷贝（②）。

```
读：磁盘 →DMA→ page cache（mmap 映射后用户可直接读）
写：page cache →CPU→ socket 缓冲区 →DMA→ 网卡
```

* 相比传统方式少 1 次 CPU 拷贝,但仍需 4 次上下文切换。

### 2. sendfile（核心方案）

`sendfile()` 系统调用直接在内核态完成"文件 → socket"的传输,应用完全不需要碰数据。

```
sendfile(fd_in, fd_out)：
磁盘 →DMA→ page cache →CPU→ socket 缓冲区 →DMA→ 网卡
```

* 只需 2 次上下文切换、2 次 DMA + 1 次 CPU 拷贝。
* 真正"零拷贝"的版本：当网卡支持 SG-DMA（Scatter-Gather DMA,分散/聚集 DMA）,CPU 只把描述信息（文件位置、长度）传给 socket 缓冲区,连最后一次 CPU 拷贝也省掉,DMA 直接从 page cache 把数据搬到网卡。

```
磁盘 →DMA→ page cache（CPU 仅传递元数据）→SG-DMA→ 网卡
```

### 3. splice

Linux 2.6 引入的 `splice()`,在两个文件描述符之间建立管道,数据在内核内部搬移,不经过用户态内存。

### 4. DMA 与硬件辅助

* DMA：让外设（磁盘、网卡）直接读写内存,绕开 CPU。
* SG-DMA：支持把不连续的内存块一次性发出,是 sendfile 实现真正零拷贝的关键。
* RDMA（远程直接内存访问）：跨机器场景下,网络数据直接写入对方内存,完全绕开两端 CPU,常用于高性能计算/存储。

## 四、具体应用

| 领域           | 应用                                          | 用到的技术                                      |
| ------------ | ------------------------------------------- | ------------------------------------------ |
| **Web 服务器**  | Nginx、Apache（开启 sendfile 时）、Caddy 静态文件传输    | sendfile / mmap                            |
| **消息中间件**    | Kafka、RocketMQ 的日志与消息传输                     | mmap + sendfile / `FileChannel.transferTo` |
| **网络 IO 框架** | Netty 的 `FileRegion`（`transferTo`）、Java NIO | sendfile / splice                          |
| **大数据**      | Flink、Spark 数据落盘与 shuffle                   | mmap                                       |
| **容器与网络**    | Kubernetes CNI、DPDK 提升网络吞吐                  | DMA / RDMA                                 |
| **存储与数据库**   | 高性能存储引擎、本地缓存                                | mmap                                       |
| **TCP 代理**   | HAProxy 转发、Nginx 反向代理                       | splice / sendfile                          |


典型案例：Kafka Kafka 消费消息时,通过 Java NIO 的 FileChannel.transferTo() 直接把磁盘上的日志段数据发到网卡,底层走 sendfile,从而在极高的吞吐下保持低延迟和低 CPU 占用。

## 五、一句话总结

零拷贝的本质是：让数据尽量在内核空间 / 硬件（DMA）之间直接流动,避免 CPU 参与的用户态-内核态拷贝。典型路径是 sendfile + SG-DMA,在 Web 服务器、消息中间件（Kafka/RocketMQ）、Netty 等高性能 IO 场景中被广泛使用。