---
title: Spring 异步注解 @Async
date: 2024-06-23 07:48:55
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
---

## Why

在 Java 中，当我们需要执行异步操作时，往往会去创建一个新线程去执行，如下：

```java
public class App {
    public static void main( String[] args ) {
        new Thread(() -> {
            System.out.println(Thread.currentThread().getName() + "：异步任务");
        }).start();
    }
}
```

Spring 3.0 之 后提供了一个 `@Async`注解，使用 `@Async` 注解进行优雅的异步调用。
其实，`@Async`注解本质上还是通过线程池创建线程去异步执行任务

## 使用

### 开启 `@Async`

使用 `@Async` 注解步骤：
1. 添加 `@EnableAsync` 注解。在主类上或者 某个类上，否则，异步方法不会生效
2. 添加 `@Async` 注解。在异步方法上添加此注解。异步方法不能被 `static` 修饰
3. 需要自定义线程池，则可以配置线程池（下文有）

### 使用`@Async`

`@Async`注解可以应用于任何Spring Bean（通常是Service层的方法）的方法声明上，指示该方法应该在一个单独的线程中异步执行：
```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class AsyncService {

    @Async
    public void asyncMethod() {
        // 这个方法将在一个独立的线程中执行
        // 执行耗时的操作，如数据库查询、网络请求等
    }
}
```
> ❗ 注意，被`@Async`标注的方法必须是void类型的，且不能有返回值，除非返回类型是Future，这样可以通过Future获取异步操作的结果。

### `@Async`方法的异常处理
由于异步方法是在后台线程中执行的，因此抛出的异常不会立即中断主线程的执行。为了捕获和处理这些异常，可以利用`@Async`注解所在方法所在的类上的`@AsyncExceptionHandler`方法：

```java
@Service
public class AsyncService {

    @Async
    public void asyncMethod() {
        // 可能抛出异常的异步代码
    }

    @AsyncExceptionHandler
    public void handleAsyncException(Exception ex) {
        // 处理异步方法中抛出的异常
    }
}
```
综上所述，Spring的`@Async`注解极大地简化了异步编程模型，使得开发者能够方便地实现异步任务调度，提高系统并发处理能力和用户体验。同时，合理配置线程池并妥善处理异步任务中可能出现的异常，也是保障系统稳定性和健壮性的重要环节。

## 自定义`@Async` 注解的线程池

### 方法 1 使用`AsyncConfigurer`指定线程池

`AsyncConfigurer`接口是Spring框架用于全局配置异步执行器（即线程池）的核心接口。当我们的Spring应用需要统一管理所有异步任务的执行环境时，可以选择实现此接口。

```java
@Configuration  
@EnableAsync  
public class GlobalAsyncConfig implements AsyncConfigurer {
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5); // 核心线程数
        executor.setMaxPoolSize(10); // 最大线程数
        executor.setQueueCapacity(20); // 队列容量
        executor.setThreadNamePrefix("global-"); // 线程名称前缀
        executor.initialize();
        return executor;
    }
}
```

在此示例中，`GlobalAsyncConfig`类实现了`AsyncConfigurer`接口，并在`getAsyncExecutor()`方法中配置了一个全局的线程池。这意味着，对于应用中所有标记为`@Async`的方法，默认都会使用这个配置好的线程池执行异步任务。

```java
@Service  
public class MyService {
    @Async
    public void executeGlobalTask() {
        // 此方法将使用GlobalAsyncConfig中配置的线程池执行
    }
}
```

### 方法 2 指定使用自定义的线程池 `Excutor` 实例 Bean

在Spring容器中注册一个线程池Bean，这种方式允许你根据业务需求更加灵活地管理和分配不同的线程池资源。

```java
@Configuration  
public class CustomThreadPoolConfig {
    @Bean(name = "customExecutor")
    public ThreadPoolTaskExecutor customExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(3);
        executor.setMaxPoolSize(5);
        executor.setQueueCapacity(10);
        executor.setThreadNamePrefix("custom-");
        executor.initialize();
        return executor;
    }
}
```

现在，我们可以明确地将特定的线程池Bean与某个异步方法关联起来：

```java
@Service  
public class MyService {
    @Async("customExecutor")
    public void executeCustomTask() {
        // 此方法将使用CustomThreadPoolConfig中名为customExecutor的线程池执行
    }
}
```

通过在`@Async`注解中指定`customExecutor`，系统将优先使用这个名字注册在Spring容器中的线程池，而不是全局配置的线程池。

### Spring 的默认线程池配置

