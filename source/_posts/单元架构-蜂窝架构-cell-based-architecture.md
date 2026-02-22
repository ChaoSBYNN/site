---
title: 单元架构 (蜂窝架构) cell-based architecture
date: 2026-01-16 08:50:15
tags: 
    - "算法"
    - "Program"
    - "Architecture"
    - "架构"
categories:
    - "算法"
    - "Program"
    - "Architecture"
    - "架构"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/Architecture.jpg"
---

- [AWS - What is a cell-based architecture?](https://docs.aws.amazon.com/zh_cn/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/what-is-a-cell-based-architecture.html)
- [AWS - Why use a cell-based architecture?](https://docs.aws.amazon.com/zh_cn/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/why-to-use-a-cell-based-architecture.html)
- [AWS - When to use a cell-based architecture?](https://docs.aws.amazon.com/zh_cn/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/when-to-use-a-cell-based-architecture.html)

## 什么是基于单元的架构？

A cell-based architecture comes from the concept of a bulkhead in a ship, where vertical partition walls subdivide the ship's interior into self-contained, watertight compartments. Bulkheads reduce the extent of seawater flooding in case of damage and provide additional stiffness to the hull girder.
基于单元的建筑源自船舶舱壁的概念，垂直隔墙将船内分割成自成一体、防水的舱室。隔舱可以减少海水进水的程度，并在船体梁梁上提供额外的刚性。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/cell-based/failure-isolation-using-bulkheads.jpg)

Isolating failures using bulkheads 利用隔板隔离故障

On a ship, bulkheads ensure that a hull breach is contained within one section of the ship. In complex systems, this pattern is often replicated to allow fault isolation. Fault isolated boundaries restrict the effect of a failure within a workload to a limited number of components. Components outside of the boundary are unaffected by the failure.
在船只上，隔舱壁确保船体破损被控制在 船。在复杂系统中，这种模式常被复制以实现故障隔离。 故障隔离边界限制了工作负载中故障的影响，仅限于有限数量的组件。边界外的组件不受失效影响。

Using multiple fault isolated boundaries, you can limit the impact on your workload. When provisioning a new customer or tenant, or applying a workload change, you can do this gradually, compartment by compartment, or in other words, isolation boundary by isolation boundary. This way, when a failure occurs, a smaller number of customers or resources will be impacted. On AWS, customers can use multiple Availability Zones and AWS Regions to provide fault isolation, but the concept of fault isolation can be extended to your workload's architecture as well.
通过设置多个故障隔离边界，你可以限制对工作负载的影响。在为新客户或租户配置，或应用工作负载变更时，你可以逐步进行，逐个隔块，换句话说，逐个隔离边界。这样，当发生故障时，受影响的客户或资源数量会减少。在 AWS 上，客户可以使用多个可用性区和 AWS 区域来实现故障隔离，但故障隔离的概念也可以扩展到你的工作负载架构中。

The overall workload is partitioned by a partition key. This key needs to align with the grain of the service, or the natural way that a service's workload can be subdivided with minimal cross-cell interactions. Examples of partition keys are customer ID, resource ID, or any other parameter easily accessible in most API calls. A cell routing layer distributes requests to individual cells based on the partition key and presents a single endpoint to clients.
整体工作负载由分区键划分。这个密钥需要与服务的纹理保持一致， 或者说，服务工作负载可以自然细分的方式 细胞间相互作用极少。分区键的例子 是客户 ID、资源 ID 或其他任何参数，都很容易 大多数 API 调用中可访问。小区路由层分布 基于分区键和 向客户端展示单一端点。

A cell-based architecture uses multiple isolated instances of a workload, where each instance is known as a cell. Each cell is independent, does not share state with other cells, and handles a subset of the overall workload requests. This reduces the potential impact of a failure, such as a bad software update, to an individual cell and the requests that it's processing. If a workload uses 10 cells to service 100 requests, when a failure occurs in one cell, 90% of the overall requests would be unaffected by the failure.
基于单元的架构使用多个孤立的工作负载实例，每个实例称为单元 。每个单元独立，不与其他单元共享状态，并处理部分整体工作负载请求。这减少了失败（如软件更新不良）对单个单元及其处理请求的潜在影响。如果一个工作负载使用 10 个单元来处理 100 个请求，当一个单元发生故障时，90%的整体请求将不受故障影响。

With cell-based architectures, many common types of failure are contained within the cell itself, providing additional fault isolation. These fault boundaries can provide resilience against failure types that otherwise are hard to contain, such as unsuccessful code deployments or requests that are corrupted or invoke a specific failure mode (also known as poison pill requests).
在基于单元的架构中，许多常见的故障类型都包含在单元内部，提供了额外的故障隔离。这些故障边界可以为那些难以控制的故障类型提供韧性，比如未成功的代码部署、损坏请求或触发特定失败模式（也称为毒丸请求 ）。

### A typical workload  典型的工作量

To make it clearer, in the following diagram, we have a typical application divided into three layers. In this context, this application would be serving requests from 100% of clients. In the event of a failure, or a change in the application, 100% of customers would be impacted.
为了更清楚地说明，下图中我们有一个典型的应用，分为三层。在这种情况下，这个应用将满足100%客户端的请求。如果发生故障或应用变更，100%的客户都会受到影响。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/cell-based/typical-workload.jpg)

A typical workload 典型的工作量

### A workload with cell-based architecture 基于单元架构的工作负载

Rather than build out services as single-image systems, we propose a different approach: break your services down internally into cells and build thin layers to route traffic to the right cells. This type of architecture can be zonal, regional, or global.
我们提出另一种方法：将服务内部拆分为单元，构建薄层以将流量路由到正确的单元。这种类型的建筑可以是区域性的、区域性的或全球性的。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/cell-based/cell-based-architecture.jpg)

