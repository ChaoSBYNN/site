---
title: Bootiful Spring Boot in 2024 - 2024 Spring Boot 之美
date: 2026-02-05 12:40:47
tags: 
    - "Spring"
    - "Java"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring.png"
---
[Bootiful Spring Boot in 2024](https://github.com/joshlong-attic/bootiful-spring-boot-2024-blog/tree/main/service)
[2024 年的 Spring Boot 之美【译】](https://www.ginonotes.com/posts/bootiful-spring-boot-in-2024)

Hi, Spring fans! I'm https://spring.io/team/joshlong[Josh Long], and I work on the Spring team. I'm a Kotlin GDE and a Java Champion, and I'm of the opinion that there's never been a better time to be a Java and Spring Boot developer. I say that fully aware of where we stand in the span of things today. It's been 21+ years since the earliest releases of the Spring Framework and 11+ years since the earliest releases of Spring Boot. This year marks 20 years since the Spring Framework and 10 years since Spring Boot. So, when I say there's never been a better time to be a Java and Spring developer, bear in mind I've been in this for the better part of those decades. I love Java and the JVM,  and I love Spring, and it's been amazing.

大家好，Spring 粉丝们！我是 Josh Long，在 Spring 团队任职。今年我很高兴能在 Microsoft 的 JDConf 上做主题演讲。作为 Kotlin GDE 和 Java Champion，我认为现在是 Java 和 Spring Boot 开发者的黄金时代。这话我是在完全了解我们目前所处历史阶段的情况下说的。现在离 Spring 框架和 Spring Boot 首次发布已分别过去了 21 年和 11 年，今年是 Spring 框架和 Spring Boot 的 20 周年和 10 周年纪念。所以，当我说现在是成为 Java 和 Spring 开发者的最佳时机时，请记住我在这个行业已经奋斗了几十年。我热爱 Java 和 JVM，我热爱 Spring，这段经历非常棒。

But this is the best time. It's never been close. So, let's develop a new application, as always, by visiting my second favorite place on the internet, after production, `start.spring.io`[https://start.spring.io], and we'll see what I mean. Click `Add Dependencies` and choose `Web`, `Spring Data JDBC`, `OpenAI`, `GraalVM Native Support`, `Docker Compose`, `Postgres`, and `Devtools`.

但现在是最好的时光，前所未有。所以，让我们像往常一样开始一个新的应用开发旅程，前往我在互联网上第二喜欢的地方 start.spring.io，你就会明白我说的。点击 Add Dependencies 并选择 Web、Spring Data JDBC、OpenAI、GraalVM Native Support、Docker Compose、Postgres 和 Devtools。

Give it an artifact name. I called my service… "service". I'm great with names. I get that from my dad. My dad was also amazing with names. When I was a small boy, we had a tiny little white dog, and my father named him _White Dog_. He was our family pet for years. But after around ten years, he disappeared. I'm not sure what became of him after all. Maybe he got a job; I don't know. But then miraculously, another small white dog appeared tapping on our home's screen door. So we took him in, and my father named him _Too_. Or _Two_. I don't know. Anyway, _great_ with names. That said, my mom tells me all the time that I'm very lucky she named me... And, yah, that's probably true.

给项目起一个名字。我把我的服务命名为“service”。我在起名方面很有一套，这一点我继承自我父亲。我的父亲在取名方面也很有天赋。我小时候，我们家有一只小白狗，我父亲给它起名叫 White Dog，它是我们家很多年的宠物。但大约十年后，它就不见了。我不确定后来它怎样了。也许它找到了工作，我不清楚。但后来神奇的是，另一只小白狗出现在我们家的纱门前。我们便收留了它，我父亲给它起名叫 Too，或者是 Two。我不确定。不管怎样，他在取名方面很有一套。尽管如此，我妈妈总是对我说，幸好是她给我起的名字......嗯，这很可能是真的。

Anyway, choose Java 21. This part is key. You can't use Java 21 if you don't use Java 21. So, you need Java 21. But we are also going to use GraalVM for its native image capabilities.

无论如何，选定 Java 21。这一点非常关键。如果你不使用 Java 21，那你就无法体验到它的优势。所以，你需要 Java 21。同时，我们还会使用 GraalVM 以利用其 Native Image 功能。

Don't have Java 21 installed? Download it! Use the fantastic https://sdkman.io[SDKMAN] tool: `sdk install java 21-graalce`. And then make it your default: `sdk default java 21-graalce`. Open a new shell. Download the `.zip` file.

还没有安装 Java 21？那就赶快下载！使用出色的 SDKMAN 工具：sdk install java 21-graalce。然后将其设置为默认版本：sdk default java 21-graalce。打开一个新的命令行界面。下载 .zip 文件。

Java 21 is amazing. It's a far sight better than Java 8. It's technically superior in every way. It's faster, more robust, more syntax-rich. It's also morally superior. You won't like the look of shame and disgrace in your children's eyes when they see you're using Java 8 in production. Don't do it. Be the change you want to see in the world. Use Java 21.

Java 21 实在是太棒了，比 Java 8 有着天壤之别。它在各方面都有技术上的优势。它更快、更稳定、语法更丰富。它甚至在道德上也更胜一筹。当你的孩子们看到你还在生产环境中使用 Java 8 时，他们眼中的羞耻和失望是你无法承受的。不要那样做，成为你想看到的变化，使用 Java 21。

You'll get a zip file. Unzip it and open it in your IDE.

你将得到一个 zip 文件。解压它，并在你的集成开发环境（IDE）中打开。

I'm using IntelliJ IDEA, and it installs a command-line tool called `idea`.

我正在使用 IntelliJ IDEA，它提供了一个名为 idea 的命令行工具。

```shell
cd service
idea build.gradlew
# idea pom.xml if you're using Apache Maven
```

If you're using  Visual Studio Code, be sure to install the https://marketplace.visualstudio.com/items?itemName=vmware.vscode-boot-dev-pack[_Spring Boot Extension Pack_] on the _Visual Studio Code Marketplace_.

如果你使用 Visual Studio Code，一定要在 Visual Studio Code Marketplace 上安装 Spring Boot 扩展包。

This  new application is going to be talking to a database; it's a data-centric application. On the _Spring Initializr_, we added support for  PostgreSQL, but now we need to connect to it. The last thing we want is a long `README.md` with a section titled, _A Hundred Easy Steps to Development_. We want to live that _`git clone` &amp; Run_ life!

这个新应用会与数据库交互，这是一个以数据为中心的应用。在 Spring Initializr 上，我们添加了对 PostgreSQL 的支持，但现在我们需要连接到它。我们不想要一个长长的 README.md 文件，里面有一个标题为“开发的一百个简单步骤”的章节。我们想要的是 git clone 然后马上运行的体验！

To that end, the Spring Initializr generated a https://github.com/docker/compose[Docker Compose] `compose.yml` file that contains a definition for Postgres, the amazing SQL database.

为此，Spring Initializr 生成了一个 Docker Compose compose.yml 文件，其中包含了 Postgres 定义，Postgres 是一个优秀的 SQL 数据库。

.the Docker Compose file, `compose.yaml`

```shell
link:compose.yaml[role=include]
```

Even better, Spring Boot is configured to automatically run the Docker Compose (`docker compose up`) configuration  when the Spring Boot application starts up. No need to configure connectivity details like `spring.datasource.url` and `spring.datasource.password`, etc. It's all done with Spring Boot's amazing  autoconfiguration. Ya love to see it! And, never wanting to leave a mess, Spring Boot will shut down the Docker containers on application shutdown, too.

更棒的是，Spring Boot 配置了自动运行 Docker Compose (docker compose up)，无需手动配置连接细节如 spring.datasource.url 和 spring.datasource.password 等。这一切都通过 Spring Boot 的自动配置完成。这太令人喜欢了！而且，Spring Boot 在应用关闭时会自动停止 Docker 容器，不会留下任何混乱。

We want to move as quickly as possible. To that end, we selected DevTools on the _Spring Initializr_. It'll allow us to move quickly. The core conceit here is that restarting Java is pretty slow. Restarting Spring, however, is really quick. So, what if we had a process monitoring our project folder, and that could  take note of newly compiled `.class` files, load them into the classloader, and then create a new Spring `ApplicationContext`, discarding the old one, and giving us the appearance of a live reload? That's exactly what Spring's DevTools do. Run it during development and see your restart time diminish by huge fractions!

我们希望尽可能迅速地进行开发。为了实现这一点，我们在 Spring Initializr 上选择了 DevTools，它可以让我们快速行动。这里的关键概念是 Java 重启相当慢，但重启 Spring 却非常快。那么，如果我们有一个过程监控我们的项目目录，并能够识别新编译的 .class 文件，将它们加载到类加载器中，然后创建一个新的 Spring ApplicationContext 并替换旧的，给我们一种实时重载的感觉呢？这正是 Spring 的 DevTools 所实现的。在开发中运行它，看看你的重启时间会缩短多少！

This works because, again, Spring is super quick to startup... _Except_, that is, when you are launching a PostgreSQL database on each restart. I love PostgreSQL, but, uh, yeah, it's not meant to be constantly restarted each time you're tweaking method names, modifying HTTP endpoint paths, or finessing some CSS. So, let's configure Spring Boot to simply start the Docker Compose file, and to leave it running instead of restarting each time.

再次强调，这是因为 Spring 启动速度极快......除非，你每次重启都要启动 PostgreSQL 数据库。我喜欢 PostgreSQL，但是，嗯，它并不是为了每次你调整方法名、修改 HTTP 端点路径或微调一些 CSS 而频繁重启而设计的。因此，让我们配置 Spring Boot 仅启动 Docker Compose 文件，并保持其运行，而不是每次都重启。

.add the property to `application.properties`

将属性添加到 application.properties

```conf
spring.docker.compose.lifecycle-management=start_only
```

We'll start with a simple record.

我们将从一个简单的实体开始。

```shell
link:src/main/java/com/example/service/Customer.java[role=include]
```

I love Java records! And you should too! Don't sleep on records. This innocuous little `record` isn't just a better way to do something like Lombok's `@Data` annotation does, it's actually part of a handful of features that, culminating in Java 21 and taken together, support something called _data-oriented programming_.

我喜欢 Java 的记录（record）功能！你也应该喜欢它们！不要忽视 record。这个简单的 record 不仅仅是像 Lombok 的 @Data 注解那样更好的做某事的方式，它实际上是 Java 21 中几个特性的一部分，这些特性共同支持一种称为 数据导向编程（data-oriented programming）的概念。

Java language architect Brian Goetz talks about this in his https://www.infoq.com/articles/data-oriented-programming-java/[InfoQ article on Data-Oriented Programming] in 2022.

Java 语言架构师 Brian Goetz 在他 2022 年的 InfoQ 文章 中讨论了这一点。

Java has dominated the world of the monolith, the reasoning goes, because of its strong access control, good and quick compiler, privacy protections, and so on. Java makes it easy to create relatively modular, composable, monolithic applications. Monolithic applications are typically large, sprawling codebases, and Java supports it. Indeed, if you want modularity and want to structure your large monolithic codebase well, check out the https://spring.io/projects/spring-modulith[Spring Modulith] project.

Java 之所以能在单体应用世界占据主导地位，是因为其强大的访问控制、优秀且迅速的编译器、隐私保护等。Java 让创建相对模块化、可组合的单体应用变得简单。单体应用通常是大型、广泛的代码库，Java 支持这种结构。实际上，如果你想要模块化并想要合理组织你的大型单体代码库，请参阅 Spring Modulith 项目。

But things have changed. These days, the vector by which we express change in a system these days is _not_ the specialized implementations of deep-seated hierarchies of abstract types (through dynamic dispatch and polymorphism), but instead in terms of the often ad-hoc messages that get sent across the wire, via HTTP/REST, gRPC, messaging substrates like Apache Kafka and RabbitMQ, etc. It's the data, dummy!

但情况已经发生变化。如今，我们表达系统变化的途径不再是通过动态派发和多态性的抽象类型层次结构的专门实现，而是通过经常是临时的消息在 HTTP/REST、gRPC、Apache Kafka 和 RabbitMQ 等消息基础设施中跨线传递。关键在于数据！

Java has evolved to support these new patterns. Let's take a look at four key features - records, pattern matching, smart switch expressions, and sealed types - to see what I mean. Suppose we work in a heavily regulated industry, like finance.

Java 已经演变以支持这些新模式。让我们看看四个关键特性：记录、模式匹配、智能 switch 表达式和封闭类型，以更好地理解。假设我们工作在一个高度监管的行业，例如金融。

Imagine we have an interface called `Loan`. Obviously, loans are heavily regulated financial instruments. We don't want somebody coming along and adding an anonymous inner class implementation of the `Loan` interface, sidestepping the validation and protection we've worked so earnestly to build into the system.

想象我们有一个名为 Loan 的接口。显然，贷款是受到严格监管的金融工具。我们不希望有人来添加 Loan 接口的匿名内部类实现，从而绕过我们辛勤构建的系统验证和保护机制。

So, we'll use `sealed` types. Sealed types are a new sort of access control or visibility modifier.

因此，我们使用 sealed 类型。封闭类型是一种新的访问控制或可见性修饰符。

```shell
link:src/main/java/com/example/service/Loan.java[role=include]
```

In the example, we're explicitly stipulating that there are two implementations of `Loan` in the system: `SecuredLoan` and `UnsecuredLoan`. Classes are open for subclassing by default, which violates the guarantees implied by a sealed hierarchy. So, we explicitly make the `SecuredLoan` `final`. The `UnsecuredLoan` is implemented as a record, and is implicitly final.

在这个示例中，我们明确指定系统中有两种 Loan 的实现：SecuredLoan 和 UnsecuredLoan。默认情况下，类是开放继承的，这违反了密封层次结构所暗示的保证。因此，我们明确将 SecuredLoan 声明为 final。UnsecuredLoan 作为 record 实现，并且隐式为 final。

Records are Java's answer to tuples. They are tuples. It's just that Java is a nominal language: *things have names*. This tuple has a name, too: `UnsecuredLoan`.  Records give us a lot of power if we agree to the contract they imply. The core conceit of records is that the identity of the object is equal to the identity of the fields, they're called 'components', in a record. So in this case, the identity of the record is equal to the identity of the `interest` variable. If we agree to that, then the compiler can give us a constructor, it can give us storage for each of the components, it can give us a `toString` method, a `hashCode` method, and an `equals` method. And it'll give us accessors for the components in the constructor. Nice! _And_, it supports destructuring! The language knows how to extract out the state for the record.

记录是 Java 对元组的回应。它们本质上是元组。只是 Java 是一种名义语言：事物有名称。这个元组也有一个名称：UnsecuredLoan。如果我们同意它们所隐含的契约，那么记录就会给我们很大的权力。记录的核心理念是，对象的身份等于记录中字段的身份，它们被称为“组件”。所以在这个例子中，记录的身份等同于 interest 变量的身份。如果我们同意这一点，那么编译器就可以为我们提供构造函数，它可以为每个组件提供存储空间，它可以为我们提供 toString 方法、hashCode 方法和 equals 方法。它还会在构造函数中为组件提供访问器。太棒了！而且，它支持解构！语言知道如何从记录中提取状态。

Now, let's say I wanted to display a message for each type of `Loan`. I'll code up a method. Here's the naïve first implementation.

现在，假设我想为每种 Loan 类型显示一条消息。我会编写一个方法。这是最初的实现尝试。

```java
@Deprecated
String badDisplayMessageFor(Loan loan) {
    var message = "";
    if (loan instanceof SecuredLoan) {
        message = "good job! ";
    }
    if (loan instanceof UnsecuredLoan) {
        var usl = (UnsecuredLoan) loan;
        message = "ouch! that " + usl.interest() + "% interest rate is going to hurt!";
    }
    return message;
}
```

This works, sort of. But it's not carrying its weight.

这个方法有些用，但不够理想。

We can clean it up. Let's take advantage of pattern matching, like this:

我们可以改进它。利用模式匹配，像这样：

```java
    @Deprecated
    String notGreatDisplayMessageFor(Loan loan) {
        var message = "";
        if (loan instanceof SecuredLoan) {
            message = "good job! ";
        }
        if (loan instanceof UnsecuredLoan usl) {
            message = "ouch! that " + usl.interest() + "% interest rate is going to hurt!";
        }
        return message;
    }
```

Better. Note that we're using pattern matching to match the shape of the object and then extract the definitively cast-able thing into a variable, `usl`. We don't even really need the `usl` variable, though, do we. Instead, we want to dereference the `interest` variable. So we can change the pattern matching to extract that variable, like this.

更好。注意我们使用模式匹配来匹配对象的形状，然后将明确可转换的对象提取到变量 usl 中。其实我们不真正需要变量 usl，对吧？我们想要引用 interest 变量。因此，我们可以改变模式匹配来直接提取该变量，像这样。

```java
    @Deprecated
    String notGreatDisplayMessageFor(Loan loan) {
        var message = "";
        if (loan instanceof SecuredLoan) {
            message = "good job! ";
        }
        if (loan instanceof UnsecuredLoan(var interest) ) {
            message = "ouch! that " + interest + "% interest rate is going to hurt!";
        }
        return message;
    }
```

What happens if I comment out one of the branches? Nothing! The compiler doesn't care.  We're not handling one of the critical paths through which this code could pass.

如果我注释掉其中一个分支会发生什么？没什么！编译器不会报错。我们没有处理这段代码可能走过的一个关键路径。

Likewise, I have this value stored in a variable, `message`, and I'm assigning it as a side effect of some condition. Wouldn't it be nice if I could cut out the intermediate value and just return some expression? Let's look at a cleaner implementation using smart switch expressions, another nifty novelty in Java.

同样，我将一个值存储在变量 message 中，并将其作为某条件的副作用进行赋值。如果我能直接返回某个表达式而不是中间值，那不是很好吗？让我们看看使用智能 switch 表达式的更清晰实现，这是 Java 中的另一种新特性。

```java
    String displayMessageFor(Loan loan) {
        return switch (loan) {
            case SecuredLoan sl -> "good job! ";
            case UnsecuredLoan(var interest) -> "ouch! that " + interest + "% interest rate is going to hurt!";
        };
    }
```

This version uses smart switch expressions to return a value and pattern matching. If you comment out one of the branches, the compiler will bark, because - thanks to sealed types - it knows that you haven't exhausted all possible options. Nice! The compiler is doing a lot of work for us! The result is both cleaner and more expressive. Mostly.

这个版本利用智能 switch 表达式来返回值，并使用模式匹配。如果你省略一个分支，编译器会警告，因为 -- 多亏了封闭类型 -- 它知道你还没有考虑所有可能的情况。太好了！编译器帮我们做了很多工作！结果既更整洁也更具表现力。大多数情况下。

So, back to our regularly scheduled programming.  Add an interface for the Spring Data JDBC repository and a Spring MVC controller class. Then start the application up. Notice that this takes an exceedingly long period of time! That's because behind the scenes, it's using the Docker daemon to start up the PostgreSQL instance.

回到我们的常规编程。添加一个 Spring Data JDBC 仓库接口和一个 Spring MVC 控制器类。然后启动应用程序。注意这需要相当长的时间！这是因为在后台，它使用 Docker 守护进程启动 PostgreSQL 实例。

But henceforth, we're going to use Spring Boot's  `DevTools`. You only need to recompile. If the app is running, and you're using Eclipse or Visual Studio Code, you will only need to save the file: CMD+S on a macOS. IntelliJ IDEA doesn't have a `Save` option; force a build with CMD+Shift+F9 on macOS. Nice.

但从现在开始，我们将使用 Spring Boot 的 DevTools。你只需要重新编译。如果应用正在运行，并且你正在使用 Eclipse 或 Visual Studio Code，你只需要保存文件：在 macOS 上使用 CMD+S，IntelliJ IDEA 没有 Save 选项；在 macOS 上使用 CMD+Shift+F9 强制构建。棒极了。

Alright, we've got our HTTP web endpoint babysitting a database, but there's nothing in the database, so this will fail, surely. Let's initialize our database with some schema and some sample data.

现在我们有了一个 HTTP Web 端点监控数据库，但数据库中没有任何内容，所以这肯定会失败。让我们用一些模式和样本数据初始化我们的数据库。

Add `schema.sql` and `data.sql`.

添加 schema.sql 和 data.sql。

.the DDL for our application, `schema.sql`

应用程序的 DDL schema.sql

```java
link:src/main/resources/schema.sql[role=include]
```

.some sample data for the application, `data.sql`

应用程序的样本数据 data.sql

```java
link:src/main/resources/data.sql[role=include]
```

Make sure to tell Spring Boot to run the SQL files on startup by adding the following property to `application.properties`:

通过将以下属性添加到 application.properties 中，告诉 Spring Boot 在启动时运行 SQL 文件：

```java
spring.sql.init.mode=always
```

Reload the application: CMD+Shift+F9, on macOS. On my computer, that reload is about 1/3rd the time, or 66% less, than it takes to restart both the JVM and the application itself. Huge.

在 macOS 上使用 CMD+Shift+F9 重新加载应用程序。在我的计算机上，这次重载大约是重新启动 JVM 和应用程序本身所需时间的三分之一，或减少了 66%。非常显著。

It's up and running. Visit `http://localhost:8080/customers` to see the results. It worked! Of course, it worked. It was a demo. It was always going to work.

它已经启动并运行。访问 http://localhost:8080/customers 查看结果。它工作了！当然，它工作了。这是一个演示，它总是会成功的。

This is all pretty stock standard stuff. You could've done something similar ten years ago. Mind you, the code would've been far more verbose. Java's improved by leaps and bounds since then. And of course, the speeds wouldn't have been comparable. And of course, the abstractions are better now. But you could've done something similar - a web application babysitting a database.

这些都是相当标准的东西。十年前你本可以做类似的事情。当然，那时的代码会更加冗长。但自那以后，Java 已经取得了飞跃式的进步。当然，那时的速度和现在无法比拟。另外，现在的抽象更加完善。但你确实可以做类似的事情 -- 一个管理数据库的 Web 应用。

Things change. There are always new frontiers. Right now, the new frontier is *Artificial Intelligence*, or AI. AI: because the search for good ol' *Intelligence* clearly wasn't hard enough.

情况在变化，总是有新的前沿。现在，新的前沿是人工智能，即 AI。

AI is a huge industry, but what most people mean when they think of AI is _leveraging_ AI. You don't need to use Python to use Large Language Models (LLMs), in the same way that most folks don't need to use C to use SQL databases. You just need to integrate with the LLMs, and here Java is second to none for choice and power.

AI 是一个巨大的产业，但当大多数人谈论 AI 时，他们实际上指的是利用 AI。你不需要使用 Python 来使用大型语言模型 (LLM)，就像大多数人不需要使用 C 来使用 SQL 数据库一样。你只需要与 LLMs 集成，在这里 Java 为选择和能力提供了无与伦比的优势。

At our last tentpole  https://springone.io/history-of-spring[SpringOne] developer event, in 2023, we announced https://spring.io/projects/spring-ai[**Spring AI**], a new project that aims to make integrating and working with AI as easy as possible.

在我们上一次重要的 SpringOne 开发者活动中，2023 年，我们推出了 Spring AI，一个旨在使集成和使用 AI 尽可能简单的新项目。

You'll want to ingest data, say from an account, files, services, or even a set of PDFs. You'll want to store them for easy retrieval in a vector database to support similarity searches. And you'll want to then integrate with an LLM, giving it data from that vector database.

你可能想要摄取数据，例如来自账户、文件、服务甚至一组 PDF。你会希望将它们存储在一个向量数据库中以支持相似性搜索，并便于检索。然后你会想要将它们与一个 LLM 集成，提供那个向量数据库中的数据。

Sure, there are client bindings for any of the LLMs that you could possibly want - https://docs.aws.amazon.com/bedrock/[Amazon Bedrock], https://azure.microsoft.com/en-us/products/ai-services/openai-service[Azure OpenAI], https://cloud.google.com/vertex-ai/docs/reference/rest[Google Vertex] _and_ https://ai.google.dev/docs/gemini_api_overview[Google Gemini], https://ollama.com[Ollama], https://huggingface.co[HuggingFace], and of course https://openai.com[OpenAI] itself, but that's _just the beginning_.

当然，存在任何你可能想要的 LLM 的提供方 -- Amazon Bedrock、Azure OpenAI、Google Vertex 和 Google Gemini、Ollama、HuggingFace，当然还有 OpenAI 本身，但这只是开始。

All the knowledge that powers an LLM is baked into a model, and that model then informs the LLM's understanding of the world. But that model has an expiry date, of sorts, after which its knowledge is stale. If the model was built two weeks ago it won't know about something that happened yesterday. So if you want to build, say, an automated assistant that fields requests from users about their bank accounts, then that LLM is going to need to be apprised of the up-to-date state of the world when it does so.

LLM 的所有知识都内置在一个模型中，然后这个模型为 LLM 的世界观提供信息。但这个模型有一种过期日期，之后它的知识会过时。如果模型是两周前构建的，它就不会知道昨天发生的事情。所以如果你想构建，比如说，一个自动助手来处理用户关于他们银行账户的请求，那么这个 LLM 在执行此操作时需要掌握最新的世界状态。

You can add information in the request that you make and use it as context to inform the response. If it were only this simple, then that wouldn't be so bad. There's another wrinkle. Different LLMs support different _token window sizes_. The token window determines how much data can you send and receive for a given request. The smaller the window, the less information you can send, and the less informed the LLM will be in its response.

你可以在你发出的请求中添加信息，并使用它作为上下文来通知响应。如果事情仅此而已，那也不算太坏。但还有另一个问题。不同的 LLM 支持不同的令牌窗口大小。令牌窗口决定了你可以为给定请求发送和接收多少数据。窗口越小，你可以发送的信息就越少，LLM 在其响应中的信息量也就越少。

One thing you might do here is to put the data in a vector store, like https://github.com/pgvector/pgvector[pgvector], https://neo4j.org[Neo4j],  http://weaviate.io[Weaviate], or otherwise, and then connect your LLM to that vector database. Vector stores give you the ability to, given a word or sets of words, find other things that are similar to them. It stores data as mathematical representations and lets you query for similar things.

这里你可以做的一件事是将数据放在一个向量存储中，例如 pgvector、Neo4j、Weaviate 等，然后将你的 LLM 连接到那个向量数据库。向量存储允许你，给定一个词或一组词，找到其他类似的东西。它将数据存储为数学表示，并允许你查询相似的东西。

This whole process of ingesting, enriching, analyzing and digesting data to inform the response from an LLM is called *Retrieval Augmented Generation* (RAG), Spring AI supports all of it. For more, see this https://www.youtube.com/watch?v=aNKDoiOUo9M[Spring Tips video I did on Spring AI].  We're not going to leverage all these capabilities here, however. Just one.

这整个过程，从摄取、丰富、分析到消化数据以通知来自 LLM 的响应，被称为检索增强生成（RAG），Spring AI 支持所有这些。有关更多信息，请查看我关于 Spring AI 的 Spring Tips 视频。不过，我们在这里不会利用所有这些能力，只会使用其中的一部分。

We added `OpenAI` support on the Spring Initializr so Spring AI is already on the classpath. Add a new controller, like this:

我们在 Spring Initializr 上添加了 OpenAI 支持，因此 Spring AI 已经在类路径上。添加一个新的控制器，如下所示：

.an AI-powered Spring MVC controller

一个 AI 驱动的 Spring MVC 控制器

```shell
link:src/main/java/com/example/service/StoryController.java[role=include]
```

Pretty straightforward! Inject Spring AI's `ChatClient`, use it to send a request to the LLM, get the response, and return it as JSON to the HTTP client.

过程相当直观！注入 Spring AI 的 ChatClient，使用它向 LLM 发送请求，获取响应，并以 JSON 格式返回给 HTTP 客户端。

You'll need to configure your connection to OpenAI's API with a property, `spring.ai.openai.api-key=`. I exported it as an environment variable, `SPRING_AI_OPENAI_API_KEY`, before I ran the program. I won't reproduce my key here. Forgive me for not leaking my API credential.

你需要用属性 spring.ai.openai.api-key= 配置你对 OpenAI API 的连接。我将其作为环境变量 SPRING_AI_OPENAI_API_KEY 导出，然后运行程序。我不会在这里公开我的密钥，请原谅我不泄露我的 API 凭据。

CMD+Shift+F9 to reload the application, and then visit the endpoint: `http://localhost:8080/story`. The LLM might take a few seconds to produce a response, so get that cup of coffee, glass of water, or whatever, ready for a quick but satisfying sip.

使用 CMD+Shift+F9 重新加载应用程序，然后访问端点：http://localhost:8080/story。LLM 可能需要几秒钟来产生响应，所以准备好一杯咖啡或一杯水，享受这短暂但令人满足的时刻。

.the JSON response in my browser, with a JSON formatter plugin enabled

我浏览器中的 JSON 响应，启用了 JSON 格式化插件。

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/story-time-ai-response.png)

There it is! We live in an age of miracles! The age of the freaking singularity! You can do anything now.

就在那里！我们生活在一个奇迹的时代！奇迹的时代！现在你可以做任何事情。

But it did take a few seconds, didn't it? I don't begrudge the computer that time! It did a splendid job! I couldn't do that any faster. Just look at the story it generated! It's a work of art.

但这确实花了一些时间，不是吗？我不怪计算机花了这么长时间！它做得很好！我做不到这么快。只是看看它生成的故事！这是艺术品。

But it did take a while. And that has scalability implications for our applications. Behind the scenes when we make a call to our LLM, we're making a network call. Somewhere, deep in the bowels of the code, there's a `java.net.Socket` from which we've obtained a  `java.io.InputStream` that represents the `byte[]` data coming from the service. I don't know if you remember using `InputStream` directly. Here's an example:

但这确实花了一段时间。这对我们的应用程序有可扩展性的影响。在幕后，当我们向我们的 LLM 发出请求时，我们正在进行网络调用。在代码的深处，有一个 java.net.Socket，我们从中获得了一个 java.io.InputStream，代表来自服务的数据的 byte 数组。我不知道你是否还记得直接使用 InputStream。这是一个例子：

```java
    try (var is = new FileInputStream("afile.txt")) {
        var next = -1;
        while ((next = is.read()) != -1) {
            next = is.read();
            // do something with read
        }
    }
```

See that part where we read bytes in from the `InputStream` by calling `InputStream.read`? We call that a *blocking operation*. If we call `InputStream.read` on line four, then we must wait until the call returns until we can get to line five.

看到我们通过调用 InputStream.read 从 InputStream 中读取字节的那部分了吗？我们称之为阻塞操作。如果我们在第四行调用 InputStream.read，那么我们必须等到调用返回才能到达第五行。

What if the service to which we're connecting   simply returns too much data? What if the service is down? What if it never returns? What if we're stuck, waiting, in perpetuity? _What if_?

如果我们连接的服务返回的数据太多怎么办？如果服务宕机了呢？如果它永远不返回怎么办？如果我们被困住了，永远等待怎么办？如果？

This is _tedious_ if it only happens once. But it's an existential threat for our services if it can happen on every thread in the system used to handle HTTP requests. This happens a lot. It's the reason why it's possible to log in to an otherwise unresponsive HTTP service and find that the CPU is basically asleep - idle! - doing absolutely nothing or little at all. All the threads in the thread pool are stuck in a wait state waiting for something that's not coming.

如果这只发生一次，这是乏味的。但如果这在系统中用于处理 HTTP 请求的每个线程上都可能发生，这对我们的服务来说是一个存在的威胁。这种情况很多。这就是为什么有可能登录到一个其他不响应的 HTTP 服务并发现 CPU 基本上在睡觉 -- 闲置！ -- 几乎什么都没做或做得很少。线程池中的所有线程都处于等待状态，等待一些不会到来的东西。

This is a huge waste of the valuable CPUs capacity we have paid for. And the best-case scenario is still not good. Even if the method will eventually return, it still means that the thread on which that request is being handled is unavailable to anything else in the system. The method is monopolizing that thread so nobody else in the system can use it. This wouldn't be an issue if threads were cheap and abundant. But they're not. For most of Java's lifetime, each new thread was paired one to one with an operating system thread. And it was not cheap. There's a certain amount of bookkeeping associated with each thread. One to two megabytes. So you won't be able to create many threads, and you're wasting what few threads you do have. The horror! Who needs sleep anyway?

这对我们支付的宝贵 CPU 来说是一个巨大的浪费。即使最好的情况也不够好。即使该方法最终会返回，这仍然意味着正在处理该请求的线程对系统中的任何其他事情都不可用。该方法正在垄断该线程，所以系统中的其他任何人都不能使用它。如果线程便宜且充足，这不会是问题。但事实并非如此。在 Java 的大部分生命周期中，每个新线程都与一个操作系统线程一对一配对。这并不便宜。每个线程都有一定数量的簿记。所以你无法创建很多线程，你正在浪费你所拥有的少数几个线程。恐怖！谁还需要睡觉呢？

There's got to be a better way.

必须有更好的方式。

You can use non-blocking IO. Things like the hemorrhoid-inducing and complex Java NIO library. This is an option, in the same way that living with a family of skunks is an option: it stinks! Most of us don't think in terms of non-blocking IO, or regular IO, anyway. We live at higher rungs on the abstraction ladder.  We could use reactive programming. I _love_ reactive programming. I even wrote a book about it - _Reactive Spring_[https://reactivespring.io]. But it's not exactly obvious how to make that work if you're not used to thinking like a functional programmer. It's a different paradigm and implies a rewrite of your code.

你可以使用非阻塞 IO。像 hemorrhoid-inducing 和复杂的 Java NIO 库这样的东西。这是一个选择，就像和一家臭鼬家族生活在一起一样：它很臭！我们大多数人不以非阻塞 IO 或常规 IO 的方式思考。我们生活在抽象层次的更高阶层上。我们可以使用反应式编程。我爱反应式编程。我甚至写了一本关于它的书 -- Reactive Spring。但如果你不习惯像函数式程序员那样思考，那么如何使其工作就不那么明显了。这是一个不同的范式，并且意味着你的代码重写。

What if we could have our non-blocking cake and eat it too? With https://openjdk.org[Java 21], now we can! There's a new feature called virtual threads that makes this stuff a ton easier! If you do something blocking on one of these new _virtual threads_, the runtime will detect that you're doing a blocking thing - like `java.io.InputStream.read`, `java.io.OutputStream.write`, and `java.lang.Thread.sleep` - and move that blocking, idle activity off of the thread and into RAM. Then, it'll basically set an egg timer for sleep, or monitor the file descriptor for IO, and let the runtime repurpose the thread for something else in the meantime. When the blocking action has finished, the runtime moves it back onto a thread and lets it continue from where it started, all with almost no changes to your code. It's hard to understand, so let's look at it by way of an example. I  shamelessly  _borrowed_ this example  from Oracle Developer Advocate https://twitter.com/josepaumard[José Paumard].

如果我们可以既要非阻塞蛋糕又要吃它怎么办？现在我们可以使用 Java 21！有一个名为虚拟线程的新特性使这些东西变得容易得多！如果你在这些新的虚拟线程上做一些阻塞的事情，运行时将检测到你正在做一些阻塞的事情 -- 像 java.io.InputStream.read、java.io.OutputStream.write 和 java.lang.Thread.sleep -- 并将那个阻塞的、空闲的活动从线程移出并移到 RAM 中。然后，它基本上会为 sleep 设定一个蛋形计时器，或监视文件描述符进行 IO，并让运行时在此期间将线程用于其他东西。当阻塞动作完成时，运行时将其移回到线程上，并让它从停止的地方继续运行，几乎不需要更改代码。这很难理解，所以让我们通过一个例子来看看。我毫不羞愧地借用了这个示例，来自 Oracle Developer Advocate José Paumard。

.this example demonstrates creating 1,000 threads and sleeping for 400 milliseconds on each one, while noting the name of the first of those 1,000 threads.

此示例演示创建 1000 个线程并在每个线程上 sleep 400 毫秒，同时注意这 1000 个线程中的第一个的名称。

```shell
link:src/main/java/com/example/service/Threads.java[role=include]
```

We're using the `Thread.ofPlatform` factory method to create regular  ol' platform threads, identical in nature to the threads we've created basically since Java's debut in the 1990's. The program creates 1,000 threads. In each thread, we sleep for 100 milliseconds, four times. In between, we test if we're on the first of the 1000 threads, and if we are, we note the current thread's name by adding it to a set. A set dedupes its elements; if the same name appears more than once, there'll still only be one element in the set.

我们使用 Thread.ofPlatform 工厂方法创建常规的平台线程，这与我们自 1990 年代以来创建的线程没有什么不同。该程序创建了 1000 个线程。在每个线程中，sleep 100 毫秒，四次。在此期间，我们检查我们是否是 1000 个线程中的第一个，如果是，我们通过将其添加到集合中来记录当前线程的名称。集合通过其元素去重；如果相同的名称出现多次，集合中仍然只有一个元素。

Run the program (CMD+Shift+F9!) and you'll see that the physics of the program are unchanged. There is only one name in the `Set<String>`. Why wouldn't there be? We only tested the same thread, over and over.

运行程序（CMD+Shift+F9！）你会看到程序的物理特性没有改变。Set<String> 中只有一个名称。为什么不会呢？我们一次又一次地测试同一个线程。

Now, change that constructor to use _virtual threads_: `Thread.ofVirtual`. Super easy change. And now run the program.  CMD+Shift+F9.

现在，将该构造函数更改为使用虚拟线程：Thread.ofVirtual。超级简单的更改。现在运行程序。CMD+Shift+F9。

You'll see that there is more than one element in the set. You didn't change the core logic of your code at all. And indeed you only even had to change _one_ thing, but now, seamlessly, behind the scenes, the compiler and the runtime rewrote your code so that when something blocking   happens on a virtual thread, the runtime seamlessly takes you off and puts you back on threads after the blocking thing has concluded. This means that the thread on which you existed before is now available to other parts of the system. Your scalability is going to go through the roof!

你会看到集合中有多个元素。你根本没有改变代码的核心逻辑。而且你甚至只需要更改一件事，但现在，在幕后编译器和运行时无缝地重写了你的代码，以便当虚拟线程上发生某些阻塞事件时，运行时无缝地将你从线程上取下并在阻塞事务结束后将你放回线程。这意味着你之前存在的线程现在可以用于系统的其他部分。你的可扩展性将会突破屋顶！

And you might protest, well I don't want to have to change all my code. First, that's a ridiculous argument, the change is trivial. You might protest: I don't want to be in the business of creating threads, either. Good point. Tony Hoare, in the 1970's, wrote that `NULL` is the 1 billion dollar mistake. He was wrong. It was, in fact, PHP. But, he also spoke at length about how untenable it was to build systems using threading. You're going to want to work with higher order abstractions, like sagas, actors, or at the very least an `ExecutorService`.

你可能会抗议，好吧，我不想改变所有的代码。首先，这是一个荒谬的论点，变化是微不足道的。你可能会抗议：我也不想从事创建线程的业务。好观点。托尼·霍尔（Tony Hoare）在 1970 年代写道，NULL 是一个 10 亿美元的错误。他错了。事实上，是 PHP。但是，他还长篇大论地讨论了使用线程构建系统是多么不可行。你将想要使用更高阶的抽象，如 saga、actors，或者至少是一个 ExecutorService。

And there's a new virtual thread executor as well: `Executors.newVirtualThreadPerTaskExecutor`. Nice! If you're using Spring Boot, it's trivial to override the default bean of that type for use in parts of the system. Spring Boot will pull that in and defer to it instead. Easy enough. But if you're using Spring Boot 3.2 You are, surely, using Spring Boot 3.2, right? You realize that each release is only support for like a year, right? Make sure to check out an given https://spring.io/projects/spring-boot#support[Spring projects' support policy]. If you are using 3.2,  then you need only add one property to `application.properties` and we'll plugin the virtual thread support for you.

还有一个新的虚拟线程执行器：Executors.newVirtualThreadPerTaskExecutor。太好了！如果你正在使用 Spring Boot，将系统中的这种类型的默认 bean 替换为非常简单。Spring Boot 将引入它并改用它。很容易。但是如果你在使用 Spring Boot 3.2，你当然在使用 Spring Boot 3.2，对吧？你意识到每个版本只支持大约一年，对吧？确保查看任何给定 Spring 项目的支持政策。如果你在使用 3.2，那么你只需要将一个属性添加到 application.properties 中，我们将为你插入虚拟线程支持。

```java
spring.threads.virtual.enabled=true
```

Nice! No code changes required. And now you should see much improved scalability, and might be able to scale down some of the instances in your load balancer, if your services are IO bound. My suggestion? Tell your boss you're gonna save the company a ton of cash but insist you want that money in your paycheck. Then deploy this change. Voilà!

太好了！不需要更改代码。现在你应该看到大大改善的可扩展性，并且可能能够缩减负载均衡器中的一些实例，如果你的服务是 IO 绑定的。我的建议？告诉你的老板你将为公司节省大量现金，但坚持要求将那笔钱放在你的工资单上。然后部署这个更改。瞧！

Alright, we're moving quickly. We've got `git clone` &amp; run-ability. We've got Docker compose support. We've got DevTools. We have a very nice language and syntax. We've got the freaking singularity. We're moving quickly. We've got scalability. Add the Spring Boot Actuator to the build and now you've got observability. I think it's time we turned to production.

好的，我们正在快速行动。我们有 git clone 和运行能力。我们有 Docker compose 支持。我们有 DevTools。我们有一个非常好的语言和语法。我们拥有奇迹。我们正在快速行动，我们有可扩展性。将 Spring Boot Actuator 添加到构建中，现在你就拥有了可观察性。我认为是时候我们转向生产了。

I wanna package this application up and make it as efficient as possible. Here, my friends, there are a couple of things we need to consider. First of all, how do we containerize the application? Simple. Use https://buildpacks.io[Buildpacks]. Easy. Remember, friends don't let friends write Dockerfiles. Use Buildpacks. They're supported out of the box with Spring Boot, too: `./gradlew buildBootImage` or `./mvnw spring-boot:build-image`. This isn't new though, so next question.

我想将这个应用程序打包并使其尽可能高效。在这里，我的朋友们，我们需要考虑几件事。首先，我们如何将应用程序容器化？简单。使用 Buildpacks。简单。记住，朋友不让朋友写 Dockerfiles。使用 Buildpacks。它们也得到了 Spring Boot 的默认支持：./gradlew bootBuildImage 或 ./mvnw spring-boot:build-image。这不是新的，所以下一个问题。

How do we get this thing to be as efficient and optimized as possible? And before we dive into this, my friends, it's important to remember that Java is already very very very efficient. I love https://thenewstack.io/which-programming-languages-use-the-least-electricity/[this blog] from 2018, before the COVID pandemic, or _BC_.

我们如何使这个东西尽可能高效和优化？在我们深入研究之前，我的朋友们，重要的是要记住 Java 已经非常非常非常高效了。我喜欢这篇来自 2018 年的博客，在 COVID 大流行之前，或 BC。

It looks at which languages use the least energy, or are the most energy-efficient. C is the most energy-efficient. It uses the least electricity. 1.0. It's the baseline. It's efficient... for *MACHINES*. Not people! Definitely, _not_  people.

它研究了哪些语言使用的电力最少，或者是最节能的。C 是最节能的。它使用的电力最少。1.0。它是基准。它很高效......对于机器。不是人！绝对，不是 人。

Then we have Rust and its zero-cost abstractions. Well done.

然后我们有 Rust 和它的零成本抽象。做得好。

Then we have C++...

然后我们有 C++...

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/gross.gif)

C++ is disgusting! Moving on...

C++ 是恶心的！继续……

Then we have the Ada language, but.. who cares?

然后我们有 Ada 语言，但是...谁在乎？

And then we have Java, which is nearly 2.0. Let's just round up and say 2.0. Java is two times - twice! - as inefficient as C. Or one half  as efficient as C.

然后我们有 Java，几乎是 2.0。我们就四舍五入说 2.0。Java 是 C 的两倍 -- 两倍！ -- 效率低下。或者是 C 的一半效率。

So far so good? Great. It's in the top 5 most efficient languages, though!

到目前为止还好吧？太好了。它是前五种最高效的语言之一！

If you scroll the list, you'll see some amazing numbers. Go and C# coming in around the 3.0+ range. Scroll down here, and we have JavaScript and TypeScript, one of which - to my endless bafflement - is four times _less_ efficient than the other!

如果你滚动列表，你会看到一些惊人的数字。Go 和 C# 在 3.0+ 左右。向下滚动，在这里我们有 JavaScript 和 TypeScript，其中之一 -- 令我无尽困惑 -- 比另一个效率低四倍！

Then we have PHP and Hack, the less said about which the better. Moving on!

然后我们有 PHP 和 Hack，越少说越好。继续！

Then we have JRuby and Ruby. Friends, remember https://www.jruby.org[JRuby] is https://www.ruby-lang.org/en/[Ru[by], written in Java. And Ruby is Ruby, written in C. And yet JRuby is almost a one third more efficient than Ruby! Just by dint of having been written in Java, and running on the JVM. The JVM is an amazing piece of kit. Absolutely phenomenal.

然后我们有 JRuby 和 Ruby。朋友们，记住 JRuby 是用 Java 编写的 Ruby。Ruby 是用 C 编写的 Ruby。然而 JRuby 几乎比 Ruby 高效三分之一！只是因为它是用 Java 编写的，并运行在 JVM 上。JVM 是一个了不起的套件。绝对非凡。

Then.. we have Python. And this, well this makes me very sad indeed! I _love_ Python! I've been using Python since the 1990's! Bill Clinton was president when I first learned Python! But these numbers are _not_ great. Think about it. 75.88. Let's round up to 76. I'm not great at math. But you know what is? Freakin' Python! Let's ask it.

然后..我们有 Python。而这，嗯，这确实让我非常难过！我爱 Python！我从 1990 年代开始使用 Python！比尔·克林顿是总统当我第一次学习 Python 时！但这些数字不 是很棒。想想看。75.88。我们就四舍五入到 76。我数学不太好。但你知道谁数学好吗？该死的 Python！让我们问它。

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/python-doing-math.png)

