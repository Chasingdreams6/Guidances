### 通讯方式
服务之间使用轻量级通信机制（通常是 HTTP RESTFUL API）进行通讯。

### 服务注册与发现（Eureka）
- Eureka Server：Eureka 服务注册中心，主要用于提供服务注册功能。当微服务启动时，会将自己的服务注册到 Eureka Server。Eureka Server 维护了一个可用服务列表，存储了所有注册到 Eureka Server 的可用服务的信息，这些可用服务可以在 Eureka Server 的管理界面中直观看到。
- Eureka Client：Eureka 客户端，通常指的是微服务系统中各个微服务，主要用于和 Eureka Server 进行交互。在微服务应用启动后，Eureka Client 会向 Eureka Server 发送心跳（默认周期为 30 秒）。若 Eureka Server 在多个心跳周期内没有接收到某个 Eureka Client 的心跳，Eureka Server 将它从可用服务列表中移除（默认 90 秒）。 

Eureka 实现服务注册与发现的流程如下：
- 搭建一个 Eureka Server 作为服务注册中心；
- 服务提供者 Eureka Client 启动时，会把当前服务器的信息以服务名（spring.application.name）的方式注册到服务注册中心；
- 服务消费者 Eureka Client 启动时，也会向服务注册中心注册；
- 服务消费者还会获取一份可用服务列表，该列表中包含了所有注册到服务注册中心的服务信息（包括服务提供者和自身的信息）；
  在获得了可用服务列表后，服务消费者通过 HTTP 或消息中间件远程调用服务提供者提供的服务。

### 负载均衡与服务调用（Ribbon）

#### 服务端负载均衡
服务端负载均衡是在客户端和服务端之间建立一个独立的负载均衡服务器，该服务器既可以是硬件设备（例如 F5），也可以是软件（例如 Nginx）。这个负载均衡服务器维护了一份可用服务端清单，然后通过心跳机制来删除故障的服务端节点，以保证清单中的所有服务节点都是可以正常访问的。

当客户端发送请求时，该请求不会直接发送到服务端进行处理，而是全部交给负载均衡服务器，由负载均衡服务器按照某种算法（例如轮询、随机等），从其维护的可用服务清单中选择一个服务端，然后进行转发。

- 不需要服务注册中心，但是有负载均衡服务器

#### 客户端负载均衡
客户端负载均衡是将负载均衡逻辑以代码的形式封装到客户端上，即负载均衡器位于客户端。客户端通过服务注册中心（例如 Eureka Server）获取到一份服务端提供的可用服务清单。有了服务清单后，负载均衡器会在客户端发送请求前通过负载均衡算法选择一个服务端实例再进行访问，以达到负载均衡的目的；

- 服务注册中心，在客户端负载均衡中，所有的客户端和服务端都需要将其提供的服务注册到服务注册中心上，没有负载均衡服务器

#### Rest Template
RestTemplate 是 Spring 家族中的一个用于消费第三方 REST 服务的请求框架。RestTemplate 实现了对 HTTP 请求的封装，提供了一套模板化的服务调用方法。通过它，Spring 应用可以很方便地对各种类型的 HTTP 请求进行访问。

RestTemplate 针对各种类型的 HTTP 请求都提供了相应的方法进行处理，例如 HEAD、GET、POST、PUT、DELETE 等类型的 HTTP 请求，分别对应 RestTemplate 中的 headForHeaders()、getForObject()、postForObject()、put() 以及 delete() 方法。

#### Ribbon
Ribbon 是一个客户端的负载均衡器，它可以与 Eureka 配合使用轻松地实现客户端的负载均衡。Ribbon 会先从 Eureka Server（服务注册中心）去获取服务端列表，然后通过负载均衡策略将请求分摊给多个服务端，从而达到负载均衡的目的。

