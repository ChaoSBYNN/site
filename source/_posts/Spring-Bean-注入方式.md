---
title: Spring Bean 注入方式
date: 2025-12-22 17:51:37
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
---

Spring 容器把“对象”变成“Bean”再塞进你类里，总共只有 **4 条官方通道** + **1 个边角料**。  
记住口诀：  
“构造必备，Setter 补，字段图快，注解玩花，接口冷门。”

------------------------------------------------
1. 构造器注入（Constructor Injection）  
   Spring 官方从 4.x 起喊“首选”，强依赖不可空，一次把对象构造到合法状态。  
   ```java
   @Component
   public class OrderService {
       private final UserService userService;   // final 更安心

       public OrderService(UserService userService) { // 一个构造器可省 @Autowired
           this.userService = userService;
       }
   }
   ```
   多构造器时用 `@Autowired` 指明哪个被 Spring 调用。

------------------------------------------------
2. Setter / 方法注入（Setter Injection）  
   可选依赖、运行期可能重新设值、方便单元测试 mock。  
   ```java
   @Component
   public class OrderService {
       private UserService userService;

       @Autowired
       public void setUserService(UserService userService) {
           this.userService = userService;
       }
   }
   ```

------------------------------------------------
3. 字段注入（Field Injection）  
   写起来最爽，直接在属性上贴注解，代码少。  
   ```java
   @Component
   public class OrderService {
       @Resource
       private UserService userService;
   }
   ```
   缺点：  
   - 无法把字段设为 `final`  
   - 单元测试必须用反射或 Spring 测试容器  
   - 隐藏依赖，阅读时要跳来跳去  
   ⇒ Spring 官方文档不推荐，但项目里最常见。

------------------------------------------------
4. 注解“玩法”分支（本质还是上面三种，只是换标签）  
   - `@Autowired`：按类型注入，required=false 可空  
   - `@Resource(name="xxx")`：JSR-250，按名字先走，再走类型  
   - `@Inject`：JSR-330（javax.inject），功能≈`@Autowired`，只是完全标准注解  
   三者可以混用，但一个项目最好统一风格。

------------------------------------------------
5. 接口回调注入（冷门）  
   实现 `BeanFactoryAware`、`ApplicationContextAware`、`EnvironmentAware` 等 `*Aware` 接口，Spring 启动后会主动把容器本身回调给你。  
   ```java
   @Component
   public class ToolBox implements ApplicationContextAware {
       private static ApplicationContext ctx;
       public void setApplicationContext(ApplicationContext ac) { ctx = ac; }
   }
   ```
   属于“我要容器本身”而不是“要另一个 Bean”，日常业务代码基本用不到。

------------------------------------------------
速查表  
| 方式         | 是否推荐 | 依赖可否空 | 单元测试便利 | 备注                          |
|--------------|----------|------------|--------------|-------------------------------|
| 构造器       | ★★★★★   | 否         | 好           | 官方首选，支持 `final`       |
| Setter       | ★★★☆☆   | 可         | 中           | 可选依赖场景                  |
| 字段         | ★★☆☆☆   | 可         | 差           | 写起来快，维护哭              |
| @Inject 等   | —        | —          | —            | 只是注解换皮，归到前三类      |
| *Aware 接口  | ★☆☆☆☆   | —          | —            | 拿容器工具，非常规业务注入    |

一句话结论  
**“强依赖用构造器，可选依赖用 Setter，字段注入能不用就不用；Aware 接口只在你需要 Spring 容器本身时才碰。”**