38! That means that if you ran a program in Java, and the generation of the energy required to run it creates a bit of carbon that ends up  trapped in the atmosphere, raising the temperature, and that heightened temperature in turn kills ONE tree, then the equivalent program in Python would kill THIRTY-EIGHT trees! That's a forest! That's worse than Bitcoin! We need to do something about this, and soon. I don't know what, but something.

38！这意味着如果你在 Java 中运行一个程序，并且运行它所需的能量产生了一些碳，这些碳最终被困在大气中，提高了温度，这种升高的温度反过来又杀死了一棵树，那么在 Python 中运行的等效程序将会杀死三十八棵树！这是一片森林！这比比特币还糟！我们需要尽快做些什么，我不知道是什么，但需要做些什么。

Anyway, what I'm trying to say is that Java is already _amazing_. I think this is because of two things that people take for granted: garbage collection and the Just In Time (JIT) compiler.

无论如何，我想说的是 Java 已经很棒了。我认为这是因为两件人们认为理所当然的事情：垃圾回收和即时 (JIT) 编译器。

Garbage collection, well we all know what it is. Heck, https://www.whitehouse.gov/wp-content/uploads/2024/02/Final-ONCD-Technical-Report.pdf[even the White House appreciates garbage-collected, memory-safe languages] like Java in its recent report on securing software to secure the building blocks of cyberspace.

