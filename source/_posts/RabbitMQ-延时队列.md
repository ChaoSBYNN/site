---
title: RabbitMQ 延时队列
date: 2024-08-05 16:37:25
tags: 
    - "RabbitMQ"
categories:
    - "Program"
    - "RabbitMQ"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/banner/RabbitMQ.png)"
excerpt: "RabbitMQ 延时队列 Delay Queue"
---

## 安装

要在 RabbitMQ 中安装和配置延时队列插件(`rabbitmq_delayed_message_exchange`)，你需要执行以下步骤：

### 使用 RabbitMQ 包管理器安装延时消息交换机插件

1. 确保 RabbitMQ 已经安装

   首先，确保你已经安装了 RabbitMQ。你可以通过以下命令检查 RabbitMQ 服务是否在运行：

   ```bash
   sudo systemctl status rabbitmq-server
   ```

2. 安装插件

   RabbitMQ 提供了 `rabbitmq_delayed_message_exchange` 插件来支持延时消息。使用以下命令来安装插件：

   ```bash
   sudo rabbitmq-plugins enable rabbitmq_delayed_message_exchange
   ```

3. 检查插件是否安装成功

   你可以通过以下命令查看插件列表，确认插件是否启用：

   ```bash
   sudo rabbitmq-plugins list
   ```

   检查输出中是否包含 `rabbitmq_delayed_message_exchange` 插件，并且状态为 `E`(已启用)。

   ```bash
    Listing plugins with pattern ".*" ...
    Configured: E = explicitly enabled; e = implicitly enabled
    | Status: * = running on rabbit@dev-dms-appserver
    |/
    [E*] rabbitmq_delayed_message_exchange 3.12.0
   ```

## 配置

### 管理界面配置延时交换机

你可以通过 RabbitMQ 管理界面或命令行创建和配置延时交换机。

- 通过 RabbitMQ 管理界面

  1. 登录 RabbitMQ 管理界面(通常在 `http://localhost:15672`)。
  2. 转到 "Exchanges" 页面。
  3. 点击 "Add a new exchange"。
  4. 填写表单：
     - Name: `delayed_exchange`(可以自定义名称)
     - Type: 选择 `x-delayed-message` 类型
     - Durable: 勾选(可选，根据需求)
     - Auto-delete: 取消勾选(可选，根据需求)
  5. 在 "Arguments" 部分，添加以下参数：
     - Key: `x-delayed-type`
     - Value: `direct`(或其他类型，如 `topic`，根据你的需求)

- 通过命令行工具

  使用 `rabbitmqadmin` 工具(你可以从 RabbitMQ 下载并安装)：

  ```bash
  rabbitmqadmin declare exchange name=delayed_exchange type=x-delayed-message durable=true arguments='{"x-delayed-type":"direct"}'
  ```

### HTTP API 创建延时队列

你可以通过创建一个延时队列并发送消息来验证插件是否按预期工作。

通过 RabbitMQ Management HTTP API 测试：

1. 创建交换机：

   使用延时交换机类型创建一个交换机。你可以使用以下 cURL 命令：

   ```bash
   curl -u username:password -X PUT -H "Content-Type: application/json" \
   -d '{"type": "x-delayed-message", "arguments": {"x-delayed-type": "direct"}}' \
   http://localhost:15672/api/exchanges/%2f/delayed_exchange
   ```

   这会创建一个名为 `delayed_exchange` 的延时交换机，`%2f` 是默认虚拟主机的 URL 编码形式。

2. 创建队列：

   ```bash
   curl -u username:password -X PUT -H "Content-Type: application/json" \
   -d '{"durable": true}' \
   http://localhost:15672/api/queues/%2f/delayed_queue
   ```

   这会创建一个名为 `delayed_queue` 的队列。

3. 绑定交换机到队列：

   ```bash
   curl -u username:password -X POST -H "Content-Type: application/json" \
   -d '{"routing_key": "delayed_routing_key"}' \
   http://localhost:15672/api/bindings/%2f/e/delayed_exchange/q/delayed_queue
   ```

4. 发布延时消息：

   ```bash
   curl -u username:password -X POST -H "Content-Type: application/json" \
   -d '{"properties": {"headers": {"x-delay": 10000}}, "routing_key": "delayed_routing_key", "payload": "Test message"}' \
   http://localhost:15672/api/exchanges/%2f/delayed_exchange/publish
   ```

   这个命令会将一条延时 10 秒的消息发布到 `delayed_exchange` 交换机。

### Spring Boot 应用中使用延时交换机

确保你的 Spring Boot 应用配置了正确的延时交换机设置。以下是一个 Spring Boot 配置的示例：

```java
import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitConfig {

    @Bean
    public CustomExchange delayedExchange() {
        return new CustomExchange("delayed_exchange", "x-delayed-message", true, false,
                Map.of("x-delayed-type", "direct"));
    }

    @Bean
    public Queue delayedQueue() {
        return new Queue("delayed_queue", true);
    }

    @Bean
    public Binding binding(Queue delayedQueue, CustomExchange delayedExchange) {
        return BindingBuilder.bind(delayedQueue).to(delayedExchange).with("delayed_routing_key").noargs();
    }
}
```

## 测试

### 发布延时消息

使用以下代码发布延时消息到延时交换机：

```java
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MessageService {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    public void sendDelayedMessage(String message) {
        rabbitTemplate.convertAndSend("delayed_exchange", "delayed_routing_key", message, messagePostProcessor -> {
            messagePostProcessor.getMessageProperties().setHeader("x-delay", 10000); // 设置延时为 10 秒
            return messagePostProcessor;
        });
    }
}
```

### 验证配置

- 确保 RabbitMQ 服务已重启以应用更改：

  ```bash
  sudo systemctl restart rabbitmq-server
  ```

- 使用 RabbitMQ 管理界面验证交换机和队列配置是否正确。

- 发送测试消息，检查延时是否生效。

如果在设置过程中遇到问题，可以查看 RabbitMQ 的日志文件，通常位于 `/var/log/rabbitmq/`，以获取更多调试信息。