1. 注意早期版本的Spring Boot环境中，如果用户没有自定义配置异步执行器（Async Executor），并且没有实现`AsyncConfigurer`接口来提供一个自定义的执行器，那么Spring Boot会使用一个默认的异步执行器，而在某些早期版本或特定配置下，这个默认执行器可能是`SimpleAsyncTaskExecutor`，这是个不重用线程、无界并发的执行器。每个提交的任务创建一个新的线程来执行。这意味着每次调用都会创建新的线程资源，而不从固定大小的线程池中获取可用线程。
2. 在后期版本中，如果没有 `Executor` 的实例 Spring Boot将会使用其默认配置的线程池（名称为 taskExecutor）来执行被`@Async`注解修饰的异步方法。
3. 在Spring Boot如果不存在 Excutor Bean 会通过`TaskExecutionAutoConfiguration`，它会自动配置一个基于`ThreadPoolTaskExecutor`的默认线程池，取名为`applicationTaskExecutor` 和 `taskExecutor` 进行自动配置。如果已经自定义了Executor bean 那么`applicationTaskExecutor`将不会自动配置。
4. 这个默认线程池的相关配置通常基于Spring Boot的默认属性这些属性可以根据应用的具体需求，在`application.properties`或`application.yml`文件中进行调整。例如：
  a. `spring.task.execution.pool.core-size`：核心线程数，默认值可能依赖于具体版本，一般较小。
  b. `spring.task.execution.pool.max-size`：最大线程数，默认值也可能因版本不同而变化。
  c. `spring.task.execution.pool.queue-capacity`：线程池的工作队列容量。
  d. `spring.task.execution.pool.keep-alive`：空闲线程的存活时间。

## 失效场景
> 💡 `@Async`注解基于 Spring AOP 动态代理实现

### 调用者与被调用者在同一个类中

#### 问题

当调用 `@Async`注解 的方法的类和被调用的方法在同一个类中时，`@Async` 注解不会生效。因为 Spring 的 AOP 代理是基于接口的，对于同一个类中的方法调用，不会经过代理，因此 `@Async` 注解不会被处理。例如：

```java
@Service  
public class MyService {  
  
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
  
    public void callAsyncMethod() {  
        asyncMethod(); // 直接调用，不会异步执行  
    }  
}
```

#### 解决方案

1. 确保异步方法和调用它的方法不在同一个类中。可以将异步方法提取到一个单独的 Service 中，并在需要的地方注入这个 Service。
2. 确保异步方法的执行类（即包含 `@Async` 注解方法的类）被 Spring 容器管理，比如通过 `@Service`、`@Component` 等注解标注

```java
// 一定使用@Service、@Component 等注解标注，确保执行类被Spring管理，
// 因为异步线程是通过动态代理实现的

@Service
public class AsyncService {
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
}
```

### 配置类未启用异步支持

#### 问题

如果配置类中没有启用异步支持，即没有使用 `@EnableAsync` 注解，那么 `@Async` 注解同样不会生效。

```java
// 没有使用 @EnableAsync 注解，因此不会启用异步支持  
@Configuration  
public class AsyncConfig {  
    // ... 其他配置 ...  
}  

@Service  
public class MyService {  
  
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
}
```

#### 解决方案

1. 在配置类上使用 `@EnableAsync` 注解，启用异步支持。
```java
@Configuration
@EnableAsync
public class AsyncConfig {  
    // ... 其他配置 ...  
}
```
### 方法不是 public 的

#### 问题

`@Async` 注解的方法必须是 `public` 的，否则不会被 Spring AOP 代理捕获，导致异步执行不生效。

```java
@Service  
public class MyService {  
  
  // 但这个方法不是 public 的，所以 @Async 不会生效  
    @Async 
    protected void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
  
    public void callAsyncMethod() {  
        asyncMethod(); // 直接调用，但由于 asyncMethod 不是 public 的，因此不会异步执行  
    }  
}
```

#### 解决方案

● 确保异步方法是 `public` 的

### 线程池未正确配置

#### 问题

在使用 `@Async` 注解时，如果没有正确配置线程池，可能会遇到异步任务没有按预期执行的情况。例如，线程池被配置为只有一个线程，且该线程一直被占用，那么新的异步任务就无法执行。

```java
@Configuration  
@EnableAsync  
public class AsyncConfig implements AsyncConfigurer {  
  
    @Override  
    public Executor getAsyncExecutor() {  
        // 创建一个只有一个线程的线程池，这会导致并发问题  
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();  
        executor.setCorePoolSize(1);  
        executor.setMaxPoolSize(1);  
        executor.setQueueCapacity(10);  
        executor.setThreadNamePrefix("Async-");  
        executor.initialize();  
        return executor;  
    }  
  
    // ... 其他配置 ...  
}
  
@Service  
public class MyService {  
  
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
}
```

