---
title: Java编程规范
date: 2021-04-23 14:15:55
tags: 
    - "Java"
cover: "/images/java.png"
---

# Java开发规范

`题外话 你想成为什么样的开发 你现在是什么样的开发 下面所写的不需要每句都熟读 希望即使读了其中某一段 也会有原来如此的收获即可 如果下面描述存在问题 评论署名`

<img src="/images/2021-04-23-0.png" height="300" width="1070" >

---------------
[toc]
## 前言 为什么需要编码规范[<sup>1</sup>](#refer-anchor-1)

为什么需要编码规范，借用《Java编程语言代码规范》一段开场白：

>一个软件需要花费80%的生命周期成本去维护。 　　
几乎没有任何软件的整个生命周期仅由其原作者来维护。 　　
编码规范改善软件的可读性,让工程师更快更彻底地理解新的代码。 　　
如果你将源代码转变为一个产品,那么您需要确保它和你创建的其它产品一样是干净且包装良好的。

好的代码结构和代码风格一般bug也相对少，当然除了编码规范，也少不了充分自测。[<sub>写代码一部分是完成工作用来给机器执行, 另一部分是给人使用的(我们都是创造者), 无论未来的维护这是不是你, 一段程序的健壮、扩展、灵活是很重要的一件事, 虽然它可有可无,也不会记录KPI,但是何必有杂念呢    --- Sp!ke</sub>]

## 1 如何命名

> 代码就是最好的注释[<sub>我们是如何读文章的, 你又是怎么样读懂我写的这句话的 ---Sp!ke</sub>]

### 1.1 命名

1. 如果模块、接口、类、方法使用了设计模式，在命名时体现出具体模式。
2. 变量名应该简短且有意义，并能够顾名思义。简单并不意味着越短越好，比如一个字符的变量名是不允许的，很影响代码的可读性。

[<sub>哪里是用缩写,哪里使用全拼,命名其他人能见词知义么,非英语母语的地区会有下一个问题`select get query find | selectOne getData queryInfo | findAll queryList getDatas`,是不是每个人都需一本字典才能开发,如果有是不是特定的字典 ---Sp!ke</sub>]

#### 1.1.1 命名风格

1. 代码中的命名均不能以$\color{CornflowerBlue}{下划线或美元符号}$ 开始，也不能以$\color{CornflowerBlue}{下划线或美元}$符号结束
    $\color{OrangeRed}{反例:}  $ `_name / __name / $name / name_ / name$ / name__`

1. 代码中的命名均不能包含$\color{CornflowerBlue}{魔法数字或无意义的数字}$, 但是允许有意义的
    $\color{MediumSeaGreen}{正例:}$ `timeout3s / list2Map / string2List`
    $\color{OrangeRed}{反例:}$ `table1 / i1 / a123aaa / str1`

1. 类名使用UpperCamelCase风格，但以下情形例外：DO / BO / DTO / VO / AO / PO / UID等
    $\color{MediumSeaGreen}{正例:} $ `MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion`
    $\color{OrangeRed}{反例:}$ `macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion`

4. 方法名、参数名、成员变量、局部变量都统一使用lowerCamelCase风格，必须遵从驼峰形式
    $\color{MediumSeaGreen}{正例:}$ `localValue / getHttpMessage() / inputUserId`
[<sub>其实我并不是很热衷写这种用例, 虽然它很直观,但是阿里巴巴已经写好了,感兴趣可以直接下载附件,更多的是我们每书写一段代码是否有认真思考过,有些人没有命名意识会做错,有些人不知道但是拷贝的做会正确,有些人通过经验习惯做会正确,但是有没有停下来归纳一下正确的写法有哪些通用的,这会比写出一百两百个用例更有效 ---Sp!ke</sub>]

#### 1.1.2 常量、变量命名

[<sub>常量与变量命名并没有太多的点只是罗列几点 ---Sp!ke</sub>]

