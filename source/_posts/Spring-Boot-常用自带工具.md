---
title: Spring Boot 常用自带工具
date: 2026-01-20 09:05:43
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
---

- [别再重复造轮子了！Spring Boot 自带的 49 个“宝藏”工具类](https://zhuanlan.zhihu.com/p/1996605165302727229)

## 字符串处理

### 1. StringUtils

一个用于常见字符串操作的综合工具，包括检查空字符串或仅包含空白字符的字符串。

```java
import org.springframework.util.StringUtils;
// 检查字符串是否为空或 null
boolean isEmpty1 = StringUtils.isEmpty(null);     // true
boolean isEmpty2 = StringUtils.isEmpty("");       // true
// 检查字符串是否有实际文本内容（不仅仅是空格）
boolean hasText1 = StringUtils.hasText("   ");    // false
boolean hasText2 = StringUtils.hasText("hello");  // true
// 将字符串标记化为数组
String[] parts = StringUtils.tokenizeToStringArray("a,b,c", ","); // ["a", "b", "c"]
// 去除开头和结尾的空格
String trimmed = StringUtils.trimWhitespace("  hello  "); // "hello"
```

### 2. AntPathMatcher

一个强大的工具，用于根据 Ant 风格的路径模式匹配字符串，常用于 URL 路由和安全配置。

```java
import org.springframework.util.AntPathMatcher;

AntPathMatcher matcher = new AntPathMatcher();
boolean match1 = matcher.match("/users/*", "/users/123");       // true
boolean match2 = matcher.match("/users/**", "/users/123/orders"); // true
boolean match3 = matcher.match("/user?", "/user1");             // true
// 提取路径变量
Map<String, String> vars = matcher.extractUriTemplateVariables(
    "/users/{id}", "/users/42"); // {id=42}
```

### 3. PatternMatchUtils

提供了一种更简单的方法来执行基本的通配符模式匹配。

```java
import org.springframework.util.PatternMatchUtils;

boolean matches1 = PatternMatchUtils.simpleMatch("user*", "username"); // true
boolean matches2 = PatternMatchUtils.simpleMatch("user?", "user1");   // true
boolean matches3 = PatternMatchUtils.simpleMatch(
    new String[]{"user*", "admin*"}, "adminuser"); // true
```

### 4. PropertyPlaceholderHelper

使用给定的属性源解析字符串中的占位符（如 ${name}）。

```java
import org.springframework.util.PropertyPlaceholderHelper;
import java.util.Properties;

PropertyPlaceholderHelper helper = new PropertyPlaceholderHelper("${", "}");
Properties props = new Properties();
props.setProperty("name", "World");
props.setProperty("greeting", "Hello ${name}!");
String result = helper.replacePlaceholders("${greeting}", props::getProperty);
// result -> "Hello World!"
```

## 集合与数组处理

### 5. CollectionUtils

提供广泛的集合操作工具，例如检查是否为空以及执行集合运算。

```java
import org.springframework.util.CollectionUtils;
import java.util.*;

// 检查集合是否为 null 或空
boolean isEmpty1 = CollectionUtils.isEmpty(null);                 // true
boolean isEmpty2 = CollectionUtils.isEmpty(Collections.emptyList()); // true
// 查找两个列表的交集
List<String> list1 = Arrays.asList("a", "b", "c");
List<String> list2 = Arrays.asList("b", "c", "d");
Collection<String> intersection = CollectionUtils.intersection(list1, list2); // [b, c]
```

### 6. MultiValueMap

标准 Map接口的扩展，允许为一个键存储多个值。

```java
import org.springframework.util.LinkedMultiValueMap;
import org.springframework.util.MultiValueMap;

MultiValueMap<String, String> map = new LinkedMultiValueMap<>();
map.add("colors", "red");
map.add("colors", "blue");
map.add("sizes", "large");
List<String> colors = map.get("colors"); // [red, blue]
```

### 7. ConcurrentReferenceHashMap

一个线程安全、基于软引用的 Map，非常适合缓存场景，允许垃圾收集器在需要时回收内存。

```java
import org.springframework.util.ConcurrentReferenceHashMap;
import java.util.Map;

// 为高并发缓存创建一个线程安全的 map
Map<String, Object> cache = new ConcurrentReferenceHashMap<>();
cache.put("key1", new Object()); // 如果内存不足，该值可以被垃圾回收
```

### 8. SystemPropertyUtils

解析 JVM 系统属性中的占位符。

```java
import org.springframework.util.SystemPropertyUtils;

// 使用系统属性解析占位符
String javaHome = SystemPropertyUtils.resolvePlaceholders("${java.home}");
// 如果找不到属性，则提供默认值
String pathWithDefault = SystemPropertyUtils.resolvePlaceholders(
    "${unknown.property:default_value}"); // "default_value"
```

## 反射与类操作

### 9. ReflectionUtils

一组用于低级反射任务（如查找字段或调用方法）的静态方法，并能优雅地处理异常。

```java
import org.springframework.util.ReflectionUtils;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

// 查找并设置字段值
Field field = ReflectionUtils.findField(MyClass.class, "name");
ReflectionUtils.makeAccessible(field);
ReflectionUtils.setField(field, myObject, "John");
// 查找并调用方法
Method method = ReflectionUtils.findMethod(MyClass.class, "setAge", int.class);
ReflectionUtils.invokeMethod(method, myObject, 30);
```

### 10. ClassUtils

提供处理 Class对象的广泛工具，例如获取短类名或查找接口。

```java
import org.springframework.util.ClassUtils;

// 获取类的短名称
String shortName = ClassUtils.getShortName("org.example.MyClass"); // "MyClass"
// 检查类路径上是否存在某个类
boolean exists = ClassUtils.isPresent("java.util.List", null); // true
// 获取默认的类加载器
ClassLoader classLoader = ClassUtils.getDefaultClassLoader();
```

### 11. MethodInvoker

一种在目标对象上准备并调用带参数方法的便捷方式。

```java
import org.springframework.util.MethodInvoker;

MethodInvoker invoker = new MethodInvoker();
invoker.setTargetObject(new MyService());
invoker.setTargetMethod("calculateTotal");
invoker.setArguments(100, 0.2);
invoker.prepare();
Object result = invoker.invoke();
```

### 12. BeanUtils

一个强大的工具，用于常见的 Bean 相关操作，包括属性复制和实例化。

```java
import org.springframework.beans.BeanUtils;

// 将属性从源对象复制到目标对象
Person source = new Person("John", 30);
PersonDTO target = new PersonDTO();
BeanUtils.copyProperties(source, target);
// 使用默认构造函数实例化一个类
Person newPerson = BeanUtils.instantiateClass(Person.class);
```

### I/O 操作

### 13. FileCopyUtils

提供简单的静态方法，用于在文件、流、Reader 和 Writer 之间复制内容。

```java
import org.springframework.util.FileCopyUtils;
import java.io.*;

// 将文件内容复制到字节数组
byte[] bytes = FileCopyUtils.copyToByteArray(new File("input.txt"));
FileCopyUtils.copy(bytes, new File("output.txt"));
// 将内容从 Reader 复制到 String
String content = FileCopyUtils.copyToString(new FileReader("input.txt"));
// 在流之间复制
FileCopyUtils.copy(new FileInputStream("input.txt"), new FileOutputStream("output.txt"));
```

### 14. ResourceUtils

用于将资源位置（如 classpath:或 file:）解析为 File或 URL对象的工具。

```java
import org.springframework.util.ResourceUtils;
import java.io.File;
import java.net.URL;

// 从类路径获取文件
File file = ResourceUtils.getFile("classpath:config.properties");
// 检查字符串是否为 URL
boolean isUrl = ResourceUtils.isUrl("http://example.com"); // true
// 从位置字符串获取 URL
URL url = ResourceUtils.getURL("classpath:data.json");
```

### 15. StreamUtils

包含处理 InputStream和 OutputStream的实用方法，防止常见的样板代码。

```java
import org.springframework.util.StreamUtils;
import java.nio.charset.StandardCharsets;

// 将流复制到字节数组
byte[] data = StreamUtils.copyToByteArray(inputStream);
// 将流复制到字符串
String text = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
// 将字符串复制到输出流
StreamUtils.copy("Hello", StandardCharsets.UTF_8, outputStream);
```

### 16. FileSystemUtils

提供文件系统操作工具，最值得注意的是递归复制和删除目录。

```java
import org.springframework.util.FileSystemUtils;
import java.io.File;

// 递归删除目录及其内容
boolean deleted = FileSystemUtils.deleteRecursively(new File("/tmp/test"));
// 递归复制目录及其内容
FileSystemUtils.copyRecursively(new File("source"), new File("target"));
```

### 17. ResourcePatternUtils

将资源模式（如 classpath*:**/*.xml）解析为 Resource对象数组。

```java
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternUtils;

// 在所有类路径位置的 META-INF 目录中查找所有 XML 文件
ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
Resource[] resources = resolver.getResources("classpath*:META-INF/*.xml");
```

## Web 相关

### 18. WebUtils

各种 Web 实用程序的集合，适用于获取 Cookie 或请求参数等任务。

```java
import org.springframework.web.util.WebUtils;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.Cookie;

// 从请求中获取特定的 cookie
Cookie cookie = WebUtils.getCookie(request, "sessionId");
// 获取带有默认值的请求参数
int pageSize = WebUtils.getIntParameter(request, "pageSize", 10);
```

### 19. UriUtils

根据 RFC 3986 对 URI 组件进行编码和解码的辅助工具。

```java
import org.springframework.web.util.UriUtils;
import java.nio.charset.StandardCharsets;

// 编码 URI 路径段
String encoded = UriUtils.encodePathSegment("path with spaces", StandardCharsets.UTF_8);
// 解码回原始段
String decoded = UriUtils.decode(encoded, StandardCharsets.UTF_8);
```

### 20. UriComponentsBuilder

一个可变构建器，用于从头开始创建 UriComponents或通过修改现有 URI 来操作它。

```java
import org.springframework.web.util.UriComponentsBuilder;
import java.net.URI;

// 以流畅的方式构建复杂的 URI
URI uri = UriComponentsBuilder.fromHttpUrl("http://example.com")
    .path("/products/{id}")
    .queryParam("category", "books")
    .build("123");
// 结果: http://example.com/products/123?category=books
```

### 21. ContentCachingRequestWrapper

包装 HttpServletRequest以缓存其主体，允许读取多次请求负载。

```java
import org.springframework.web.util.ContentCachingRequestWrapper;
import javax.servlet.http.HttpServletRequest;

// 包装请求，例如在过滤器中
ContentCachingRequestWrapper wrapper = new ContentCachingRequestWrapper(request);
// ... 稍后，在处理完请求之后
byte[] body = wrapper.getContentAsByteArray();
String bodyAsString = new String(body, wrapper.getCharacterEncoding());
```

### 22. HtmlUtils

用于 HTML 转义和反转义的工具，有助于防止跨站脚本 (XSS) 攻击。

```java
import org.springframework.web.util.HtmlUtils;

// 转义 HTML 以防止浏览器渲染它
String escaped = HtmlUtils.htmlEscape("<script>alert('XSS')</script>");
// <script>alert('XSS')</script>
// 将 HTML 实体反转义回字符
String unescaped = HtmlUtils.htmlUnescape("<b>Bold</b>");
// <b>Bold</b>
```

## 验证与断言

### 23. Assert

提供静态断言方法来检查前置条件。如果断言失败，它会抛出 IllegalArgumentException。

```java
import org.springframework.util.Assert;

// 验证参数的常用断言
Assert.notNull(object, "Object must not be null");
Assert.hasText(name, "Name must not be empty");
Assert.isTrue(amount > 0, "Amount must be positive");
Assert.notEmpty(items, "Items collection must not be empty");
Assert.state(isInitialized, "Service is not initialized");
```

### 24. ObjectUtils

用于处理对象的工具，特别是用于空安全操作和检查各种类型（数组、集合等）是否为空。

```java
import org.springframework.util.ObjectUtils;

// 检查对象是否为空（null、空字符串、空数组或空集合）
boolean isEmpty1 = ObjectUtils.isEmpty(null);       // true
boolean isEmpty2 = ObjectUtils.isEmpty(new int[0]); // true
// 空安全的 equals 比较
boolean equals = ObjectUtils.nullSafeEquals(obj1, obj2);
// 如果对象为 null，则获取默认值
String value = ObjectUtils.getOrDefault(null, "default"); // "default"
```

### 25. NumberUtils

用于在不同数字类型之间转换并将字符串解析为数字的工具。

```java
import org.springframework.util.NumberUtils;

// 将字符串解析为特定的数字类型
Integer parsedInt = NumberUtils.parseNumber("42", Integer.class);
// 将一种数字类型转换为另一种
Double convertedDouble = NumberUtils.convertNumberToTargetClass(42, Double.class);
```

## 日期与时间

### 26. DateFormatter

一个灵活的 java.util.Date对象格式化程序，用于日期和字符串之间的转换。

```java
import org.springframework.format.datetime.DateFormatter;
import java.util.Date;
import java.util.Locale;

// 注意：Spring 倾向于使用 formatters 而不是静态的 DateTimeUtils 类。
DateFormatter formatter = new DateFormatter("yyyy-MM-dd HH:mm:ss");
String formattedDate = formatter.print(new Date(), Locale.getDefault());
```

### 27. StopWatch

一个简单的任务计时工具，提供了一种方便的方法来测量不同代码块的执行时间。

```java
import org.springframework.util.StopWatch;

// 一个简单的性能计时工具
StopWatch watch = new StopWatch("MyTask");
watch.start("Phase 1: Data Loading");
// ... 执行数据加载任务
Thread.sleep(100);
watch.stop();
watch.start("Phase 2: Data Processing");
// ... 执行处理任务
Thread.sleep(200);
watch.stop();
// 打印格式化的计时摘要
System.out.println(watch.prettyPrint());
```

## 安全相关

### 28. DigestUtils

提供创建消息摘要（哈希）（如 MD5）的静态方法。

```java
import org.springframework.util.DigestUtils;
import java.io.InputStream;
import java.io.FileInputStream;

// 创建字符串的 MD5 哈希
String md5Hex = DigestUtils.md5DigestAsHex("password".getBytes());
// 创建文件内容的 MD5 哈希
try (InputStream is = new FileInputStream("file.txt")) {
    String fileMd5 = DigestUtils.md5DigestAsHex(is);
}
```

### 29. Base64Utils

用于 Base64 编码和解码数据的工具。

```java
import org.springframework.util.Base64Utils;

byte[] data = "Hello World".getBytes();
// 将数据编码为 Base64 字符串
String encoded = Base64Utils.encodeToString(data);
// 将 Base64 字符串解码回数据
byte[] decoded = Base64Utils.decodeFromString(encoded);
```

### 30. TextEncryptor

Spring Security 提供的接口，用于文本的双向加密和解密。

```java
import org.springframework.security.crypto.encrypt.Encryptors;
import org.springframework.security.crypto.encrypt.TextEncryptor;

// 注意：需要 spring-security-crypto 依赖
String password = "my-secret-password";
String salt = "deadbeef"; // hex-encoded
TextEncryptor encryptor = Encryptors.text(password, salt);
String encryptedText = encryptor.encrypt("This is a secret message");
String decryptedText = encryptor.decrypt(encryptedText);
```

## JSON 与数据转换

### 31. JsonParserFactory

用于获取 JsonParser实例的工厂，提供了一种简单的方法来解析 JSON 字符串，而无需完整的数据绑定库。

```java
import org.springframework.boot.json.JsonParser;
import org.springframework.boot.json.JsonParserFactory;
import java.util.Map;
import java.util.List;

// 获取默认的 JSON 解析器
JsonParser parser = JsonParserFactory.getJsonParser();
// 将 JSON 对象解析为 Map
Map<String, Object> map = parser.parseMap("{\"name\":\"John\", \"age\":30}");
// 将 JSON 数组解析为 List
List<Object> list = parser.parseList("[1, 2, 3]");
```

### 32. ResolvableType

提供一种机制来在运行时处理 java.lang.Class无法单独处理的复杂泛型类型。

```java
import org.springframework.core.ResolvableType;
import java.util.List;
import java.util.Map;

// 获取集合的泛型类型
ResolvableType listType = ResolvableType.forField(getClass().getDeclaredField("myList"));
ResolvableType elementType = listType.getGeneric(0); // 例如，对于 List<String> 是 String
// 为泛型类创建可解析类型
ResolvableType mapType = ResolvableType.forClassWithGenerics(Map.class, String.class, Integer.class);
```

### 33. MappingJackson2HttpMessageConverter

Spring MVC 中用于在 Java 对象和 JSON 之间进行转换的核心组件，可以进行自定义。

```java
import org.springframework.http.converter.json.MappingJackson2HttpMessageConverter;
import com.fasterxml.jackson.databind.ObjectMapper;

// 创建并自定义 JSON 消息转换器
ObjectMapper customMapper = new ObjectMapper();
// ... 配置 ObjectMapper 属性（例如，日期格式、特性）
MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter(customMapper);
```

## 其他实用工具

### 34. RandomStringUtils (Apache Commons)

虽然不是 Spring 类，但在 Spring 项目中经常使用（通常通过 spring-boot-starter包含），值得一提的是它用于生成随机字符串。

```java
// 注意：来自 org.apache.commons.lang3 库
import org.apache.commons.lang3.RandomStringUtils;

// 生成随机字符串
String randomAlpha = RandomStringUtils.randomAlphabetic(10);
String randomAlphanumeric = RandomStringUtils.randomAlphanumeric(10);
String randomNumeric = RandomStringUtils.randomNumeric(6);
```

### 35. CompletableFuture

标准 Java 类，但在现代异步 Spring 应用程序中必不可少。使用其静态方法来组合结果。

```java
import java.util.concurrent.CompletableFuture;
import java.util.List;
import java.util.stream.Collectors;

// 组合多个异步结果的常见模式
List<CompletableFuture<String>> futures = /* 异步调用的 future 列表 */;
CompletableFuture<Void> allOf = CompletableFuture.allOf(
    futures.toArray(new CompletableFuture[0]));
CompletableFuture<List<String>> allResults = allOf.thenApply(v ->
    futures.stream()
           .map(CompletableFuture::join)
           .collect(Collectors.toList()));
```

### 36. CacheControl

一个构建器，用于以类型安全的方式创建 HTTP Cache-Control标头值。

```java
import org.springframework.http.CacheControl;
import java.util.concurrent.TimeUnit;

// 构建 Cache-Control 标头值
CacheControl cacheControl = CacheControl.maxAge(1, TimeUnit.HOURS)
    .noTransform()
    .mustRevalidate();
String headerValue = cacheControl.getHeaderValue(); // "max-age=3600, no-transform, must-revalidate"
```

### 37. AnnotationUtils

用于查找类、方法和字段上的注解的强大工具，包括支持元注解和合并注解属性。

```java
import org.springframework.core.annotation.AnnotationUtils;
import org.springframework.stereotype.Component;

// 查找类上的注解，向上搜索层次结构
Component annotation = AnnotationUtils.findAnnotation(MyService.class, Component.class);
// 从注解中获取特定的属性值
String value = (String) AnnotationUtils.getValue(annotation, "value");
```

### 38. DefaultConversionService

Spring 对 ConversionService的默认实现，可以在许多标准类型之间进行转换（例如，字符串到整数）。

```java
import org.springframework.core.convert.support.DefaultConversionService;

// 创建转换服务实例
DefaultConversionService conversionService = new DefaultConversionService();
// 将字符串转换为整数
Integer intValue = conversionService.convert("42", Integer.class);
Boolean boolValue = conversionService.convert("true", Boolean.class);
```

### 39. HttpHeaders

一个专门用于处理 HTTP 标头的 MultiValueMap，提供方便的常量和方法。

```java
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;

// 构建 HTTP 标头的便捷方式
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_JSON);
headers.setContentLength(1024);
headers.setCacheControl("max-age=3600");
```

### 40. MediaTypeFactory

提供一种根据文件名确定资源的 MediaType（MIME 类型）的方法。

```java
import org.springframework.http.MediaType;
import org.springframework.http.MediaTypeFactory;
import java.util.Optional;

