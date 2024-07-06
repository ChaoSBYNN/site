---
title: Spring Boot 文件上传大小设置
date: 2024-07-06 11:19:36
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
    - "Java"
    - "Spring"
cover: "/images/spring.png"
excerpt: "文件上传大小设置"
---

```yml
spring:
  servlet:
    multipart:
      enabled: true
      max-file-size: 20MB
      max-request-size: 200MB
```