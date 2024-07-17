---
title: Spring  Boot  Cloud 前世今生
date: 2024-07-17 12:47:01
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
    - "Java"
    - "Spring"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
excerpt: "Spring | Boot | Cloud 的前世今生"
---

2004年3月24日，Spring官网发布了Spring Framework的1.0版本，在一定程度上颠覆了Java开发的模式。Spring Framework提供的控制反转和依赖注入，这两个概念的提出也是受到了JNDI和EJB的容器注入的影响。换句话说，Spring Framework其实也是在J2EE的基础上又建了一个轮子而已。从当时的分层来看，Struts为web表示层、Hibernate为持久层，而Spring Framework就是把大家伙整合到一起的那么个作用。此时的Spring Framework还有一个致命的问题，Spring Framework需要随着Java容器的启动而装载。
那么在Spring Framework4.0发布的时候，Spring还同时发布了Spring Boot。官方是这么定义Spring Boot的。

> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
>
> We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need minimal Spring configuration.

Spring Boot使构建独立的Spring生产级别的项目变得简单，同时提供了固化视图来固化Spring Boot所依赖的Spring平台库和第三方类库，从而减少管理类库的烦恼。大多数Spring Boot项目只需要少量的Spring配置。

1. Create stand-alone Spring applications
2. Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
3. Provide opinionated 'starter' dependencies to simplify your build configuration
4. Automatically configure Spring and 3rd party libraries whenever possible
5. Provide production-ready features such as metrics, health checks, and externalized configuration
6. Absolutely no code generation and no requirement for XML configuration

官网中也同时给出了Spring Boot的特性，分别是：

1. 创建独立的Spring应用, 使用 Spring 项目引导页面可以在几秒构建一个项目
2. 支持运行期内嵌容器, 直接嵌入Tomcat、Jetty或Undertow（不需要部署WAR文件）；
3. 提供固化的“Starter”依赖，简化构建的配置步骤；
4. 自动装配Spring或第三方类库；
5. 提供运维特性，比如指标信息、健康检查和配置外部化, 自动管理依赖, 自带应用监控
6. 绝无代码生成，而且不需要配置XML文件, 非常简洁的策略集成

从官网的说明中，我们不难看出，Spring Boot的优点：

> 使编码变的简单
>
> 使配置变的简单
>
> 使部署变的简单
>
> 使监控变的简单

## 前世今生

### 过去

时间回到2002年，当时正是 Java EE 和 EJB 大行其道的时候，很多知名公司都是采用此技术方案进行项目开发。 EJB 太过臃肿，并不是所有的项目都需要使用 EJB 这种大型框架，应该会有一种更好的方案来解决这个问题。

做Spring项目开发时，程序员要知道配置哪些类来让Hibernate和Spring一起工作，还要知道如何配置view resolver来控制哪个模版进行视图层的展示。写了一大堆代码其实只是在处理Spring框架本身的配置，此时一行业务逻辑都没有写。开发完成之后，还要考虑部署的问题，比如如何配置容器，如何修改配置文件等等。而且在多应用部署到同一个Tomcat的时候，经常会出现冲突。就算花了很大力气解决了这些问题，程序部署成功之后，也很难去了解这个程序的运行状态。那么还要配置第三方工具来了解应用程序运行状态，参数，环境变量。

尽管Spring帮我们解决了依赖注入的问题，简化了MVC的流程，但是Spring框架本身集成了越来越多东西，导致其越来越难配置，维护成本成直线上升。

### 现在

Spring Boot使用“约定优于配置”的理念让项目快速运行起来。简单描述就是项目中存在大量的配置，此外还内置一个习惯性的配置，让我们无须手动进行配置。使用Spring Boot 很容易创建一个独立运行、准生产级别的Spring项目，而且我们几乎不用怎么进行配置。

