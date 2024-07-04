---
title: 微服务版本控制 Microservices Versioning
date: 2024-07-04 09:01:36
tags: 
    - "Microservices"
categories:
    - "Program"
    - "Architecture"
    - "Microservices"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/code.jpg"
excerpt: "版本控制的含义、为什么微服务中的版本控制至关重要、基于微服务的应用程序中的版本控制策略类型"
---

## 原文

* [Best Practices in Microservices Versioning](https://www.codeguru.com/dotnet/best-practices-versioning-microservices/)

## What is Versioning and Why is it Important? 什么是版本控制?为什么它很重要?

版本控制是一种策略，可让您维护具有相同功能的多个服务。与传统应用程序不同，基于微服务的应用程序中的版本控制并不简单。

版本控制是 API 设计的重要组成部分，它使开发人员能够改进 API，而不会破坏其他版本 API 的现有功能。随着应用程序的不断发展和演变，企业通常需要提供新功能，以便客户可以使用此类服务​​。

微服务架构主张团队应独立设计、构建、测试和部署服务。因此，版本控制可能是一个挑战，因为服​​务兼容性可能会丢失。您应该以这样的方式设计微服务，以便在需要时可以恢复到以前的版本。

## Microservices Versioning 微服务版本控制

假设您有一个在生产环境中运行的服务，并且有多个消费者。现在假设您需要为该服务添加更多功能以满足客户的需求。

由于旧服务有多个用户，您可能希望保留现有功能。因此，您可能有一些用户需要旧服务，而其他用户则需要具有新功能或扩展功能的版本。这正是 API 版本控制发挥作用的地方。

阅读: [Transitioning to Microservices – Are You Ready?](https://www.developer.com/design/transition-to-microservices/)

## Microservices Versioning Strategies 微服务版本控制策略

微服务应该随着时间推移而发展——它们应该能够表现出多种行为以满足不同客户的需求。这正是微服务版本控制的作用所在。

基于微服务的应用程序 API 版本控制最常见的两种方式是：

* URI 中的版本控制
* 标题中的版本控制

微服务的版本控制有点棘手。虽然团队设计、开发和部署彼此独立的微服务，但这在版本控制方面带来了问题。虽然您应该设计一个基于微服务的应用程序来支持该服务的多个版本，但这需要额外的路由逻辑来帮助应用程序在运行时支持同一微服务的多个版本。

阅读: [Read: Versioning REST APIs.](https://www.developer.com/java/version-rest-api/)

## URI Versioning URI 版本控制

在此方法中，版本信息作为服务 URL 的一部分提供。这样您就可以识别正在使用的服务版本。您应该将 URL 中未指定版本的服务请求重定向到服务的默认版本。以下是 URI 中使用版本信息的示例:

```curl
http://myecommerceapp/v1.1.2/v1/GetAllProducts
http://myecommerceapp/v2.0.0/GetProducts
```

URI 修订通常用于更新服务的公共 API。它不会对其后端数据存储进行任何重大更改。使用 URI 版本控制的缺点是，随着时间的推移，处理非常大的 URI 占用空间可能会变得难以管理。

以下代码片段展示了如何实现启用版本控制的控制器。

```c#
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
namespace SaveRequestResponseHeader.Controllers
{
    [ApiController] 
    [ApiVersion("1.0")] 
    [Route("api/{version:apiVersion}/product")] 
    public class ProductV1Controller : ControllerBase 
    {
        [HttpGet] 
        public IActionResult Get() 
        { 
            return new OkObjectResult("Inside Product v1 Controller"); 
        } 
    }
}
```

## Header Versioning 标头版本控制

在此方法中，版本信息使用请求标头传递。HTTP 协议中的几个标头属性之一是内容版本。标头驱动的版本控制使用此属性来指定服务。

标头版本控制的主要好处是资源的名称和位置保持不变。这可确保 URI 不会因版本信息而杂乱无章，并且 API 对开发人员而言仍然具有语义意义。

这种方法有一个缺点：信息无法轻松地编码在超媒体链接中。以下是在 ASP.NET Core 中配置基于标头的版本控制的方法。

```c#
public void ConfigureServices(IServiceCollection services)
{
        services.AddControllers();
        services.AddApiVersioning(config => {
        config.DefaultApiVersion = new ApiVersion(1, 0); 
        config.AssumeDefaultVersionWhenUnspecified = true; 
        config.ReportApiVersions = true; 
        config.ApiVersionReader = 
        new HeaderApiVersionReader("x-api-version");
    });
}

```

## Semantic Versioning 语义版本控制

语义版本控制是另一种版本控制策略，其中每个版本使用三个非负整数表示，即Major、Minor和Patch。使用语义版本控制指定版本信息的格式如下：MAJOR.MINOR.PATCH 以下是每个版本的含义：

* **主要版本(Major)** 主要版本表示对 API 的重大更改。如果由于版本发生重大更改导致 API 与新版本不兼容，则可以增加此版本。
* **次要版本(Minor)** 当您拥有兼容的软件版本但业务逻辑已发生更改时，将增加次要版本。
* **补丁(Patch)** 如果旧版本与新版本兼容，但更新版本中修复了错误，则应增加此值。

次要版本和补丁版本通常用于向后兼容更新。在语义版本控制中，版本号是根据更改的严重程度分配的。

例如，v1.0.1是一个小改动，只有几个补丁，而v1.1.0是一个小改动。如果您的服务有重大变化，您可以将版本号设为v2.0.0。

以下是使用语义版本控制调用 API 的方法。

```curl
http://api.product.com/v1.2.3/GetProducts 
```

如果您正在从事的项目有多个模块之间相互依赖，那么语义版本控制是一个不错的选择。

## Calendar Versioning 日历版本控制

日历版本控制是一种语义版本控制，利用日历日期代替非负整数。它不固定特定的格式 - 相反，它使用年、月、日的组合，并允许您从各种年、月、日组合中进行选择。

对于受时间限制的应用程序来说，日历版本控制是一个不错的选择。与语义版本控制不同，用于指定日历版本控制的格式如下：

```
MAJOR.MINOR.MICRO 
```

请注意，在语义版本控制中，MICRO被称为PATCH 。

阅读: [Understanding Android Versioning.](https://www.developer.com/mobile/android/understanding-android-versioning/)