垃圾回收，我们都知道它是什么。见鬼，甚至白宫都赞赏垃圾回收的、内存安全的语言 像 Java 在其最近的报告中确保软件以确保网络空间的构建块。

The Java programming language garbage collector lets us write mediocre software and sort of _get away_ with it. It's dope! That said, I do take issue with the notion that it is the _original_ Java garbage collector! That honor belongs elsewhere, like perhaps https://twitter.com/jtannady/status/981547257479778307?lang=en[with Jesslyn].

Java 编程语言垃圾回收器让我们能够编写中等质量的软件并有点侥幸 通过。这很棒！话虽如此，我对它是原始 Java 垃圾回收器的概念有异议！这个荣誉属于其他地方，也许是 与 Jesslyn 有关。

And the JIT is another amazing piece of kit. It analyses frequently accessed code paths in your application and turns them into operating system and architecture-specific native code. It can only do this for some of your code though. It needs to know that the types that are in play when you compile the code are the only types that will be in play when you run the code. And some things in Java - a very dynamic language with a runtime that's more akin to that of JavaScript, Ruby, and Python - allow Java programs to do things that would violate this constraint. Things like serialization, JNI, reflection, resource loading, and JDK proxies. Remember, it's possible, with Java, to have a `String` that has as its contents a Java source code file, compile that string into a `.class` file on the filesystem, load the `.class` file into the `ClassLoader`, reflectively create an instance of that class, and then - if that class is an interface - to create a JDK proxy of that class. And if that class implements `java.io.Serializable`, it's possible to write that class instance over a network socket and load it on another JVM.  And you can do all  of that without ever having an explicit typed reference to anything beyond `java.lang.Object`! It's an amazing language, and this dynamic nature makes it a very productive language. It also frustrates the JIT's attempt at optimizations.

