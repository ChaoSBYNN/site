---
title: Spring HttpMessageConverter
date: 2024-07-27 20:21:03
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
    - "Java"
    - "Spring"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
excerpt: "序列化 转换器"
---
`HttpMessageConverter` 是 Spring 框架中用于处理 HTTP 请求和响应的机制。它的作用是将 HTTP 请求体转换为 Java 对象，或将 Java 对象转换为 HTTP 响应体。这一机制使得 Spring 能够轻松地处理不同的数据格式，如 JSON、XML、表单数据等。

## 以下是一些常见的 `HttpMessageConverter` 类型及其作用

### 1. **`MappingJackson2HttpMessageConverter`**

- **作用**：将 JSON 数据转换为 Java 对象，或将 Java 对象转换为 JSON 数据。
- **常用场景**：处理 JSON 格式的数据，通常与 RESTful Web 服务一起使用。
- **配置**：Spring Boot 默认配置了这个转换器。可以通过 `ObjectMapper` 自定义其行为。

  ```java
  @Bean
  public MappingJackson2HttpMessageConverter mappingJackson2HttpMessageConverter() {
      MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
      converter.setObjectMapper(objectMapper()); // 使用自定义的 ObjectMapper
      return converter;
  }
  ```

### 2. **`MappingJackson2XmlHttpMessageConverter`**

- **作用**：将 XML 数据转换为 Java 对象，或将 Java 对象转换为 XML 数据。
- **常用场景**：处理 XML 格式的数据。
- **配置**：需要 Jackson XML 模块来支持 XML 处理。

  ```java
  @Bean
  public MappingJackson2XmlHttpMessageConverter mappingJackson2XmlHttpMessageConverter() {
      MappingJackson2XmlHttpMessageConverter converter = new MappingJackson2XmlHttpMessageConverter();
      // 可以设置 ObjectMapper 和其他配置
      return converter;
  }
  ```

### 3. **`GsonHttpMessageConverter`**

- **作用**：将 JSON 数据转换为 Java 对象，或将 Java 对象转换为 JSON 数据，使用 Google 的 Gson 库。
- **常用场景**：如果项目中使用 Gson 作为 JSON 处理库。
- **配置**：需要添加 Gson 依赖。

  ```java
  @Bean
  public GsonHttpMessageConverter gsonHttpMessageConverter() {
      GsonHttpMessageConverter converter = new GsonHttpMessageConverter();
      converter.setGson(new Gson()); // 使用自定义的 Gson 实例
      return converter;
  }
  ```

### 4. **`XmlMapper` 和 `Jaxb2RootElementHttpMessageConverter`**

- **作用**：将 XML 数据转换为 Java 对象，或将 Java 对象转换为 XML 数据，通常与 JAXB (Java Architecture for XML Binding) 一起使用。
- **常用场景**：处理 XML 格式的数据，特别是在使用 JAXB 进行 XML 绑定时。
- **配置**：可以通过添加 `jaxb2` 依赖来启用。

  ```java
  @Bean
  public Jaxb2RootElementHttpMessageConverter jaxb2RootElementHttpMessageConverter() {
      return new Jaxb2RootElementHttpMessageConverter();
  }
  ```

### 5. **`FormHttpMessageConverter`**

- **作用**：将表单数据（`application/x-www-form-urlencoded`）转换为 Java 对象，或将 Java 对象转换为表单数据。
- **常用场景**：处理表单提交数据。
- **配置**：Spring Boot 默认配置了这个转换器，通常无需自定义。

  ```java
  @Bean
  public FormHttpMessageConverter formHttpMessageConverter() {
      return new FormHttpMessageConverter();
  }
  ```

### 6. **`ByteArrayHttpMessageConverter`**

- **作用**：将字节数组（`byte[]`）数据转换为 Java 对象，或将 Java 对象转换为字节数组。
- **常用场景**：处理二进制数据，如文件上传或下载。
- **配置**：Spring Boot 默认配置了这个转换器，通常无需自定义。

  ```java
  @Bean
  public ByteArrayHttpMessageConverter byteArrayHttpMessageConverter() {
      return new ByteArrayHttpMessageConverter();
  }
  ```