A cell-based architecture 基于单元的架构

* The cell-based architecture has the following components, which will be further explored later in this guidance:
    基于单元的架构包含以下组件，后续将在本指南中进一步探讨：
* Cell router — We also refer to this layer as the thinnest possible layer, with the responsibility of routing requests to the right cell, and only that.
    蜂窝路由器 ——我们也称这一层为 最薄的层 ，负责将请求路由到正确的单元，仅此而已。
* Cell — A complete workload, with everything needed to operate independently.
    小区 ——一个完整的工作负载，所有设备都需要独立运作。
* Control plane — Responsible for administration tasks, such as provisioning cells, de-provisioning cells, and migrating cell customers.
    控制平面 ——负责管理任务，如配置小区、取消配置小区和迁移小区客户。

Building a cell-based architecture doesn't necessarily mean having to double, triple, or more your application's infrastructure. It might be that your application has 30 hosts, and in a cell-based architecture it has the same 30 hosts, but with a cell router and with tasks that are distributed or grouped between cells.
构建基于单元的架构并不一定意味着必须将应用基础设施加倍、三倍甚至更多。你的应用可能有30台主机，而在基于小区的架构中，它也有同样的30台主机，但配备了小区路由器，任务分布或分组在小区之间。

## Why use a cell-based architecture? 为什么要采用基于单元的架构？

These are the main advantages of a cell-based architecture.
这些是基于单元架构的主要优势。

### Scale-out over scale-up  规模扩大而非规模化

* Scaling up, or accommodating growth by increasing the size of a system's component (such as a database, server, or subsystem) is a natural and straightforward way to scale. Scaling out, on the other hand, accommodates growth by increasing the number of system components (such as databases, servers, and subsystems), and dividing the workload such that the load on any component stays bounded over time despite the overall increase in workload. The task of dividing the workload can make scaling out more challenging than scaling up, particularly for stateful systems, but has well-understood benefits:
    通过扩大系统组件规模来适应增长 （例如数据库、服务器或子系统）是一种自然且直接的扩展方式。 而缩放扩展则通过增加 系统组件数量 （如数据库、服务器和子系统），以及如何划分工作负载，使得尽管整体工作负载增加，任何组件的负载仍保持有界。分配工作负载的任务可能使扩展比扩展更具挑战性，尤其是对于有状态系统，但其优点是众所周知的：