// 根据文件名猜测媒体类型
Optional<MediaType> mediaType = MediaTypeFactory.getMediaType("document.pdf");
// 返回 Optional[application/pdf]
```

### 41. MimeTypeUtils

包含常见 MIME 类型的常量，并提供处理它们的实用方法。

```java
import org.springframework.util.MimeTypeUtils;

// 提供常见的 MIME 类型常量和实用程序
MimeType a = MimeTypeUtils.APPLICATION_JSON;
MimeType b = MimeType.valueOf("application/*");
boolean isCompatible = a.isCompatibleWith(b); // true
```

### 42. WebClient.Builder

创建 WebClient（Spring 的现代非阻塞 HTTP 客户端）实例的主要工具。

```java
import org.springframework.web.reactive.function.client.WebClient;

// 使用构建器来配置和创建 WebClient 实例
WebClient webClient = WebClient.builder()
    .baseUrl("https://api.example.com")
    .defaultHeader("Authorization", "Bearer my-token")
    .build();
```

### 43. PropertySourceUtils

一个辅助工具，用于处理环境中所包含的 PropertySource对象中的属性值。

```java
import org.springframework.core.env.ConfigurableEnvironment;
import org.springframework.core.env.MapPropertySource;
import org.springframework.core.env.PropertySource;