而 JIT 是另一件了不起的装备。它分析你应用中频繁访问的代码路径，并将它们转换为特定于操作系统和架构的本地代码。它只能为你的一些代码做到这一点。它需要知道在编译代码时播放的类型是在运行代码时播放的唯一类型。Java 中的一些事情 -- 一个非常动态的语言，其运行时更类似于 JavaScript、Ruby 和 Python -- 允许 Java 程序做一些会违反此约束的事情。像序列化、JNI、反射、资源加载和 JDK 代理。记住，用 Java，可以在一个 String 中有一个包含 Java 源代码文件的内容，将该字符串编译成文件系统上的 .class 文件，将 .class 文件加载到 ClassLoader 中，反射性地创建该类的实例，然后 -- 如果该类是接口 -- 创建该类的 JDK 代理。如果该类实现了 java.io.Serializable，则可以通过网络套接字将该类实例写入另一个 JVM。而且你可以做所有这些，而不需要对任何东西有一个明确的类型引用，超出了 java.lang.Object！这是一种了不起的语言，这种动态性使它成为一种非常高效的语言。它还阻碍了 JIT 的优化尝试。

Still, the JIT does an amazing job where it can. And the results speak for themselves. So, one wonders: why couldn't we proactively JIT the whole program? Ahead of time? And we can. There's an OpenJDK distribution called https://graalvm.org[GraalVM] that has a number of niceties that extend the OpenJDK release with extra tools like the `native-image` compiler. The native image compiler is dope. But this native image compiler has the same constraints. It can't do its magic for very dynamic things. Which is a problem. As most code - your unit testing libraries, your web frameworks, your ORMs, your logging libraries… everything! - use one or all of those dynamic behaviors.