* Workload isolation: Dividing the workload across components means workloads are isolated from each other. This provides failure containment and narrows the impact of issues, such as deployment failures, poison pills, misbehaving clients, data corruption, and operational mistakes.
    工作量隔离： 将工作负载划分到各个组件意味着工作负载彼此隔离。这提供了故障控制，并缩小了部署失败、毒丸、不当客户端、数据损坏和作错误等问题的影响。
* Maximally-sized components: Accommodating growth by increasing the number of components rather than increasing component size means that the size of each component can be capped to a maximum size. This reduces the risk of surprises from non-linear scaling factors and hidden contention points present in scale-up systems.
    最大尺寸组件： 通过增加组件数量而非增加组件规模来适应增长，意味着每个组件的规模可以被限制在最大范围内。这降低了非线性缩放因子和放大系统中隐藏争点带来的意外风险。
* Not too big to test: With maximally-sized components, the components are no longer too big to test. These components can be stress tested and pushed past their breaking point to understand their safe operating margin. This approach does not address testing the overall scaled out system composed of these components, but if the majority of the complexity and risk of the system sits in stress-tested components (and it should), the level of test coverage and confidence should be significantly higher.
    不大而难以测试： 对于最大尺寸的组件，组件不再太大而无法测试。这些组件可以经过压力测试，并将其推至极限，以了解其安全的工作余裕。该方法不涉及对由这些组件组成的整体扩展系统进行测试，但如果系统大部分复杂性和风险都集中在经过压力测试的组件中（而且应该如此），那么测试覆盖率和置信度应当显著提高。

Cells change our approach from scaling up to scaling out. Each cell, a complete independent instance of the service, has a fixed maximum size. Beyond the size of a single cell, regions grow by adding more cells. This change in design doesn't change the customer experience of your services. Customers can continue to access the services as they do today.
细胞改变了我们从放大到放大的方式。每个单元是服务的完全独立实例，最大大小固定。超过单个单元格的规模，区域通过增加更多单元格来增长。这种设计变化不会改变客户对服务的体验。客户可以像现在一样继续使用这些服务。

### Lower scope of impact 影响范围较低

Breaking a service up into multiple cells reduces the scope of impact. Cells represent bulkheaded units that provide containment for many common failure scenarios. When properly isolated from each other, cells have failure containment similar to what we see with Regions. It's highly unlikely for a service outage to span multiple Regions. It should be similarly unlikely for a service outage to span multiple cells.
将服务拆分为多个单元可以减少影响范围。单元代表隔板单元，为许多常见故障场景提供隔离。当彼此适当隔离时，细胞具有类似区域的失效容纳。服务中断跨多个区域的可能性极低。服务中断跨越多个小区的可能性也同样不大。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/cell-based/scope-of-impact.jpg)

Cell-based architectures can reduce scope of impact 基于单元的架构可以缩小影响范围

### Higher scalability or cells as a unit scale 更高的可扩展性或单元单元作为单位尺度

As recommended in Manage service quotas and constraints in the Well-Architected Framework, for your workloads, defining, testing, and managing the limits and capacity of a cell is also essential. Knowing and monitoring this capacity, it's possible to define limits, and scale your workload by adding new cells to your architecture, thus scaling it out.
正如《管理良好架构框架中的服务配额与约束》中建议的，针对您的工作负载，定义、测试和管理单元单元的限制和容量同样至关重要。了解并监控这些容量后，可以定义限制，并通过向架构中添加新的单元来扩展工作负载，从而实现扩展。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/cell-based/scale-out-with-cells.jpg)

Scale-out with multiple cells 多单元的扩展