// 将属性源添加到环境的示例
Map<String, Object> myProps = new HashMap<>();
myProps.put("app.name", "MyApp");
PropertySource<?> myPropertySource = new MapPropertySource("my-properties", myProps);
environment.getPropertySources().addFirst(myPropertySource);
```

### 44. ApplicationEventPublisher

Spring 中用于在应用程序上下文中发布事件的核心接口，支持解耦的事件驱动架构。

```java
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.beans.factory.annotation.Autowired;

@Autowired
private ApplicationEventPublisher eventPublisher;
public void doSomething() {
    // ... 逻辑 ...
    MyCustomEvent event = new MyCustomEvent(this, "Something important happened");
    eventPublisher.publishEvent(event);
}
```

### 45. LocaleContextHolder

保存当前线程的 Locale，对于国际化 (i18n) 至关重要。

```java
import org.springframework.context.i18n.LocaleContextHolder;
import java.util.Locale;

// 获取当前请求/线程的 locale
Locale currentLocale = LocaleContextHolder.getLocale();
// 以编程方式设置 locale
LocaleContextHolder.setLocale(Locale.FRENCH);
```

### 46. AopUtils

一组用于处理 AOP 代理的静态实用方法。

```java
import org.springframework.aop.support.AopUtils;

// 检查对象是否为 Spring AOP 代理
boolean isAopProxy = AopUtils.isAopProxy(myBean);
// 检查是否为基于 CGLIB 的代理
boolean isCglibProxy = AopUtils.isCglibProxy(myBean);
// 获取代理的底层目标类
Class<?> targetClass = AopUtils.getTargetClass(myBean);
```

### 47. ProxyFactory

一种以编程方式创建 AOP 代理的方法，使你可以细粒度地控制应用哪些接口和建议。

```java
import org.springframework.aop.framework.ProxyFactory;