不过，JIT 在可以的地方做得很好。结果不言自明。所以，人们不禁想知道：为什么我们不能提前为整个程序进行 JIT？我们可以。有一个名为 GraalVM 的 OpenJDK 发行版，具有一些额外的工具，如 native-image 编译器。它很棒，但这个 native-image 编译器有相同的约束，它不能为非常动态的事情施展魔法。这是一个问题。因为大多数代码 -- 你的单元测试库、你的 Web 框架、你的 ORMs、你的日志库......一切！ -- 都使用了这些动态行为中的一个或全部。

There is an escape hatch. You can furnish configuration in the form of `.json` files to the GraalVM compiler in a well-known directory: `src/main/resources/META-INF/native-image/$groupId/$artifactId/\*.json`. These `.json` files have two problems.

有一个逃生舱口。你可以以 .json 文件的形式为 GraalVM 编译器提供配置，在一个众所周知的目录中：src/main/resources/META-INF/native-image/$groupId/$artifactId/\\*.json。这些 .json 文件有两个问题。

First, the word "JSON" sounds stupid. I don't like saying the word "JAY-SAWN". As an adult, I can't believe we say these things to each other. I speak French, and in French, you'd pronounce it _jeeesã_. So, `.gison`. Much nicer. The Hokkien language has a word - _gingsong_ (happiness), which also could work. So you could have .gingsong. Pick your team! Either way,  `.json` should not stand. I'm team `.gison`, but it doesn't matter.