Cell-based architectures scale-out rather than scale-up, and are inherently more scalable. This is because when scaling up, you can reach the resource limits of a particular service, instance, or AWS account. However, scaling out your workload within an Availability Zone, Region, and AWS account, you can avoid reaching the limits of a specific service or resource. When building cells with a fixed size and known and testable limits, it is possible to add new cells that are in accordance with the limits of the same cited resources.
基于单元的架构是扩展而非扩展，且本质上更具可扩展性。这是因为在扩展时，你可能会达到某个服务、实例或 AWS 账户的资源限制。然而，在可用性区、区域和 AWS 账户内扩展工作负载，可以避免达到特定服务或资源的限制。在构建固定大小且已知且可测试的单元时，可以添加符合上述资源极限的新单元。

### Higher testability  更高的可检性

Testing distributed systems is a challenging task and is amplified as the system grows. The capped size of cells allows for well-understood and testable maximum scale behavior. It is easier to test cells as compared to bulk services since cells have a limited size. It is impractical for cost reasons for large-scale services to regularly simulate the entire workload of all their tenants, but it is reasonable to simulate the largest workload that can fit into a cell, which should match the largest workload that a single customer can send to your application.
测试分布式系统是一项具有挑战性的任务，随着系统的发展，这一挑战会更加显著。单元格的上限尺寸允许实现被充分理解且可测试的最大规模行为。相比批量服务，检测单元更容易，因为单元体积有限。出于成本原因，大型服务定期模拟所有租户的全部工作负载是不切实际的，但合理的做法是模拟一个单元中能容纳的最大工作负载，这应与单个客户能发送到你应用的最大工作负载相匹配。

### Higher mean time between failure (MTBF) 平均失效间隔时间（MTBF）

Not only is the scope of impact of an outage reduced with cells, but so is the probability of an outage. Cells have a consistent capped size that is regularly tested and operated, eliminating the every day is a new adventure dynamic.
小区不仅减少了停电的影响范围，停电的概率也相应降低。细胞有一个固定的上限尺寸，需要定期测试和作，消除日常活动是一种新的冒险动态。

As in day-to-day operations, your customers are distributed among the cells and a problem can be identified locally. The same goes for new versions of applications that can only be applied to a small number of cells (up to just one) and when a failure is identified, rollback.
就像日常运营一样，你的客户分布在各个小区之间，问题可以在本地被发现。同样，对于只能应用于少量单元（最多一个）的新版本应用，发现故障时回滚。

With your customers spread across cells, for example, 10, you now have 10% of your customers in each cell. This, added to a gradual deployment strategy that we will describe later in this guidance, allows you to better manage system changes, and contain the scope of failures such as code or traffic spikes in some cells while others remain stable and unaffected, thus increasing the average time between application failures.
例如，当你的客户分散在不同单元格中，比如10个单元，你现在每个单元里有10%的客户。这结合我们稍后将在本指南中介绍的渐进部署策略，使您能够更好地管理系统变更，并在某些单元中控制代码或流量激增等故障范围，而其他单元则保持稳定且不受影响，从而延长应用失败的平均间隔时间。

### Lower mean time to recovery (MTTR) 较低的平均恢复时间（MTTR）

Cells are also easier to recover, because they limit the number of hosts that need to be analyzed and touched for problem diagnosis and the deployment of emergency code and configuration. The predictability of size and scale that cells bring also make recovery more predictable in the event of a failure.
小区也更容易恢复，因为它们限制了需要分析和触碰的问题诊断以及紧急代码和配置部署的主机数量。细胞带来的大小和规模的可预测性也使得故障时的恢复更为可预测。

### Higher availability  更高的可用性

A natural conclusion is that cell-based architectures should have the same overall availability as monolithic systems, because a system with n cells will have n times as many failure events, but each with 1/nth of the impact. But the higher MTBF and lower MTTR afforded by cells means fewer shorter failures events per cell, and higher overall availability.
自然的结论是，基于单元的架构应具有与单片系统相同的整体可用性，因为拥有 n 个单元的系统将具有 失败事件数是 n 倍，但每个事件 仅为冲击的 1/n。但小区提供的更高 MTBF 和更低的 MTTR 意味着每个小区的短故障事件更少，整体可用性更高。

