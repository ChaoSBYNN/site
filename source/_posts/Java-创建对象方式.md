---
title: Java 创建对象方式
date: 2024-07-18 08:25:17
tags: 
    - "Java"
categories:
    - "Program"
    - "Java"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/java.png"
excerpt: "对象创建方式 & 适用场景"
---
在 Java 中，创建对象有多种方式。以下是详细的介绍，涵盖了所有主要的对象创建方式及其适用场景。

## 1. 使用 *new* 关键字

### 直接创建对象

最常见的创建对象方式是使用 `new` 关键字来调用类的构造函数。

```java
Person person = new Person();
```

### 带参数的构造函数

可以在创建对象时传递参数给构造函数。

```java
Person person = new Person("John", 30);
```

### 多重构造函数

一个类可以有多个构造函数，这些构造函数可以有不同的参数列表。

```java
public class Person {
    private String name;
    private int age;

    public Person() {
        // 默认构造函数
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## 2. 使用 *clone()* 方法

### `clone()` 克隆对象

`clone()` 方法是 `Object` 类提供的一个方法，可以用来复制现有的对象。

```java
Person originalPerson = new Person("John", 30);
Person clonedPerson = (Person) originalPerson.clone();
```

### 实现 `Cloneable` 接口

要使用 `clone()` 方法，需要实现 `Cloneable` 接口并重写 `clone()` 方法。

```java
public class Person implements Cloneable {
    private String name;
    private int age;

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

## 3. 使用反射机制

### 通过反射创建对象

反射可以动态地创建对象。

```java
Class<?> clazz = Class.forName("Person");
Person person = (Person) clazz.getConstructor().newInstance();
```

### 使用构造函数反射

可以通过反射调用带参数的构造函数。

```java
Class<?> clazz = Class.forName("Person");
Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
Person person = (Person) constructor.newInstance("John", 30);
```

## 4. 使用工厂方法模式

### 工厂方法

工厂方法是一种设计模式，通过工厂类来创建对象。

```java
public class PersonFactory {
    public static Person createPerson(String name, int age) {
        return new Person(name, age);
    }
}

// 使用工厂方法创建对象
Person person = PersonFactory.createPerson("John", 30);
```

## 5. 使用建造者模式

### 建造者模式

建造者模式允许一步步地构建复杂对象。

```java
public class Person {
    private String name;
    private int age;

    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }

    public static class Builder {
        private String name;
        private int age;

        public Builder setName(String name) {
            this.name = name;
            return this;
        }

        public Builder setAge(int age) {
            this.age = age;
            return this;
        }

        public Person build() {
            return new Person(this);
        }
    }
}

// 使用建造者模式创建对象
Person person = new Person.Builder().setName("John").setAge(30).build();
```

## 6. 使用 *java.util.Optional* 创建对象

### 使用 `Optional.of()` 或 `Optional.ofNullable()`

`Optional` 是一个容器对象，用于表示一个可能为 `null` 的值。

```java
Optional<Person> optionalPerson = Optional.of(new Person("John", 30));
Optional<Person> optionalPersonNull = Optional.ofNullable(null);
```

## 7. 使用 Java 8 的 Lambda 表达式

### Lambda 表达式创建对象

Lambda 表达式用于简化函数式接口的实例化。

```java
Supplier<Person> personSupplier = () -> new Person("John", 30);
Person person = personSupplier.get();
```

## 8. 使用 *ObjectFactory* 类

### 自定义工厂类

你可以创建一个工厂类来负责对象的创建。

```java
public class PersonFactory {
    public Person createPerson(String name, int age) {
        return new Person(name, age);
    }
}
```

### 使用工厂类创建对象

```java
PersonFactory factory = new PersonFactory();
Person person = factory.createPerson("John", 30);
```

## 9. 使用 *ApplicationContext* （Spring 框架）

### Spring 框架中的 Bean

在 Spring 框架中，Bean 可以通过 `ApplicationContext` 创建。

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
Person person = context.getBean(Person.class);
```

### 配置 Bean

```java
@Configuration
public class AppConfig {
    @Bean
    public Person person() {
        return new Person("John", 30);
    }
}
```

## 10. 使用序列化与反序列化

### 序列化与反序列化

对象可以通过序列化和反序列化进行创建。

```java
// 序列化
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"));
oos.writeObject(person);
oos.close();

// 反序列化
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"));
Person person = (Person) ois.readObject();
ois.close();
```

### 需要实现 `Serializable` 接口

```java
public class Person implements Serializable {
    private String name;
    private int age;
}
```

## 11. 使用 *ObjectInputStream* 和 *ObjectOutputStream*

### `Stream` 序列化与反序列化

通过 `ObjectInputStream` 和 `ObjectOutputStream` 可以将对象转换为字节流并存储到文件中，再从文件中读取对象。

```java
// 序列化
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"));
oos.writeObject(person);
oos.close();

// 反序列化
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"));
Person person = (Person) ois.readObject();
ois.close();
```

## 12. 使用 *Proxy* 创建动态代理对象

### 动态代理

`Proxy` 类用于创建实现特定接口的动态代理对象。

```java
Person personProxy = (Person) Proxy.newProxyInstance(
    Person.class.getClassLoader(),
    new Class[]{Person.class},
    (proxy, method, args) -> {
        // 方法调用的处理逻辑
        return null;
    });
```

## 13. 使用 *Record* 类

### `Record` 类（Java 14 引入）

`Record` 是一种新类型的类，用于简化数据存储类的创建。

```java
public record Person(String name, int age) {}
```

### 创建 `Record` 对象

```java
Person person = new Person("John", 30);
```

## 14. 使用 *Object.clone()* 方法

### `Object.clone()` 克隆对象

可以使用 `Object.clone()` 方法来复制现有对象。

```java
public class Person implements Cloneable {
    private String name;
    private int age;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

## 15. 使用 *Factory Bean*

### Spring 的 Factory Bean

`FactoryBean` 是 Spring 框架中的一种特殊 bean，用于创建其他 beans。

```java
public class PersonFactoryBean implements FactoryBean<Person> {
    @Override
    public Person getObject() throws Exception {
        return new Person("John", 30);
    }

    @Override
    public Class<?> getObjectType() {
        return Person.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

### 配置 `Factory Bean`

```java
@Configuration
public class AppConfig {
    @Bean
    public PersonFactoryBean personFactoryBean() {
        return new PersonFactoryBean();
    }
}
```

## 16. 使用 Java 17 的 *Record* 类

### `Record` 类（Java 17 引入）

`Record` 类是 Java 14 引入的，简化了数据传输对象的创建。

```java
public record Person(String name, int age) {}
```

### `Factory Bean` 创建 `Record` 对象

```java
Person person = new Person("John", 30);
```

## 17. 使用 *Supplier* 和 *Function* 接口

### Java 8 函数式接口

可以使用 `Supplier` 和 `Function` 接口来创建对象。

```java
Supplier<Person> personSupplier = () -> new Person("John", 30);
Person person = personSupplier.get();
```

### `Function` 接口的使用

```java
Function<String, Person> personCreator = name -> new Person(name, 30);
Person person = personCreator.apply("John");
```

## 总结

Java 提供了多种创建对象的方式，适用于不同的场景和需求。从最简单的 `new` 关键字到复杂的 `Factory Bean` 和 `动态代理`，这些技术可以帮助你在编写 Java 代码时选择最合适的对象创建方式。掌握这些技术将有助于提高代码的灵活性、可维护性和性能。

### 参考文献

- [Java官方文档](https://docs.oracle.com/en/java/)
- [Java 设计模式](https://www.designpatterns.io/)
- [Spring Framework 文档](https://spring.io/projects/spring-framework)
- [Java 8 新特性](https://docs.oracle.com/javase/8/docs/technotes/guides/language/)
- [Java 14 新特性](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)
- [Java 17 新特性](https://www.oracle.com/java/technologies/javase-downloads.html)