### 7. **`StringHttpMessageConverter`**

- **作用**：将字符串（`String`）数据转换为 Java 对象，或将 Java 对象转换为字符串。
- **常用场景**：处理文本数据，如普通的文本或 HTML 内容。
- **配置**：Spring Boot 默认配置了这个转换器，通常无需自定义。

  ```java
  @Bean
  public StringHttpMessageConverter stringHttpMessageConverter() {
      return new StringHttpMessageConverter();
  }
  ```

### 8. **`ResourceHttpMessageConverter`**

- **作用**：将资源（如文件）转换为 Java 对象，或将 Java 对象转换为资源。
- **常用场景**：处理资源文件的上传或下载。
- **配置**：Spring Boot 默认配置了这个转换器，通常无需自定义。

  ```java
  @Bean
  public ResourceHttpMessageConverter resourceHttpMessageConverter() {
      return new ResourceHttpMessageConverter();
  }
  ```

### 总结

`HttpMessageConverter` 在 Spring 中发挥着至关重要的作用，通过不同的转换器处理各种数据格式的转换。根据你的应用需求，可能需要配置或自定义这些转换器，以确保数据的正确处理和传递。

## 除了常见的 `HttpMessageConverter` 类型，Spring 还提供了其他一些特定的 `HttpMessageConverter` 类型，处理更特殊的场景或需求。

### 1. **`ResourceRegionHttpMessageConverter`**

- **作用**：用于将 `ResourceRegion` 处理为 HTTP 响应，通常用于分段传输大文件。这种转换器可以支持从一个大文件中读取特定的部分（即分段）并将其传输给客户端。
- **常用场景**：实现大文件的分段下载，特别是在需要支持 HTTP Range 请求时。
- **配置**：通常与 `Resource` 和 `ResourceRegion` 类一起使用。Spring Boot 默认不配置这个转换器，因此如果需要，你可能需要自定义配置它。

  ```java
  @Bean
  public ResourceRegionHttpMessageConverter resourceRegionHttpMessageConverter() {
      return new ResourceRegionHttpMessageConverter();
  }
  ```

### 2. **`AllEncompassingFormHttpMessageConverter`**

- **作用**：处理表单数据（`application/x-www-form-urlencoded`）和多部分表单数据（`multipart/form-data`）。这个转换器能够处理提交的表单数据，并将其转换为 Java 对象或处理文件上传。
- **常用场景**：当你需要处理复杂的表单提交，包括文件上传和多部分数据时，`AllEncompassingFormHttpMessageConverter` 会提供全面的支持。
- **配置**：这是一个比较特殊的转换器，通常需要在特定场景下使用，并且可能需要自定义配置以满足需求。

  ```java
  @Bean
  public AllEncompassingFormHttpMessageConverter allEncompassingFormHttpMessageConverter() {
      return new AllEncompassingFormHttpMessageConverter();
  }
  ```

### 3. **`SourceHttpMessageConverter`**

- **作用**：将 `Source` 类型的数据转换为 HTTP 响应。`Source` 是一个接口，通常用于处理 XML 数据源，它代表一个可以被处理的 XML 数据源。
- **常用场景**：处理 XML 数据的场景，特别是在需要将 XML 数据以原始的 `Source` 类型进行处理或传输时。
- **配置**：通常在需要处理 XML 数据源时使用，默认配置中可能不会启用。

  ```java
  @Bean
  public SourceHttpMessageConverter<Source> sourceHttpMessageConverter() {
      return new SourceHttpMessageConverter<>();
  }
  ```

### 说明与用途

- **`ResourceRegionHttpMessageConverter`**：用于处理大文件的分段传输，特别适用于支持 Range 请求的场景。
- **`AllEncompassingFormHttpMessageConverter`**：处理复杂的表单数据，包括文件上传和多部分表单数据，适用于需要全面表单支持的情况。
- **`SourceHttpMessageConverter`**：用于处理 XML 数据源，适合需要以 `Source` 类型传输和处理 XML 数据的应用。

这些转换器通常是针对特定的使用场景或需求设计的，如果你的应用场景需要它们，可以根据需要进行配置和使用。
