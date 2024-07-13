---
title: Spring Cloud Gateway 4种接口放行方式
date: 2024-07-08 10:32:20
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
    - "Java"
    - "Spring"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
excerpt: "接口拦截过滤放行 免鉴权"
---

## 4种接口过滤方式

### 使用Spring Cloud Gateway的路由规则，在application.yml文件中定义predicates和filters

```yml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/user/login # 只放行/user/login接口
```

### 使用Spring Security的配置，在application.yml文件中定义ignoreUrls

```yml
security:
  ignoreUrls:
    - /user/login # 放行/user/login接口
```

### 使用自定义过滤器，在GatewayFilterFactory中实现自己的逻辑

```java
public class AuthGatewayFilterFactory extends AbstractGatewayFilterFactory<AuthGatewayFilterFactory.Config> {
 
    @Override
    public GatewayFilter apply(Config config) {
        return (exchange, chain) -> {
            String path = exchange.getRequest().getURI().getPath();
            if (path.equals("/user/login")) { // 放行/user/login接口
                return chain.filter(exchange);
            }
            // 其他逻辑...
        };
    }
 
    public static class Config {
        // 配置属性...
    }
}
```

### 使用自定义路由仓库，在RouteDefinitionRepository中实现自己的逻辑

```java
public class RedisRouteDefinitionRepository implements RouteDefinitionRepository {
 
    @Override
    public Flux<RouteDefinition> getRouteDefinitions() {
        // 从Redis中获取路由定义，并根据需要放行接口...
    }
 
    @Override
    public Mono<Void> save(Mono<RouteDefinition> route) {
        // 保存路由定义到Redis中...
    }
 
    @Override
    public Mono<Void> delete(Mono<String> routeId) {
        // 从Redis中删除路由定义...
    }
}
```

## 四种方式的优缺点

* 使用Spring Cloud Gateway的路由规则，优点是简单方便，可以在配置文件中定义多种路由条件和过滤器，支持动态刷新和自定义扩展；缺点是可能不够灵活，需要遵循Spring Cloud Gateway的规范和约束。
* 使用Spring Security的配置，优点是可以利用Spring Security提供的强大的安全功能，如认证、授权、加密等；缺点是需要额外引入Spring Security依赖，并且可能与其他过滤器冲突或重复。
* 使用自定义过滤器，优点是可以实现自己的业务逻辑和需求，有更高的灵活性和可定制性；缺点是需要编写更多的代码，并且可能需要考虑性能、异常处理、兼容性等问题。
* 使用自定义路由仓库，优点是可以实现自己的路由存储和管理方式，如使用Redis或数据库等；缺点是需要编写更多的代码，并且可能需要考虑数据同步、缓存、事务等问题。