首先，“JSON”这个词听起来很愚蠢。我不喜欢说“JAY-SAWN”这个词。作为一个成年人，我不敢相信我们彼此说这些话。我会说法语，在法语中，你会发音为jeeesã。所以，.gison。更好。闽南语有一个词 -- gingsong（幸福），也可以工作。所以你可以有 .gingsong。选择你的团队！无论如何，.json 不应该成立。我是 .gison 团队的，但无关紧要。

The second problem is that, well, there's just so much darned of it that's required! Again, just think about all the places where your programs do these fun, dynamic things like  reflection, serialization, JDK proxies, resource loading, and JNI! It's endless. Your web frameworks. Your testing libraries. Your data access technologies. I don't have time to write artisanal, handcrafted configuration files for every program. I don't even have enough time to finish this blog!

第二个问题是，哦，需要的东西太多了！再想想你的程序在哪些地方做这些有趣的、动态的事情，如反射、序列化、JDK 代理、资源加载和 JNI！这是无尽的。你的 Web 框架、你的测试库、你的数据访问技术我没有时间为每个程序编写手工制作的配置文件。我甚至没有足够的时间来完成这篇博客！

So, instead, we'll use the Spring Boot  ahead-of-time (AOT) engine introduced in 3.0. The AOT engine analyses the beans in your Spring application and emits the requisite  configuration files for you. Nice! There's even a whole component model that you can use that extends Spring to compile time. I won't get into all of that here, but you can read my https://tanzu.vmware.com/content/white-papers/spring-boot-3[free e-book] or watch my free YouTube video introducing all things   https://www.youtube.com/watch?v=TOfYlLjXufw[Spring and AOT]. It's basically the same content, just different ways to consume it.

