---
title: 'Spring Cloud Gateway: An expected CSRF token cannot be found'
date: 2024-07-06 11:15:24
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
    - "Java"
    - "Spring"
cover: "/images/spring.png"
excerpt: "An expected CSRF token cannot be found"
---

> An expected CSRF token cannot be found

因为mvc和gateway冲突，而配置了：`spring.main.web-application-type=reactive`

```java
    @Bean
    public SecurityWebFilterChain springSecurityFilterChain(ServerHttpSecurity http) {
        http.csrf(ServerHttpSecurity.CsrfSpec::disable);
        return http.build();
    }
```