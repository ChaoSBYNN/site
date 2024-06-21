---
title: 选择 www 或非 www URL 作为域名
date: 2017-02-13 23:36:36
tags: HTTP
---
> 选择非 www 或 www 作为网址，对于网站持有者是一个反复出现的问题。本页将会提供了一些有用的相关建议。

# 什么是域名？

在 HTTP URL 中，跟在网址头部 http:// 或 https:// 后面的第一个子字符串称为域，它是网站文件资源所在的服务器的名称。

服务器不一定是物理机：几台服务器可以驻留在同一台物理机器上，或者一台服务器可以通过几台机器进行处理，协作处理并响应或负载均衡它们之间的请求。关键点在于语义上一个域名代表一个单独的服务器。

# 所以，我只能选择其中一个做为我的网站的网址？

是的，你必须选择其中之一，并坚持使用。选择并使用其中哪一个取决于你，但无论你选择那一个，保持下去。这将让你的网站在用户使用搜索引擎检索时更加准确与一致。这包括始终链接到所选域名（如果你在网站中使用相对网址，则不应该很难），也可以始终将链接（通过电子邮件/社交网络等）共享使用同一个域名。
不，你不能有两个。最重要的是，你是保持的那一个官方的域名，这个官方域名被称为规范名称。你所有的绝对链接应该使用它。但即便如此，你仍然可以有其他域名使用：HTTP允许使用两种技术，以便它在使用规范域名的同时还允许非规范域名使用，使使用者或搜索引擎可以准确的访问到所预期的页面。
所以，选择其中一个作为你的域名的规范地址！下面有两种技术允许不规范的域名仍然起作用。

# 规范网址方式

选择下面有两种不同的方式使网站规范。

### 使用HTTP301重定向

在这种情况下，你需要配置服务器接收的HTTP请求（ 常见为 www 和非 www 网址相同）以及适当的HTTP响应 301 去响应所有非规范的域名请求。这会将尝试使访问非规范网址的浏览器重定向到其规范的等效网址。举例来说，如果您选择使用非 www 网址为规范类型，你的所有 www 网址都应该被重定向到对应的非 www 网址上。

例如：

1.  服务器收到 http://www.example.org/whaddup 请求（当规范域名是 example.org 时）
2.  服务器则以代码 301 与头 Location ：http://example.org/whaddup
3.  该客户端发出的规范的域名请求：http://example.org/whatddup

[HTML5 boilerplate project](https://github.com/h5bp/html5-boilerplate)  有一个示例 [how to configure an Apache server to redirect one domain to the other](https://github.com/h5bp/html5-boilerplate/blob/7a22a33d4041c479d0962499e853501073811887/.htaccess#L219-L258) 。

### 使用 HTML 标签元素

```javascript 
<link rel="canonical">
```

它可以将一个特殊的 HTML <link> 元素添加到网页指示什么网页的标准地址，这对页面的访问者没有影响，但在搜索引擎检索时会告诉搜索引擎当页面实际的地址。通过这种方式，搜索引擎不需要索引同一页面多次，那样可能导致它被视为重复的内容或垃圾邮件，甚至从搜索引擎结果中删除或者降低你的页面显示排名。

当加入这样一个标签，会告诉搜索引擎，你提供相同内容的两个域名那一个是规范的。
以上方为例，
```
http://www.example.org/whaddup
```
将提供与 
```
http://example.org/whaddup
```
 相同的内容，但有一个附加的 <link> 头部元素：

```javascript 
<link href="http://example.org/whaddup" rel="canonical"> 
```

不同于以往，浏览器历史记录将考虑非 www 和 www 的网址作为独立的条目。

# 请使用两者中的一个为你的网页服务

有了这些技术，您可以将服务器配置为两个正确响应， www 的前缀和非 www 前缀的域名，这是要做到这一点，因为你无法预测哪些 URL 用户输入他们的浏览器的 URL 好的建议酒吧，这是选择你要作为规范的位置使用，然后重定向其他类型的它哪种类型的问题。

# 根据情况决定使用

可以认为这是一个非常主观 [bikeshedding](http://bikeshed.com/) 问题。 如果你想更深入的阅读，请参阅  [WWW vs non-WWW for your Canonical Domain URL – Which is Best and Why?](http://www.hyperarts.com/blog/www-vs-non-www-for-your-canonical-domain-url-which-is-best-and-why/) ，它可能提出进一步的见解。

# 请参阅

[Stats on what people type in the URL bar](http://www.chrisfinke.com/2011/07/25/what-do-people-type-in-the-address-bar/) (2011)

# 源网页

* 英文版 [Choosing between www and non-www URLs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Choosing_between_www_and_non-www_URLs)
* 中文版 [选择 www 或非 www URL 作为域名](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/%E9%80%89%E6%8B%A9_www_%E6%88%96%E9%9D%9E_www_URL_%E4%BD%9C%E4%B8%BA%E5%9F%9F%E5%90%8D) 