变量:
1. 驼峰命名规则
1. 第一个首字母小写后续单词首字母大写
1. 减少不通用的缩写
1. 命名需要满足阅读顺序 `actorListName` -> `actorNameList`,  `sourcePrefixActorList`->`actorSourcePerfixList`
1. 不要加入无意义的魔法数字或者符号 `str1` -> `origin` , `table13` -> `mainTable`
1. 歧义的变量 例如：`logger` `LOGGER` 根据实际情况处理

常量
1. 全大写
1. 单词之间使用下划线做分割
1. 其他规则参考变量

#### 1.1.3 类命名

[<sub>类的命名是很重要的，但是不单单是命名更多的是名字就是他的职责与功能，类有两个点需要时刻注意 1.类型 2.命名 ---Sp!ke</sub>]

##### 1.1.3.1 类型

[<sub>无论下面哪种类型在编译之后都会以.class形式存在， 那么为什么又要拆分成不同类型呢，不同类型负责的职责也不同，当然你也可以混用但是不建议，失去了真正职责的类相当于，使用饭碗装排泄物 ---Sp!ke</sub>]

1. `Class` 类 : 更泛泛的通用的容器，可以理解为 normal
2. `Enum` 枚举类 : 枚举的特性在于内部的强关联，以及元素之间的规律性 `RED, BLUE, BLACK` 可以做一组枚举， 但是 `RED, NAME, JAVA`一组枚举就会莫名其妙
3. `Interface` 接口类 : 接口在Java中作用其一是弥补Java单继承的不足，作用其二是更好实现OOP的多态[<sub>我们的代码中有将常量放在interface里定义的情况，并不是说这种不可以，首先不建议因为失去了Interface的真正意义，其次如何给Interface命名呢`UserConstant`么，还是需要思考一下吧 ---Sp!ke</sub>]
4. `Annotation` 注解类 : 注解类不做太多阐述，他的使用会使程序更加灵活，但也不可控[<sub>事物都存在双面性 ---Sp!ke</sub>]

##### 1.1.3.2 命名

[<sub>我们所期待的良好的类命名是为了看到名字就知道作用 ---Sp!ke</sub>]

包名 `(packages)` 建议后面追加`s`, 例如`Utils` `Constants` `Enums`

----------------
基础层 [<sub>每一层完成他自己的事情,不要越级处理 ---Sp!ke</sub>]

1. `Controller`	$\color{CornflowerBlue}{后缀}$ Web控制层 负责请求分配, 具体交给`Service`还是重定向有具体业务判断, 建议只包含`Request` `Response`(可自定义) 数据的封装与下发 [<sub>上层、下层、底层、业务 Whatever ---Sp!ke</sub>]
1. `Service` $\color{CornflowerBlue}{后缀}$ 业务层 具体业务处理 由入口输入(`Controller` `Executer` 等进入 ) 负责加工、执行、处理调用数据层(`DAO` `Manager` `RedisTemplate` `Service`等)
1. `DAO` `Dao` $\color{CornflowerBlue}{后缀}$ 数据持久层 与持久化层对接 只处理数据的 `CURD`操作、以及数据源使用[<sub>我理解一个完整的`DAO`包含了`MySQL` `Redis` `Mongo`等等的集成 而不是单一的处理 ---Sp!ke</sub>]

-----------------
常见类命名

1. `Application` $\color{CornflowerBlue}{后缀}$ 启动类 项目唯一主入口 [<sub>我建议业务类型服务只用`Application`命名即可,不需要加入具体业务前缀, 可以规范运维启动脚本 ---Sp!ke</sub>]
1. `Config` `Configuration` $\color{CornflowerBlue}{后缀}$ 配置类
1. `Constant` $\color{CornflowerBlue}{后缀}$ 常量类 建议同一个常量类中常量具有相关性,便于管理以及类的命名 [<sub> 这个是否是常量`int timeout = 3;` ---Sp!ke </sub>]
1. `Enum` $\color{CornflowerBlue}{后缀}$ 枚举类 内部强关联 便于数据整理相当于多维`Constant`
1. `Interface` $\color{CornflowerBlue}{后缀}$ 接口 [<sub>接口是用来实现的, 失去了行为的接口是没有意义的</sub>]
1. `Base` `Common` `Super` `Abstract` $\color{CornflowerBlue}{前后缀均可} $ `Abstract` $\color{CornflowerBlue}{前缀}$父类或基础类
1. `Exception` $\color{CornflowerBlue}{后缀}$  异常类 异常类多指自定义异常
1. `Impl` $\color{CornflowerBlue}{后缀}$  实现类 与 `Interface`相关,子母关系缺一不可 使用`extends`不要加`Impl`后缀
1. `Utils`  $\color{CornflowerBlue}{后缀}$ 工具类 工具类`pulibc`修饰应加入 `static` 内部 `private`为特有
1. `Data` `Entity` `Bean` `Domain` `Model` `POJO`  $\color{CornflowerBlue}{后缀}$ 实体