当然 Spring Boot 不是为了取代 Spring ,Spring Boot 基于 Spring 开发，是为了让人们更容易的使用 Spring。看到 Spring Boot 的市场反应，Spring 官方也非常重视 Spring Boot 的后续发展，已经将 Spring Boot 作为公司最顶级的项目来推广，放到了官网上第一的位置，因此后续 Spring Boot 的持续发展也被看好。

## 约定优于配置

约定优于配置（Convention Over Configuration），也称作按约定编程，是一种软件设计范式，旨在减少软件开发人员需做决定的数量、获得简单的好处，而又不失灵活性。

本质上说，开发人员仅需规定应用中不符约定的部分。例如，如果模型中有个名为 User 的类，那么数据库中对应的表就会默认命名为 user。只有在偏离这一约定时，例如将该表命名为“legal_user”，才需写有关这个名字的配置。

我们可以按照这个思路来设想，我们约定 Controller 层就是 Web 请求层可以省略 MVC 的配置；我们约定在 Service 结尾的类自动注入事务，就可以省略了 Spring 的切面事务配置。

在 Spring 体系中，Spring Boot JPA 就是约定优于配置最佳实现之一，不需要关注表结构，我们约定类名即是表名，属性名即是表的字段，String 对应 varchar，long 对应 bigint，只有需要一些特殊要求的属性，我们再单独进行配置，按照这个约定我们可以将以前的工作大大简化。
Spring Boot 体系将约定优于配置的思想展现得淋漓尽致，小到配置文件、中间件的默认配置，大到内置容器、生态中的各种 Starters 无不遵循此设计规则。Spring Boot 鼓励各软件组织方创建自己的 Starter，创建 Starter 的核心组件之一就是 autoconfigure 模块，也是 Starter 的核心功能，在启动的时候进行自动装配，属性默认化配置。

可以说正是因为 Spring Boot 简化了配置和众多的 Starters 才让 Spring Boot 变得简单、易用、快速上手。Spring Boot 约定优于配置的思想让 Spring Boot 项目非常容易上手。

### 体现

1. 项目结构约定
    * `src.main.java` 存放源代码文件
    * `src.main.resource` 存放资源文件
    * `src.test.java` 测试代码
    * `src.test.resource` 测试资源文件
    * `target` 编译后的 class 文件和 jar 文件
2. 内置了嵌入式的 Web 容器, 支持四种嵌入式的 Web 容器
    * Tomcat
    * Jetty
    * Undertow
    * Reactor
3. 自动装配
    * 自动装配的核心是 `@EnableAutoConfiguration` 注解，它会自动扫描和导入符合条件的依赖
    * 自动装配的核心是 `@ComponentScan` 注解，它会扫描和导入符合条件的组件
4. 默认提供了两种配置文件
    * `application.properties` 项目的默认配置文件
    * `application.yml` 项目的默认配置文件
5. 内置了多种 Starter
    * Spring Boot 项目中默认包含了大量的 Starter，我们可以直接使用这些 Starter 来简化 Spring Boot 的配置

## Spring Boot 都市传说

### 一切一切的起点

说起 Spring Boot 我们不得不先了解一下 Spring 这个企业，不仅因为 Spring Boot 来源于 Spirng 大家族，而且 Spring Boot 的诞生和 Sping 框架的发展息息相关。

2002 年 10 月，Rod Johnson 撰写了一本名为 Expert One-on-One J2EE 设计和开发的书。本书当时由 Wrox出版社出版，介绍了当时 Java 企业应用程序开发的情况，并指出了 Java EE 和 EJB 组件框架中的存在的一些主要缺陷。在这本书中，他提出了一个基于普通 Java 类和依赖注入的更简单的解决方案。（这里就对应了我们开篇讲到的，依赖注入并不是Spring发明的，而是在J2EE原来的基础上做了优化改进）

在书中，他展示了如何在不使用 EJB 的情况下构建高质量，可扩展的在线座位预留系统。为了构建应用程序，他编写了超过 30,000 行的基础结构代码。包含许多可重用的 Java 接口和类，如 `ApplicationContext和BeanFactory` 。由于java接口是依赖注入的基本构建块，因此他将这些类的根包命名为`com.interface21`。所以人们最初称这套开源框架为 interface21，也就是 Spring 的前身。