#### 解决方案

● 正确配置线程池：确保线程池配置合理，能够处理预期的并发任务量

### 异常处理不当

#### 问题

如果在异步方法中抛出了异常，并且没有妥善处理，那么这个异常可能会导致任务失败，而调用者可能无法感知到异常的发生。

```java
@Service  
public class MyService {  
  
    @Async  
    public void asyncMethod() {  
        // 模拟一个可能会抛出异常的耗时操作  
        throw new RuntimeException("Async method exception");  
    }  
}
// 调用者  
@Service  
public class CallerService {  
  
    @Autowired  
    private MyService myService;  
  
    public void callAsyncMethod() {  
        myService.asyncMethod(); // 调用异步方法，但如果该方法抛出异常，调用者不会立即感知到  
    }  
}
```

#### 解决方案

● 合理处理异常：在异步方法中妥善处理异常，可以通过 `Future` 对象来捕获异步任务执行过程中抛出的异常。

### Spring代理未生效

#### 问题

如果通过 `new` 关键字直接创建了服务类的实例，而不是通过 Spring 容器来获取，那么 Spring 的 AOP 代理将不会生效，导致 `@Async` 注解无效。

```java
@Service  
public class MyService {  
  
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
}  
  
public class SomeNonSpringClass {  
  
    public void someMethod() {
        // 直接通过 new 创建 MyService 实例，不会经过 Spring 代理  
        MyService myService = new MyService(); 
        myService.asyncMethod(); // 这里 `@Async` 不会生效  
    }  
}
```

#### 解决方案

● 合理利用依赖注入：始终通过 Spring 容器来获取服务类的实例，而不是直接通过 `new` 关键字创建

```java
@Service  
public class MyService {  
    @Async  
    public void asyncMethod() {  
        // 模拟耗时操作  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        System.out.println("Async method executed.");  
    }  
}  
@Service
public class SomeNonSpringClass {  
    @Autowired  
    private MyService myService;  
    public void someMethod() {   
        myService.asyncMethod(); // 这里 `@Async` 会生效  
    }  
}
```

### 使用 `@Transactional` 与 `@Async` 同时注解方法，导致事务失效

#### 问题

在同一个方法上同时使用 `@Transactional` 和 `@Async` 注解可能会导致问题。由于 `@Async` 会导致方法在一个新的线程中执行，而 `@Transactional` 通常需要在一个由 Spring 管理的事务代理中执行，这两个注解的结合使用可能会导致事务管理失效或行为不可预测。此种场景不会导致`@Async`注解失效，但是会导致`@Transactional`注解失效，也就是事务失效。例如：

```java
@Service  
public class MyService {  
  
    @Autowired  
    private MyRepository myRepository;  
  
    // 错误的用法：同时使用了 @Transactional 和 `@Async`  
    @Transactional  
    @Async  
    public void asyncTransactionalMethod() {  
        // 模拟一个数据库操作  
        myRepository.save(new MyEntity());  
          
        // 模拟可能抛出异常的代码  
        if (true) {  
            throw new RuntimeException("Database operation failed!");  
        }  
    }  
}  
  
@Repository  
public interface MyRepository extends JpaRepository<MyEntity, Long> {  
    // ...  
}  
  
@Entity  
public class MyEntity {  
    // ... 实体类的属性和映射 ...  
}
```

上面的代码，在抛出异常的时候，我们期望的是回滚前面的数据库保存操作，但是因为事务失效，会导致错误数据成功保存进数据库。

#### 解决方案

● 正确配置事务，比如单独提取事务执行的逻辑到一个新的Service里，事务执行方法单独使用`@Transactional`标识

```java
@Service  
public class MyService {  
  
    @Autowired  
    private MyTransactionalService myTransactionalService;  
  
    @Autowired  
    private AsyncExecutor asyncExecutor;  
  
    public void callAsyncTransactionalMethod() {  
        // 在事务中执行数据库操作  
        MyEntity entity = myTransactionalService.transactionalMethod();  
          
        // 异步执行其他操作  
        asyncExecutor.execute(() -> {  
            // 这里执行不需要事务管理的异步操作  
            // ...  
        });  
    }  
}  
  
@Service  
public class MyTransactionalService {  
  
    @Autowired  
    private MyRepository myRepository;  
  
    @Transactional  
    public MyEntity transactionalMethod() {  
        // 在事务中执行数据库操作  
        return myRepository.save(new MyEntity());  
    }  
}  
  
@Component  
public class AsyncExecutor {  
  
    @Async  
    public void execute(Runnable task) {  
        task.run();  
    }  
}
```