所以，相反，我们将使用 3.0 中引入的 Spring Boot 提前时间（AOT）引擎。AOT 引擎分析你的 Spring 应用中的 bean 并为你生成必需的配置文件。太好了！还有一个你可以使用的整个组件模型，它扩展了 Spring 到编译时。我不会在这里详细介绍所有这些，但你可以阅读我的免费电子书或观看我关于 Spring 和 AOT 的免费 YouTube 视频。基本上是相同的内容，只是消费方式不同。

So let's kick off that build with Gradle, `./gradlew nativeCompile`, or `./mvnw -Pnative native:compile` if you're using Apache Maven. You might want to skip tests on this one... This build will take a little while. Remember, it's doing an analysis of everything in your codebase - be it the libraries on the classpath, the JRE, and the classes in your code itself - to determine which types it should preserve and which it should throw out. The result is a lean, mean, lightning-fast runtime machine, but at the cost of a very, _very_ slow compilation time.

让我们用 Gradle 启动构建，./gradlew nativeCompile，或者如果你使用 Apache Maven，./mvnw -Pnative native:compile。你可能想跳过这个......这个构建会花一些时间。记住，它正在分析你代码库中的一切 -- 无论是类路径上的库、JRE 还是你代码中的类 -- 来确定它应该保留哪些类型，哪些应该丢弃。结果是一个精简、高效、快速启动的运行时机器，但代价是非常、非常 慢的编译时间。