Rod Johnson 在悉尼大学不仅获得了计算机学位，同时还获得了音乐学位，更令人吃惊的是在回到软件开发领域之前，他还获得了音乐学的博士学位。现在 Rod Johnson 已经离开了 Spring ，成为了一个天使投资人，同时也是多个公司的董事，早已走上人生巅峰。

### Spring 诞生

在本书发布后不久，开发者 Juergen Hoeller 和 Yann Caroff 说服 Rod Johnson 创建一个基于基础结构代码的开源项目。Rod，Juergen 和 Yann 于 2003 年 2 月左右开始合作开发该项目 。Yann 为新框架创造了“Spring”的名字。

2004 年 3 月，1.0 版发布。有趣的是，在1.0发布之前，spring 就被开发人员广泛采用。2004 年 8 月，Rod Johnson，Juergen Hoeller，Keith Donald 和Colin Sampaleanu 共同创立了一家专注于 Spring 咨询，培训和支持的公司： interface21。

Yann Caroff 在很早离开了团队，Rod Johnson 在 2012 年离开，Juergen Hoeller 目前仍然还在团队中进行开发工作。自 2004 年 1.0 版本发布以来，Spring 框架迅速发展。Spring 2.0 于 2006 年 10 月发布，Spring的下载量超过了 100 万。Spring 2.0 具有可扩展的 XML 配置功能，用于简化 XML 配置，支持 Java 5，额外的 IoC 容器扩展点，支持动态语言。

在 Rod 领导下管理 Interface21 项目于 2007 年 11 月更名为 SpringSource。同时发布了 Spring 2.5。Spring 2.5 中的主要新功能包括支持 Java 6 / Java EE 5，支持注释配置，classpath 中的组件自动检测和兼容 OSGi 的 bundle。

2007 年，SpringSource 从基准资本获得了 A 轮融资（1000万美元）。SpringSource 在此期间收购了多家公司，如Hyperic，G2One 等。2009年8月，SpringSource 以 4.2 亿美元被 VMWare 收购。SpringSource 在几周内收购了云代工厂，这是一家云 PaaS 提供商。2015 年，云代工厂转型成了非营利云代工厂。

2009 年 12 月，Spring 3.0 发布。Spring 3.0 具有许多重要特性，如重组模块系统，支持 Spring 表达式语言，基于 Java 的 bean 配置（JavaConfig），支持嵌入式数据库（如 HSQL，H2 和 Derby），模型验证/ REST 支持和对 Java EE 的支持。

2011 年和 2012 年发布了许多 3.x 系列的小版本。2012 年 7 月，Rod Johnson 离开了团队。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/20240717130324.png)

```markdown
mermaid
timeline
    title Spring TL
    2002 : 2002 年 10 月 : Rod Johnson 撰写《Expert One-on-One J2EE》一书，介绍了 Spring 框架的早期思想
    2003 : 2003 年 2 月 : Juergen Hoeller 和 Yann Caroff 说服 Rod Johnson 创建一个基于基础结构代码的开源项目 : Yann 为新框架创造了“Spring”的名字
    2004 : 2004 年 3 月 : Spring 1.0 版发布
    2004 : 2004 年 8 月 : Rod Johnson、Juergen Hoeller、Keith Donald 和 Colin Sampaleanu 共同创立了一家专注于 Spring 咨询、培训和支持的公司 `interface21`
    2006 : 2006 年 10 月 : Spring 2.0 发布
    2007 : 2007 年 11 月 : Interface21 项目更名为 SpringSource
    2009 : 2009 年 12 月 : Spring 3.0 发布
    2011 : 2011 年 : 发布了许多 3.x 系列的小版本
    2012 : 2012 年 7 月 : Rod Johnson 离开了团队
    2014 : 2014 年 12 月 : Pivotal 宣布发布 Spring 4.0
    2017 : 2017 年 09 月 : Spring 5.0 发布
```