// 以编程方式为目标对象创建代理
ProxyFactory factory = new ProxyFactory(myTargetObject);
factory.addAdvice(new MyMethodInterceptor()); // 添加建议 (Advice)
MyInterface proxy = (MyInterface) factory.getProxy();
```

### 48. ClassPathScanningCandidateComponentProvider

一个强大的工具，用于扫描类路径以查找符合特定条件的组件（通常由过滤器定义）。

```java
import org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider;
import org.springframework.core.type.filter.AnnotationTypeFilter;
import org.springframework.beans.factory.config.BeanDefinition;
import java.util.Set;

// 扫描类路径以查找带有 @Component 注解的类
ClassPathScanningCandidateComponentProvider scanner =
    new ClassPathScanningCandidateComponentProvider(true);
scanner.addIncludeFilter(new AnnotationTypeFilter(Component.class));
Set<BeanDefinition> components = scanner.findCandidateComponents("com.example.myapp");
```

### 49. YamlPropertiesFactoryBean

一个工厂 Bean，可以加载 YAML 文件并将其转换为 Properties对象。

```java
import org.springframework.beans.factory.config.YamlPropertiesFactoryBean;
import org.springframework.core.io.ClassPathResource;
import java.util.Properties;

// 加载 YAML 文件并将其视为 Properties 对象
YamlPropertiesFactoryBean yamlFactory = new YamlPropertiesFactoryBean();
yamlFactory.setResources(new ClassPathResource("application.yml"));
Properties properties = yamlFactory.getObject();
String appName = properties.getProperty("spring.application.name");
```