It takes so long, in fact, that it kinda just gums up my flow. It stops me dead in my tracks, waiting. I am like the platform threads from earlier on, in this very blog: _blocked_! I get bored. Waiting. Waiting. I now finally understand this https://xkcd.com/303/[famous XKCD cartoon].

它花的时间如此之长，以至于它有点阻碍了我的流程。它让我停下来，等待。我就像前面这篇博客中提到的那些平台线程：被阻塞！我开始无聊。等待。等待。我现在终于明白这个著名的 XKCD 漫画。

Sometimes I start to hum music. Or theme songs. Or elevator music. You know what https://en.wikipedia.org/wiki/Elevator_music[elevator music] sounds like, right? Ceaseless, endless. So, I thought, wouldn't it be great if everyone heard elevator music, as well?  https://github.com/oracle/graal/issues/5327[So I asked].  I got some great responses.

有时我开始哼歌，或主题歌，或电梯音乐。你知道电梯音乐听起来像什么，对吧？无尽，无休止。所以，我想，如果每个人都听到电梯音乐，那不是很好吗？所以我问了。我得到了一些很棒的回复。

One suggested, from our friend  that we should play this elevator music from the soundtrack to the Nintendo 64 video game to the first Pierce Brosnan outing as James Bond, _Goldeneye_. I like it.

一个建议，来自我们的朋友，我们应该从 Nintendo 64 视频游戏的原声带中播放这首电梯音乐，皮尔斯·布鲁斯南（Pierce Brosnan）首次扮演詹姆斯·邦德（James Bond），Goldeneye。我喜欢它。

._Goldeneye_ had some amazing elevator music!

Goldeneye 有一些了不起的电梯音乐！

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/adinn-elevator-music.png)

One response suggested that having a beeping sound would be useful. Couldn't agree more. My stupid microwave will make a _ding!_ sound when it's done. Why couldn't my multi-million line compiler?

有一个回应建议有一个哔哔声会很有用。完全同意。我的愚蠢微波炉在完成时会发出 叮！ 声。我的多百万行编译器为什么不行呢？

._DING!_
![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/ivan-beeps.png)


And then we got this response, from another one of my favorite doctors, https://twitter.com/fniephaus[Dr. Niephaus], who works on the GraalVM team. He said that adding elevator music would only just fix the symptoms, and not the cause of the problem, which is making GraalVM even more efficient in terms of time and memory.

然后我们得到了这个回应，来自我最喜欢的专家之一，Dr. Niephaus，他在 GraalVM 团队工作。他说增加电梯音乐只会修复问题的症状，而不是导致问题的原因，即使 GraalVM 在时间和内存方面更加高效。

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/doctor-niephaus.png)

Ok. But he did share this promising prototype!

好吧。但他确实分享了这个有前景的原型！

![-](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/spring/graalvm-prototype.png)

I'm sure it'll get merged any day now...

我敢肯定它很快就会合并......

Anyway! If you check the compilation, it should be done now. It's in the `./build/native/nativeCompile/` folder, and it's called `service`. On my machine, the compilation took 52 seconds. Yeesh!

无论如何！如果你检查编译，现在应该完成了。它在 ./build/native/nativeCompile/ 文件夹中，名为 service。在我的机器上，编译花了 52 秒。哎呀！

Run it. It'll fail because, again, we're living that `git clone` &amp; run lifestyle! We didn't specify any connectivity credentials! So, run it with   environment variables specifying your SQL database connectivity details. Here's the script I used on my Machine. This will work only on a Unix-flavored operating system, and works for either Maven or Gradle.

运行它。它会失败，因为，我们生活在 git clone 然后运行的生活方式中！我们没有指定任何连接凭证！所以，用环境变量运行它，指定你的 SQL 数据库连接详细信息。这是我在我的机器上使用的脚本。这只适用于类 Unix 操作系统，并且适用于 Maven 或 Gradle。

```shell
#!/usr/bin/env bash

export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost/mydatabase
export SPRING_DATASOURCE_PASSWORD=secret
export SPRING_DATASOURCE_USERNAME=myuser

SERVICE=.
MVN=$SERVICE/target/service
GRADLE=$SERVICE/build/native/nativeCompile/service
ls -la $GRADLE && $GRADLE || $MVN
```

On my machine, it starts up in ~100 milliseconds! Like a rocket! And, obviously, it would be much faster still if I were using something like Spring Cloud Function to build AWS Lamba-style functions-as-a-service, because I wouldn't need for example to package an HTTP server. Indeed, if pure startup speed were _all_ that I really wanted, then I might even use Spring's amazing support for https://www.youtube.com/watch?v=dMhpDdR6nHw&t=2658s[Project CRaC]. That's neither here no there. I don't really care all that much about that because this is a standalone, long-lived service. What I care about is the resource usage, as represented by the https://en.wikipedia.org/wiki/RSS[Resident Set Size (RSS)]. Note the process identifier (PID) - it'll be in the logs. If the PID is, let's say, `55`, then get the RSS like this using the `ps` utility, available on virtually all Unixes:

在我的机器上，它在大约 100 毫秒内启动！像火箭一样！显然，如果我使用的是 Spring Cloud Function 来构建 AWS Lambda 风格的函数即服务（FaaS），因为我不需要打包 HTTP 服务器，它会更快。事实上，如果纯粹的启动速度是我真正想要的全部，那么我甚至可能使用 Spring 对 Project CRaC 的惊人支持。那不是这里的重点。我并不真正关心那个，因为这是一个独立的、长寿命的服务。我关心的是资源使用情况，由 驻留集大小 (RSS) 代表。注意进程标识符 (PID) -- 它会在日志中。如果 PID 是，比方说，55，那么使用 ps 实用程序获取 RSS，几乎在所有 Unix 上都可用：

```shell
ps -o rss 55
```

It'll dump out a number in kilobytes;  divide by a thousand and you'll get the number in megabytes. On my machine, it takes just over 100MB to run. You can't run Slack in that tiny amount of memory! I bet you've got individual browser tabs in Chrome taking that much, or more!

它会以千字节为单位输出一个数字；除以一千，你会得到以兆字节为单位的数字。在我的机器上，运行它只需要稍微超过 100MB 的内存。你甚至无法在那么少的内存中运行 Slack！我敢打赌你在 Chrome 中的单个浏览器标签占用的内存就有这么多，或者更多！

So, we have a program that is as concise as can be while being easy to develop and iterate on. And it uses virtual threads to give us unparalleled scalability. It runs as a standalone, self-contained, operating system and architecture-specific native image. Oh! And, it supports the freaking singularity!

所以，我们有一个程序，尽可能简洁，同时易于开发和迭代。并且它使用虚拟线程来提供无与伦比的可扩展性。它作为一个独立的、自包含的、特定于操作系统和架构的 Native Image 运行。哦！而且，它支持奇妙的奇迹！

We live in an amazing time. There's never been a better time to be a Java and Spring developer. I hope I've so persuaded you, too.

我们生活在一个了不起的时代。成为 Java 和 Spring 开发者的时机从未如此之好。我希望我也说服了你。