There is also the availability defined by:
还有一种可用性定义为：

```bash
#successful requests / #total requests
```

With cells, you can minimize the amount of time the numerator is zero.
用单元格，你可以最小化分子为零的时间。

### More control over the impact of deployments and rollbacks 对部署和回滚影响的更多控制

Like one-box and Single-AZ deployments, cells provide another dimension in which to phase deployments and reduce scope of impact from problematic deployments. Further, the first cell deployed to in a phased cell deployment can be a canary cell, and each cell can have its own canary with synthetic and other non-critical workloads to further reduce the impact of a failed deployment.
与单一机和单区部署类似，单元为分阶段部署提供了另一个维度，并减少问题部署带来的影响范围。此外，分阶段单元部署的第一个单元可以是金丝雀单元，每个单元都可以拥有自己的金丝雀单元，配备合成及其他非关键工作负载，以进一步减少部署失败的影响。

Applications that do not use cell-based architecture can also benefit from strategies such as canary deployment. But what the cells bring is the possibility of a combination of a canary deployment strategy being applied in a context of even lesser impact.
不使用基于单元架构的应用也可以受益于诸如金丝雀部署等策略。但这些细胞带来的是，在影响更小的环境中，可能采用金丝雀部署策略的组合。

## When to use a cell-based architecture? 什么时候应该使用基于单元的架构？

There are applications that serve such a large number of customers that interruption of all customers is unacceptable, either for reputation, or financial reasons, where every unavailable second has a big impact. Workloads that have the following characteristics can benefit from a cell-based architecture:
有些应用服务于如此庞大的客户，以至于中断所有客户是不可接受的，无论是出于声誉还是经济原因，每一秒的不可用都会产生巨大影响。具有以下特性的工作负载可以从基于单元架构中受益：

* Applications where any downtime can have a huge negative impact on customers.
    任何停机时间都可能对客户产生巨大负面影响的应用。
* FSI customers with workloads critical to economic stability.
    FSI 客户承担着对经济稳定至关重要的工作量。
* Ultra-scale systems that are too big/critical to fail.
    那些规模过大、关键到不能倒的超大规模系统。
* Less than 5 seconds of Recovery Point Objective (RPO).
    恢复点目标（RPO） 不到 5 秒。
* Less than 30 seconds of Recovery Time Objective (RTO).
    恢复时间目标（RTO） 不到 30 秒。
* Multi-tenant services where some tenants require fully dedicated tenancy (meaning, their own dedicated cell).
    多租户服务，有些租户需要完全专用的租户（即他们自己的专用手机）。

A question to ponder regarding your workload is this: "Is it better for 100% of customers to experience a 5% failure rate, or 5% of customers to experience a 100% failure rate?"
关于你的工作量，有个问题需要思考： “让 100%的客户体验 5%的失败率更好，还是让 5%的客户体验 100%的失败率更好？”

Cell-based architecture will not be a good choice for all your workloads, but it can bring great benefits for some your most critical workloads.
基于单元架构可能不适合所有工作负载，但它能为一些最关键的工作带来巨大好处。

Implementing a cell-based architecture is not a simple task, among the disadvantages are:
实现基于单元的架构并非易事，缺点包括：

* Increase in the complexity of the architecture due to the redundancy of infrastructure and components.
    由于基础设施和组件的冗余，架构的复杂性增加。
* High cost of infrastructure and services, although utilization based fee structures like Amazon EC2 Reserved Instances (RIs) and saving plans help close this delta.
    基础设施和服务成本高昂，尽管基于利用率的费用结构如亚马逊 EC2 预留实例（RI）和储蓄计划有助于弥补这一差距。
* Requires specialized operational tools and practices to operate these multiple replicas (cells) of the workload.
    需要专门的作工具和作方法来作这些工作负载的多个副本（单元）。
* Necessity to invest in a cell routing layer.
    需要投资一个小区路由层。