Spring Cloud Ribbon 提供了一个 IRule 接口，该接口主要用来定义负载均衡策略，它有 7 个默认实现类，每一个实现类都是一种负载均衡策略。

### OpenFeign
OpenFeign 全称 Spring Cloud OpenFeign，它是 Spring 官方推出的一种声明式服务调用与负载均衡组件，它的出现就是为了替代进入停更维护状态的 Feign。OpenFeign内部集成了Ribbon，并支持Spring MVC注解

### Hystrix 服务熔断与降级组件
如果一个微服务发生崩溃，依赖于它的微服务都会进入阻塞状态进而崩溃，引发雪崩效应。为了防止此类事件的发生，微服务架构引入了“熔断器”的一系列服务容错和保护机制。
在微服务系统中，Hystrix 能够帮助我们实现以下目标：

- 保护线程资源：防止单个服务的故障耗尽系统中的所有线程资源。
- 快速失败机制：当某个服务发生了故障，不让服务调用方一直等待，而是直接返回请求失败。
- 提供降级（FallBack）方案：在请求失败后，提供一个设计好的降级方案，通常是一个兜底方法，当请求失败后即调用该方法。
- 防止故障扩散：使用熔断机制，防止故障扩散到其他服务。
- 监控功能：提供熔断器故障监控组件 Hystrix Dashboard，随时监控熔断器的状态。

可以提供服务器端服务降级、客户端服务降级。

### Gateway API网关
API 网关是一个搭建在客户端和微服务之间的服务，我们可以在 API 网关中处理一些非业务功能的逻辑，例如权限验证、监控、缓存、请求路由等。

API 网关就像整个微服务系统的门面一样，是系统对外的唯一入口。有了它，客户端会先将请求发送到 API 网关，然后由 API 网关根据请求的标识信息将请求转发到微服务实例。

常见的 API 网关实现方案主要有以下 5 种：
- Spring Cloud Gateway
- Spring Cloud Netflix Zuul
- Kong
- Nginx+Lua
- Traefik

### Spring Cloud 动态路由
默认情况下，Spring Cloud Gateway 会根据服务注册中心（例如 Eureka Server）中维护的服务列表，以服务名（spring.application.name）作为路径创建动态路由进行转发，从而实现动态路由功能。
修改 application.yml 文件，将Route的Uri地址修改为
```yml
lb:/service-name
```
lb表示开启负载均衡。

### Spring config center
默认的微服务架构，每个微服务各自有各自的配置文件，管理难度大，安全性低。

Spring cloud config可以将各个微服务的配置文件集中存储在一个外部的存储仓库或系统（例如 Git 、SVN 等）中，对配置的统一管理，以支持各个微服务的运行。
Spring Cloud Config 工作流程如下：
- 开发或运维人员提交配置文件到远程的 Git 仓库。
- Config 服务端（分布式配置中心）负责连接配置仓库 Git，并对 Config 客户端暴露获取配置的接口。
- Config 客户端通过 Config 服务端暴露出来的接口，拉取配置仓库中的配置。
- Config 客户端获取到配置信息，以支持服务的运行。

朴素的实现的问题是，当git上的配置信息更新之后，server可以及时获取最新的消息，但client必须重启。可以使用spring cloud bus来实现自动刷新配置信息

### Spring cloud alibaba
Spring Cloud Alibaba 的应用场景如下：
- 大型复杂的系统，例如大型电商系统。
- 高并发系统，例如大型门户网站、商品秒杀系统。
- 需求不明确，且变更很快的系统，例如创业公司业务系统。

Spring cloud nacos 集合了eureka和config center的功能

- Nacos server集成了服务注册中心，配置中心的功能，可以从官网直接下载
- Nacos client是client，在server中自动注册，可以分为service provider-consumer.

Nacos 支持部署server集群，可以使用nginx管理

### Sentinel
sentinel是alibaba提供的spring cloud hystrix，但比hystrix更加强大。

应用场景：秒杀，高并发。