### Spring Boot诞生

2012 年 10 月，Mike Youngstrom 在 Spring jira 中创建了一个功能请求 ， 要求在 Spring 框架中支持无容器 Web 应用程序体系结构。他谈到了在主容器引导 spring 容器内配置 Web 容器服务。（画重点，Spring Boot 的苗头发生了）

2013 年 4月，VMware 和 EMC 通过 GE 投资创建了一家名为 Pivotal 的合资企业。所有的 Spring 应用项目都转移到了 Pivotal。

2013 年 12 月，Pivotal 宣布发布 Spring 框架 4.0。Spring 4.0 是 Spring 框架的一大进步，它包含了对Java 8 的全面支持，更高的第三方库依赖性（groovy 1.8+，ehcache 2.1+，hibernate 3.6+等），Java EE 7 支持，groovy DSL for bean 定义，对 websockets 的支持以及对泛型类型的支持作为注入 bean 的限定符。

2014 年 4 月，Spring Boot 1.0.0 发布。改进的模板支持，gemfire 支持，Elastic Search 和 Apache Solr 的自动配置。

2015 年 3 月，Spring Boot 1.2 发布。升级到 servlet 3.1 / tomcat 8 / jetty 9，spring 4.1 升级，支持 banner / jms / SpringBootApplication 注解。

2016 年 12 月，Spring Boot 1.3发布新的 spring-boot-devtools，用于缓存技术（ehcache，hazelcast，redis 和 infinispan）的自动配置以及完全可执行的 jar 支持。于此同时，Spring 4.2 升级。

2017年1月，Spring Boot 1.4 支持 couchbase / neo4j，分析启动失败和RestTemplateBuilder。于此同时Spring 4.3 升级。

2017年2月，Spring Boot 1.5 支持 kafka / ldap，第三方库升级，弃用 CRaSH 支持和执行器记录器端点以动态修改应用程序日志级别。

2014 年至 2017 年期间发布了许多 Spring 框架 4.xx 系列版本。Spring 4.3.8 于 2017 年 4 月发布，并成为 4.x 系列中的最后一个。

2018 年 03 月，Spring Boot 2.0 基于 Java 8，支持 Java 9，支持 Quartz ，调度程序大大简化了安全自动配置，支持嵌入式 Netty。于此同时，Spring 5.0发布。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/20240717130241.png)

```markdown
mermaid
timeline
    title Spring Boot TL
    2012 : 2012 年 10 月 : Mike Youngstrom 在 Spring jira 中创建了一个功能请求 : Spring Boot 的苗头发生了
    2013 : 2013 年 4月 : VMware 和 EMC 通过 GE 投资创建了一家名为 Pivotal 的合资企业。所有的 Spring 应用项目都转移到了 Pivotal
    2013 : 2013 年 12 月 : Pivotal 宣布发布 Spring 框架 4.0
    2014 : 2014 年 4 月 : Spring Boot 1.0.0 发布
    2015 : 2015 年 3 月 : Spring Boot 1.2 发布
    2016 : 2016 年 12 月 : Spring Boot 1.3 发布 : 同时 Spring 4.2 升级
    2017 : 2017 年 1 月 : Spring Boot 1.4 支持 Couchbase / Neo4j
    2017 : 2017 年 2 月 : Spring Boot 1.5 支持 Kafka / LDAP
    2017 : 2017 年 4 月 : Spring 4.3.8 发布，并成为 4.x 系列中的最后一个
    2018 : 2018 年 3 月 : Spring Boot 2.0 基于 Java 8，支持 Java 9，支持 Quartz : 同时 Spring 5.0 发布
    2022 : 2022 年 11 月 : Spring Boot 3.0 支持 Java 17 : 升级到 Spring 6.0
```

## 参考

[Spring Boot的前世今生](https://www.jianshu.com/p/c3e57bb974f4)