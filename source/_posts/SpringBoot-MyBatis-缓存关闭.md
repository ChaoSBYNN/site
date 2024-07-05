---
title: SpringBoot MyBatis 缓存关闭
date: 2024-07-05 08:59:45
tags: 
    - "Java"
    - "Spring"
    - "Mybaits"
categories:
    - "Program"
    - "Java"
    - "ORM"
    - "MyBatis"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/mybatis.jpg"
excerpt: "同session请求 MyBatis相同SQL执行缓存 实际需要实时数据"
---

在 Spring Boot 中使用 MyBatis，并且想要关闭缓存，可以通过以下几种方法来实现：

## 方法一：在 MyBatis 配置文件中关闭缓存

在 MyBatis 的 XML 配置文件(通常是 `mybatis-config.xml`)中配置关闭缓存的选项。

创建或编辑 MyBatis 配置文件

如果项目中没有 `mybatis-config.xml` 文件，可以创建一个。示例配置如下：

```xml
<!-- mybatis-config.xml -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 关闭缓存 -->
    <settings>
        <setting name="cacheEnabled" value="false"/>
    </settings>
</configuration>
```

在 `<settings>` 下添加 `<setting name="cacheEnabled" value="false"/>`，这将关闭所有 MyBatis 映射器的缓存。

在 Spring Boot 中引入 MyBatis 配置

如果在 Spring Boot 中使用 `application.properties` 或 `application.yml` 进行 MyBatis 配置，需要在配置文件中指定 MyBatis 配置文件的位置：

```yaml

mybatis:
  config-location: classpath:mybatis-config.xml
```

或者在 Java 配置中指定：

```java
@Configuration
@MapperScan("com.example.mapper")
public class MyBatisConfig {

    @Bean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
        sessionFactory.setDataSource(dataSource);

        // 设置 MyBatis 配置文件路径
        Resource resource = new ClassPathResource("mybatis-config.xml");
        sessionFactory.setConfigLocation(resource);

        return sessionFactory.getObject();
    }
}
```

## 方法二：在 Mapper 接口中禁用缓存

另一种方法是在具体的 Mapper 接口或方法上禁用缓存。

在 Mapper 接口中配置

在需要禁用缓存的 Mapper 接口方法上添加 `@Options(useCache = false)` 注解。

```java
@Mapper
public interface UserMapper {

    @Options(useCache = false)
    @Select("SELECT * FROM users WHERE id = #{id}")
    User getUserById(Long id);
}
```

这样做会针对该方法禁用缓存。

## 方法三：在 application.properties 或 application.yml 中配置

如果使用 Spring Boot 的自动配置，可以在 `application.properties` 或 `application.yml` 文件中配置全局的 MyBatis 属性，包括缓存配置。

```yaml
mybatis:
    configuration:
        cache-enabled: false
```

这种方法会在全局范围内禁用 MyBatis 的缓存。

## 注意事项

缓存的作用：MyBatis 的缓存能够显著提高查询效率，因此在关闭缓存之前，请确保了解其对系统性能的影响。
多种方法选择：根据实际需求选择合适的方法，可以是全局禁用、局部禁用或配置文件中配置禁用。
通过以上方法，可以在 Spring Boot 中关闭 MyBatis 的缓存功能。