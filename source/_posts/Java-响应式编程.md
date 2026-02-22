---
title: Java 响应式编程
date: 2026-01-28 09:21:11
tags: 
    - "Java"
categories:
    - "Program"
    - "Java"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/java.png"
excerpt: "Java 编程思想 响应式编程"
---

**响应式编程（Reactive Programming）** 是一种面向**数据流**和**变化传播**的编程范式。它的核心思想是：当数据发生变化时，自动将变化传播给所有依赖它的部分。

## 核心概念

| 概念 | 解释 |
|------|------|
| **数据流（Stream）** | 随时间推移产生的一系列事件或数据 |
| **观察者模式** | 被观察者（Observable）发生变化时通知所有观察者 |
| **背压（Backpressure）** | 当生产者速度超过消费者时，防止系统过载的机制 |
| **非阻塞异步** | 不阻塞线程等待结果，通过回调处理响应 |

## 生活中的类比

> 传统编程像**打电话**：你拨号，等待对方接听，聊完才做下一件事（阻塞）
> 
> 响应式编程像**发微信**：你发送消息，不用等待回复，继续做事，收到回复时自动通知你（非阻塞）

## Java 中的实现

### 1. Project Reactor（Spring 生态标配）

```java
import reactor.core.publisher.Mono;
import reactor.core.publisher.Flux;

public class ReactiveExample {
    
    // Mono: 0 或 1 个元素的异步序列
    public Mono<User> getUser(String id) {
        return Mono.fromCallable(() -> userRepository.findById(id))
                   .subscribeOn(Schedulers.boundedElastic());
    }
    
    // Flux: 0 到 N 个元素的异步序列
    public Flux<Order> getUserOrders(String userId) {
        return getUser(userId)
            .flatMapMany(user -> Flux.fromIterable(orderRepository.findByUser(user)))
            .filter(order -> order.getStatus() == Status.ACTIVE)
            .map(this::enrichOrderData);
    }
    
    // 链式操作：声明式处理数据流
    public Flux<OrderSummary> getOrderSummaries(String userId) {
        return getUser(userId)
            .flatMapMany(user -> orderRepository.findReactiveByUserId(user.getId()))  // 异步查订单
            .flatMap(order -> itemRepository.findByOrderId(order.getId())             // 异步查详情
                .collectList()
                .map(items -> calculateTotal(order, items)))
            .onErrorResume(e -> {                                                    // 错误处理
                log.error("获取订单失败", e);
                return Flux.empty();
            })
            .subscribeOn(Schedulers.parallel());
    }
}
```

### 2. RxJava（Android 和传统 Java 常用）

```java
import io.reactivex.rxjava3.core.Observable;
import io.reactivex.rxjava3.core.Single;
import io.reactivex.rxjava3.schedulers.Schedulers;

// 网络请求 + 缓存 + 合并结果
public Observable<List<Product>> getProductsWithCache() {
    Observable<List<Product>> network = api.getProducts()
        .subscribeOn(Schedulers.io());
    
    Observable<List<Product>> cache = localDb.getProducts()
        .subscribeOn(Schedulers.io());
    
    // 先读缓存，再请求网络更新
    return Observable.concat(cache, network)
        .distinctUntilChanged();
}

// 防抖动搜索（用户停止输入 300ms 后才搜索）
public Disposable setupSearch(EditText searchBox) {
    return RxTextView.textChanges(searchBox)
        .debounce(300, TimeUnit.MILLISECONDS)      // 防抖
        .filter(text -> text.length() > 2)          // 过滤短输入
        .switchMap(text -> api.search(text))        // 取消旧请求，发起新请求
        .observeOn(AndroidSchedulers.mainThread())  // 切回主线程更新 UI
        .subscribe(results -> updateUI(results));
}
```

### 3. Java 9+ Flow API（标准库）

```java
import java.util.concurrent.Flow.*;
import java.util.concurrent.SubmissionPublisher;

// 标准库实现，但功能较基础
public class FlowExample {
    public static void main(String[] args) {
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();
        
        // 订阅者
        Subscriber<String> subscriber = new Subscriber<>() {
            private Subscription subscription;
            
            @Override public void onSubscribe(Subscription s) {
                this.subscription = s;
                s.request(1); // 背压：请求 1 个数据
            }
            
            @Override public void onNext(String item) {
                System.out.println("收到: " + item);
                subscription.request(1); // 处理完再要下一个
            }
            
            @Override public void onError(Throwable t) { t.printStackTrace(); }
            @Override public void onComplete() { System.out.println("完成"); }
        };
        
        publisher.subscribe(subscriber);
        publisher.submit("Hello Reactive");
        publisher.close();
    }
}
```

## 响应式 vs 传统编程

| 特性 | 传统编程 | 响应式编程 |
|------|---------|-----------|
| **执行方式** | 同步，阻塞等待 | 异步，非阻塞 |
| **代码结构** | 命令式（一步步指令） | 声明式（描述做什么） |
| **错误处理** | try-catch | 流式 error handler |
| **并发** | 手动管理线程 | 自动调度（Schedulers） |
| **资源利用** | 一个请求一个线程 | 少量线程处理大量请求 |

## 实际应用场景

### WebFlux（Spring 响应式 Web）

```java
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);  // 不会阻塞 Tomcat 线程
    }
    
    @GetMapping(value = "/users/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<User> streamUsers() {
        return userService.findAll()
            .delayElements(Duration.ofSeconds(1)); // 每秒推送一个用户
    }
}
```

### 实时数据处理

```java
// 股票价格实时流处理
public Flux<StockPrice> analyzeStockPrices() {
    return stockPriceStream
        .window(Duration.ofSeconds(5))           // 每 5 秒一个窗口
        .flatMap(window -> window.collectList())
        .map(prices -> calculateMovingAverage(prices))
        .filter(avg -> avg.getChangePercent() > 5)  // 只关注波动 > 5%
        .doOnNext(alert -> sendNotification(alert));
}
```

## 关键优势

1. **高并发**：少量线程处理大量连接（Netty 模式）
2. **资源效率**：避免线程阻塞等待 I/O
3. **实时性**：数据到达立即处理，无需轮询
4. **可组合性**：通过操作符（map/filter/flatMap 等）组合复杂逻辑

## 适用场景

- ✅ 高并发 Web 服务（网关、API 聚合）
- ✅ 实时数据流（IoT、股票行情、日志处理）
- ✅ 微服务间异步通信
- ❌ 简单 CRUD（过度设计）
- ❌ 复杂事务处理（响应式事务管理较复杂）

响应式编程的本质是**以数据为中心，让数据的变化自动驱动程序的执行**，而不是人工控制执行流程。