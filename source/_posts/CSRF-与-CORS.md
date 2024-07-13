---
title: CSRF 与 CORS
date: 2024-07-06 11:16:54
tags: 
    - "HTTP"
categories:
    - "Program"
    - "HTTP"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/http.jpg"
excerpt: "CSRF CORS 区别"
---

`CSRF(Cross-Site Request Forgery)`和`CORS(Cross-Origin Resource Sharing)`都是与Web安全和跨域资源共享相关的概念，但它们解决的问题和应对的场景有所不同。

1. CSRF(跨站请求伪造)：CSRF是一种攻击方式，攻击者通过伪造用户已认证的请求，使用户在不知情的情况下执行恶意操作。攻击者通常利用用户的身份验证凭据，以用户的身份执行未经授权的操作，可能导致数据泄露、账户劫持等问题。为了防止CSRF攻击，常见的做法是在请求中包含一个随机生成的令牌(CSRF令牌)，并在服务器端进行验证，确保请求是来自合法的来源。

2. CORS(跨域资源共享)：CORS是一种机制，允许Web应用程序从不同的源头(域、协议、端口)请求资源，而不受浏览器的同源策略限制。同源策略是浏览器的安全策略，它限制了一个源头的文档或脚本如何与其他源头的资源进行交互。通过CORS，服务器可以指定哪些源头(域)有权限访问资源，从而确保跨域请求的安全性。

区别总结如下：

- CSRF是一种攻击方式，而CORS是一种机制。
- CSRF关注的是恶意请求的安全性，而CORS关注的是跨域资源共享的安全性。
- CSRF攻击利用用户身份进行未经授权的操作，而CORS允许合法的跨域请求。

在Web应用程序中，为了保护用户和资源的安全性，我们需要同时考虑防止CSRF攻击和使用CORS机制来进行跨域资源共享。

希望对你有所帮助！如果还有其他问题，请随时提问。