[<sub>有一种说法 `Data` `Model` 修饰不需要加, 这是一种累赘的命名 类似`actorNameString` ---Sp!ke</sub>]

实体、数据对象[<sup>4</sup>] (#refer-anchor-4) [<sup>5</sup>] (#refer-anchor-5)

>	1. `Data` `Info` `Detail` [<sub>不好说没有这个规则, 只能说能看懂 ---Sp!ke</sub>]
	 1. `Entity` 就是实体的意思，所以也是最常用到的，`Entity`包中的类是必须和数据库相对应的
	 1. `Model` 最早与WEB相关, 是为页面提供数据和数据校验的
	 1. `Domain` 代表一个对象模块, 这个包国外很多项目经常用到，字面意思是域的意思, 比如一个商城的项目，商城主要的模块就是用户，订单，商品三大模块，那么这三块数据就可以叫做三个域
	 1. `POJO` `Plain OrdinaryJava Object`的缩写不错，但是它通指没有使用`Entity` `Beans`的普通java对象，可以把`POJO`作为支持业务逻辑的协助类
	 1. `VO`  `View Object` 视图对象，用于展示层，它的作用是把某个指定页面（或组件）的所有数据封装起来
	 1. `DTO` `Data Transfer Object` 数据传输对象，这个概念来源于J2EE的设计模式，原来的目的是为了EJB的分布式应用提供粗粒度的数据实体，以减少分布式调用的次数，从而提高分布式调用的性能和降低网络负载，但在这里，我泛指用于展示层与服务层之间的数据传输对象
	 1. `DO` `Domain Object` 领域对象，就是从现实世界中抽象出来的有形或无形的业务实体
	 1. `PO` `Persistent Object` 持久化对象，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系，如果持久层是关系型数据库，那么，数据表中的每个字段（或若干个）就对应PO的一个（或若干个）属性。

-----------------

对于常量`Class` `Interface`进行了一些文献查询，还是众说纷纭，只能说建议
> 常量接口模式
**The constant interface pattern is a poor use of interfaces** . That a class uses some constants internally is an implementation detail.
Implementing a constant interface causes this implementation detail to leak into the class's exported API.
It is of no consequence to the users of a class that the class implements a constant interface. In fact, it may even confuse them.
Worse, it represents a commitment: if in a future release the class is modified so that it no longer needs to use the constants, it still must implement the interface to ensure binary compatibility.
If a nonfinal class implements a constant interface, all of its subclasses will have their namespaces polluted by the constants in the interface.
There are several constant interfaces in the java platform libraries, such as  `java.io.ObjectStreamConstants` .
These interfaces should be regarded as anomalies and should not be emulated.
**原文出自see [Effective Java](http://www.oracle.com/technetwork/java/effectivejava-136174.html)**

一下连接仅供参考
- https://www.cnblogs.com/wanqieddy/p/9051568.html
- https://blog.csdn.net/voo00oov/article/details/50433672
- https://stackoverflow.com/questions/2659593/what-is-the-use-of-interface-constants
- https://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html

也有一种优雅的写法

```
public final class Constants {

    private Constants() {
        // restrict instantiation
    }

    public static final double PI = 3.14159;
    public static final double PLANCK_CONSTANT = 6.62606896e-34;
}
```

-----------------
管理类命名

1. `Pool`  $\color{CornflowerBlue}{后缀}$ 池
1. `Manager` `Mgr`  $\color{CornflowerBlue}{后缀}$ 管理器
1. `Group`  $\color{CornflowerBlue}{后缀}$ 群
1. `Proxy`  $\color{CornflowerBlue}{后缀}$ 代理类
1. `Balance`  $\color{CornflowerBlue}{后缀}$ 均衡器
1. `Container`  $\color{CornflowerBlue}{后缀}$ 容器

-----------------
创建类命名

1. `Generator`  $\color{CornflowerBlue}{后缀}$ 生成器
1. `Builder`  $\color{CornflowerBlue}{后缀}$ 构建器
1. `Factory`  $\color{CornflowerBlue}{后缀}$ 工厂

-----------------
协议通讯相关功能
1. `Msg` `Ack` `Req` `Resp`  $\color{CornflowerBlue}{前后缀均可,推荐后缀}$ 消息类
1. `Header` `Body`  $\color{CornflowerBlue}{后缀}$ 头部 主体
1. `Proto` `Protobuf`  $\color{CornflowerBlue}{后缀}$ 协议类
1. `Sender`  $\color{CornflowerBlue}{后缀}$ 发送者
1. `Receiver`  $\color{CornflowerBlue}{后缀}$ 接收者

-----------------
其他功能

1. `Listener`  $\color{CornflowerBlue}{后缀}$ 监听
1. `Filter`  $\color{CornflowerBlue}{后缀}$ 过滤器 Java中起源于Servlet
1. `Interceptor`  $\color{CornflowerBlue}{后缀}$ 拦截器 Java中起源于Spring
[<sub>从某种意义上来说 `Filter` `Interceptor` 并没有冲突 他们是两个容器的筛选 存在顺序关系 可以查看容器的结构 ---Sp!ke</sub>]
1. `Executer`  $\color{CornflowerBlue}{后缀}$ 处理器 [<sub>`Executer`与`Service`在于 `Service`可以拥有独立的规范函数 `Executer` 则具有集体特性 ---Sp!ke</sub>]
1. `Templater`  $\color{CornflowerBlue}{后缀}$ 模板
1. `Converter`  $\color{CornflowerBlue}{后缀}$ 转换器
1. `Connector`  $\color{CornflowerBlue}{后缀}$ 连接器
1. `Recorder`  $\color{CornflowerBlue}{后缀}$ 记录器
1. `Monitor`  $\color{CornflowerBlue}{后缀}$ 监控器


1. `Server`  $\color{CornflowerBlue}{后缀}$ 服务器 注意`Server`代表一个大型的容器概念 , `Service`代表某个具体的业务
1. `Client`  $\color{CornflowerBlue}{后缀}$ 终端类

-----------------
`er` `or` 尾缀代表执行者关系类似 `TaskPool` `Tasker`或者`ExecutorGroup` `Executor`
[<sub>更多的命名可以参考设计模式名称, 核心是见词知意 ---Sp!ke</sub>]
(`Singleton` `Prototype` `Adapter` `Bridge` `Decorator` `Interpreter` `Command` `Mediator` `State` `Visitor` `Strategy` `Memento` `...`)[<sup>7</sup>] (#refer-anchor-7)
> 思考: `DAO` `Dao`, `DTO` `Dto` 模棱两可的命名如何处理?


### 1.2 注释

注释有利于帮助理解代码，如果使用不当，反而会影响代码的简洁性，不利于理解代码。注释在使用上笔者认为需要坚持三个原则：

1. 保持代码干净，消除不必要的注释：好的代码本身就是最好的注释，只在必要时通过注释协助理解代码，目的是保持代码的简洁性，增强代码的可读性；
2. 区分注释和JavaDoc：类、域、方法使用JavaDoc，方法内部使用注释；
3. 注释及时更新：注释也是代码的一部分，如果代码发生变更，注释也要跟着改；

[<sub>我的理解注释的意义是补全,补全那些我们不能写成有效代码的片段、文字,也有人会指这我问为什么这个类没有一句注释, `String userLastName = "zhang";` 需要注释么 ---Sp!ke</sub>]

例:[<sup>2</sup>](#refer-anchor-2) [<sup>3</sup>] (#refer-anchor-3)

```
String unitAbbrev = "μs";									| 赞，即使没有注释也非常清晰
String unitAbbrev = "\u03bcs"; // "μs"						| 允许，但没有理由要这样做
String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"				| 允许，但这样做显得笨拙还容易出错
String unitAbbrev = "\u03bcs";								| 很糟，读者根本看不出这是什么
return '\ufeff' + content; // byte order mark					| Good，对于非打印字符，使用转义，并在必要时写上注释
```

## 2 日志
> 不管是使用何种编程语言，日志输出几乎无处不在。总结起来，日志大致有以下几种用途：
	* 问题跟踪：通过日志不仅仅包括我们程序的一些bug，也可以在安装配置时，通过日志可以发现问题。
	* 状态监控：通过实时分析日志，可以监控系统的运行状态，做到早发现问题，早处理问题。
	* 安全审计：审计主要体现在安全方面上，通过日志进行分析，可以发现是否存在非授权的操作。

### 2.1 级别

1. `fatal` - 严重的，造成服务中断的错误；
2. `error` - 其他错误运行期错误；
3. `warn` - 警告信息，如程序调用了一个即将作废的接口，接口的不当使用，运行状态不是期望的但仍可继续处理等；
4. `info` - 有意义的事件信息，如程序启动，关闭事件，收到请求事件等；
5. `debug` - 调试信息，可记录详细的业务处理到哪一步了，以及当前的变量状态；
6. `trace` - 更详细的跟踪信息；

### 2.2 基本的Logger编码规范

1. 在一个对象中通常只使用一个`Logger`对象，Logger应该是`static final`的，只有在少数需要在构造函数中传递`logger`的情况下才使用`private final`。
2. 输出`Exceptions`的全部`Throwable`信息，因为`logger.error(msg)`和`logger.error(msg,e.getMessage())`这样的日志输出方法会丢失掉最重要的StackTrace信息。

```java
		void foo() {
			try {
				// do something
			} catch (Exception e) {
				logger.error(e.getMessage()); // 错误
				logger.error("Bad things", e.getMessage()); // 错误
				logger.error("Bad things", e); // 正确
			}
		}
```

3. 不允许记录日志后又抛出异常，因为这样会多次记录日志，只允许记录一次日志。

```java
	void foo() throws Exception {
		try {
			// do something
		} catch (Exception e) {
			logger.error(e.getMessage());
			throw new Exception("Bad things", e);
		}
	}
```

4. 不允许出现System print(包括System.out.println和System.error.println)语句。

```java
	void foo() {
		try {
			// do something
		} catch (Exception e) {
			System.out.println(e.getMessage()); // 错误
			logger.error("Bad things", e); // 正确

		}
	}
```

5. 不允许出现printStackTrace。

```java
	void foo() {
		try {
			// do something
		} catch (Exception e) {
			e.printStackTrace(); // 错误
			logger.error("Bad things", e); // 正确

		}
	}
```

6. 日志性能的考虑，如果代码为核心代码，执行频率非常高，则输出日志建议增加判断，尤其是低级别的输出<`debug`、`info`、`warn`>。
	debug日志太多后可能会影响性能，有一种改进方法是：

```java
	if (logger.isDebugEnabled()) {
		logger.info("returning content: " + content);
	}
```

	但更好的方法是Slf4j提供的最佳实践:

```java
	logger.debug("returning content: " + content);
```

	一方面可以减少参数构造的开销，另一方面也不用多写两行代码。

7. 有意义的日志

通常情况下在程序日志里记录一些比较有意义的状态数据：程序启动，退出的时间点；程序运行消耗时间；耗时程序的执行进度；重要变量的状态变化。
初次之外，在公共的日志里规避打印程序的调试或者提示信息。

### 2.3 日志的输出

#### 2.3.1 什么时候输出

> 日志并不是越多越详细就越好。在分析运行日志，查找问题时，我们经常遇到该出现的日志没有，无用的日志一大堆，或者有效的日志被大量无意义的日志信息淹没，查找起来非常困难。那么什么时候输出日志呢？以下列出了一些常见的需要输出日志的情况，而且日志的级别基本都是`INFO`，至于`DEBUG`级别日志的使用场景，需要具体情况具体分析，但也是要追求“恰如其分”，不是越多越好。

1. 系统启动参数、环境变量 : 系统启动的参数、配置、环境变量、`System.Properties`等信息对于软件的正常运行至关重要，这些信息的输出有助于安装配置人员通过日志快速定位问题，所以程序有必要在启动过程中把使用到的关键参数、变量在日志中输出出来。在输出时需要注意，不是一股脑的全部输出，而是将软件运行涉及到的配置信息输出出来。比如，如果软件对jvm的内存参数比较敏感，对最低配置有要求，那么就需要在日志中将`-Xms -Xmx -XX:PermSize`这几个参数的值输出出来。
1. 异常捕获 : 在捕获异常处输出日志，大家在基本都能做到，唯一需要注意的是怎么输出一个简单明了的日志信息。这在后面的问题问题中有进一步说明。
1. 函数获得期望之外的结果时 : 一个函数，尤其是供外部系统或远程调用的函数，通常都会有一个期望的结果，但如果内部系统或输出参数发生错误时，函数将无法返回期望的正确结果，此时就需要记录日志，日志的基本通常是`WARN`。需要特别说明的是，这里的期望之外的结果不是说没有返回就不需要记录日志了，也不是说返回`false`就需要记录日志。比如函数：`isXXXXX()`，无论返回`true`、`false`记录日志都不是必须的，但是如果系统内部无法判断应该返回`true`还是`false`时，就需要记录日志，并且日志的级别应该至少是 `WARN`。
1. 关键操作 : 关键操作的日志一般是INFO级别，如果数量、频度很高，可以考虑使用DEBUG级别。以下是一些关键操作的举例，实际的关键操作肯定不止这么多。

#### 2.3.2 输出什么

1. 关键路径 : 日志并不是独立存在的,而是以一片文章阅读方式存在的
2. 关键字段 : `UID` `CID` `AID` `RequestID` 一个链路上可以通过唯一一个值,即可将整个`I/O`操作串联起来的`Keyword`, 	扩展是跨项目、跨工程之间的思考
3. 关键描述 : 时间、地点、人物、事件大家都玩过这个游戏吧



## 3 代码

> 代码! 代码! 代码!

> Error=more code^2

## 4 如何优化

## 5 如何思考

## 6 如何避免陷入(牛角尖、瓶颈、自大、自卑、思维僵化)

<video id="video" controls="" loop="loop" autoplay="autoplay" src="/images/2021-04-23-0.mp4" />


## 参考

<div id="refer-anchor-1"></div>

- [1] [浅谈Java编码规范](https://zhuanlan.zhihu.com/p/104253155)

<div id="refer-anchor-2"></div>

- [2] [Google Java编程风格指南中文版](https://www.cnblogs.com/lanxuezaipiao/p/3534447.html)

<div id="refer-anchor-3"></div>

- [3] [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)

<div id="refer-anchor-4"></div>

- [4] [实体entity、JavaBean、Model、POJO、domain的区别](https://www.cnblogs.com/qianjinyan/p/10341710.html)

<div id="refer-anchor-5"></div>

- [5] [浅析VO、DTO、DO、PO的概念、区别和用处](https://www.cnblogs.com/huangwentian/p/10526917.html)

<div id="refer-anchor-6"></div>

- [6] [设计模式与追妹子(23种设计模式巧妙解析，趣味理解)](http://www.jizhuomi.com/software/402.html)

<div id="refer-anchor-7"></div>

- [7] [设计模式|菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)

<div id="refer-anchor-8"></div>

- [8] [阿里巴巴开发手册]

<div id="refer-anchor-9"></div>

- [9] [java Log日志规范](https://my.oschina.net/xiaominmin/blog/1599733)

<div id="refer-anchor-10"></div>

- [10] [log日志输出规范](https://blog.csdn.net/Farrell_zeng/article/details/99303649)


