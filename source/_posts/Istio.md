---
title: Istio
date: 2019-04-26 02:21:15
categories: DevOps
tags:
  - k8s
  - Service Mesh
  - Microservice
  - 微服务
  - 服务网格
---


参考:

- Istio docs: <https://istio.io/docs>
- Istio中文文档: <https://istio.io/zh/docs/>
- Istio github: <https://github.com/istio/istio>

<br>

环境:

- RHEL7x86_64
- Istio v1.1










<br/>
<br/>

---

<!--more-->

<br/>
<br/>


















# 概念

[Concepts](https://istio.io/docs/concepts/)


<br/>


## Istio是什么

[What is Istio?](https://istio.io/docs/concepts/what-is-istio/)


Istio允许您连接(connect)，保护(secure)，控制(control)和观察(observe)服务。
在较高的层级上，Istio有助于降低部署的复杂性，减轻开发团队的压力。它是一个完全开源的**服务网格(service mesh)**，可透明地分层到现有的分布式应用程序上。它也是一个平台，包括可以将其集成到任何日志记录平台或策略系统的API。Istio的多样化功能使你能够成功，高效地运行分布式微服务(microservice)架构，并提供安全，连接和监控微服务的统一方法。


<br/>


### 服务网格

Service Mesh


Istio解决了开发人员和运营商在单片应用程序向分布式微服务架构过渡时所面临的挑战。有必要详细了解Istio服务网格。

术语服务网格用于描述构成此类应用程序的微服务网络以及它们之间的交互。随着服务网格的大小和复杂性的增加，理解和管理变得更加困难。其要求包括:

- **发现(discovery)**
- **负载均衡(load balancing)**
- **故障恢复(failure recovery)**
- **指标(metrics)**
- **监控(monitoring)**

服务网格通常还具有更复杂的操作要求，如:

- **A/B测试**
- **金丝片部署(canary rollouts)**
- **速率限制(rate limiting)**
- **访问控制(access control)**
- **端到端认证()*end-to-end authentication*

Istio作为一个整体提供对服务网格的行为洞察和操作控制。



<br/>
<br/>



### 为什么使用它

Why use Istio?


通过负载均衡，服务到服务的身份认证，监控...使用服务代码中很少或不需要更改代码，Istio可以轻松创建已部署的服务网格。通过在整个环境中部署特殊的sidecar代理来拦截服务的Istio支持，该代理拦截微服务之间的所有网络通信，然后使用其控制平面配置和管理Istio。包括:

- HTTP, gRPC, WebSocket, TCP流量的自动负载均衡；
- 通过丰富的路由规则，重试(retries)，故障转移(failovers)，故障注入(fault injection)，对流量欣慰 进行细粒度控制；
- 可插入的策略层和API配置，支持访问控制，速率限制和配额；
- 集群中所有流浪的自动度量、日志和追踪，包括集群的ingress, egress；
- 通过强大的基于身份的认证和授权，在鸡群中实现安全的服务到服务的通信。



<br/>
<br/>



### 核心功能

Core features


<br/>


#### 流量管理

Traffic management


通过Istio简单的规则配置和流量路由，你可以控制服务之间的流量和API调用。它简化了服务级别的属性配置，如熔断器(circuit breakers)，超时(timeouts)，重试(retries)，并且可以轻松设置A/B测试，金丝片部署(canary rollouts)，基于百分比流量分割的分阶段部署等重要任务。

通过更好地了解流量和开箱即用的故障恢复功能，你可在问题出现之前发现问题，使调用更加可靠、网络更加强大。


<br/>
<br/>


#### 安全

Security


Istio的安全功能使开发人员可以更加专注于应用程序级别的安全性。Istio提供了底层安全通信信道，并大规模管理服务通信的认证、授权和加密。使用Istio，服务通信在默认情况下是安全的，允许你跨多种协议和运行时一致地实施策略。所有这些基本都不用对应用程序进行更改。

虽然Istio与平台无关，但与k8s网络策略一起使用时，其优势更大，包括在网络层和应用层保护`pod-to-pod`或`service-to-service`通信的能力。


<br/>
<br/>


#### 可观察性

Observability


Istio强大的追踪、监控和日志记录功能可以让你更深入了解服务网格部署。通过Istio的监控功能，真正了解服务性能如何影响上下游(upstream, downstream)的功能，而其自定义的仪表盘可提供对所有服务性能的可视性，并让你了解该性能如何影响你的其他进程。

Istio的**混合器（Mixer)**组件负责策略控制和遥测收集。它提供后端抽象和中间媒介，将Istio的其余部分与各个基础架构后端的实现细节隔离开来，并为运营商提供对网格网络和基础架构后端之间所有交互的细粒度控制。

这些功能使你可以更有效地设置，监控和实施服务上的SLOs。当然，最重要的是，你可以快速有效地检测和修复问题。


<br/>
<br/>


#### 平台支持

Platform support


Istio是独立于平台的，旨在各种环境中运行。包括跨云，内在部署，k8s，Mesos...
你可在k8s上部署Istio，或在带有Nomad的Consul上部署它。Istio目前支持:

- Service deployment on Kubernetes
- Services registered with Consul
- Services running on individual virtual machines


<br/>
<br/>


#### 集成和自定义

Integration and customization


可以扩展和自定义Istio的策略实施组件，来与现有的ACL，日志记录，监控，配额，审计等方案集成。




<br/>
<br/>




### 架构

Architecture


Istio服务网格逻辑上分为**数据平面(data plane)**和**控制平面(data plane)**。

- 数据平面由一组以 sidecar 方式部署的**智能代理(Envoy)**组成。这些代理可以调节和控制微服务及Mixer之间所有的网络通信。
- 控制平面负责管理和配置代理来路由流量。此外控制平面配置Mixer以实施策略和收集遥测数据。

![](images/Istio/arch.svg)


<br/>


#### Envoy

Istio使用**Envoy**代理的扩展版本，Envoy是以`C++`开发的高性能代理，用于调解服务网格中所有服务的所有入站和出站流量。
Envoy 的许多内置功能被 Istio 发扬光大，如:

- 动态服务发现(Dynamic service discovery)
- 负载均衡(Load balancing)
- TLS termination
- HTTP/2 and gRPC proxies
- 熔断器(Circuit breakers)
- 健康检查(Health checks)
- 基于百分比流量拆分的灰度发布
- 故障注入(Fault injection)
- 丰富的度量指标(Rich metrics)

Envoy 被部署为 sidecar，和对应服务在同一个 k8s pod 中。这允许 Istio 将大量关于流量行为的信号作为属性提取出来，而这些属性又可以在 Mixer 中用于执行策略决策，并发送给监控系统，以提供整个网格行为的信息。
Sidecar 代理模型还可以将 Istio 的功能添加到现有部署中，而无需重新构建或重写代码。


<br/>
<br/>


#### Mixer

**Mixer** 是一个独立于平台的组件，负责在服务网格上执行访问控制和使用策略，并从 Envoy 代理和其他服务收集遥测数据。代理提取请求级属性，发送到 Mixer 进行评估。

Mixer 中包括一个灵活的插件模型，使其能够接入到各种主机环境和基础设施后端，从这些细节中抽象出 Envoy 代理和 Istio 管理的服务。


<br/>
<br/>


#### Pilot

**Pilot** 为 Envoy sidecar 提供服务发现功能，为智能路由（如 A/B测试、金丝雀部署）和弹性（超时、重试、熔断器）提供流量管理功能。

它将控制流量行为的高级路由规则转换为特定于 Envoy 的配置，并在运行时将它们传播到 sidecar。Pilot 将平台特定的服务发现机制抽象化并将其合成为符合 Envoy 数据平面 API 的任何 sidecar 都可以使用的标准格式。这种松散耦合使得 Istio 能够在多种环境下运行（如 k8s、Consul、Nomad），同时保持用于流量管理的相同操作界面。


<br/>
<br/>


#### Citadel

**Citadel** 通过内置身份和凭证管理赋能强大的服务间和最终用户身份验证。可用于升级服务网格中未加密的流量，并为运维人员提供基于服务标识而不是网络控制的强制执行策略的能力。


<br/>
<br/>


#### Galley

**Galley** 代表其他的 Istio 控制平面组件，用来验证用户编写的 Istio API 配置。随着时间的推移，Galley 将接管 Istio 获取配置、 处理和分配组件的顶级责任。它将负责将其他的 Istio 组件与从底层平台(如k8s)获取用户配置的细节中隔离开来。



<br/>
<br/>



### 设计目标

Design Goals


Istio的架构设计中有几个关键目标，这些目标对于使系统能够应对大规模流量和高性能地服务处理至关重要。

- 最大化透明度(Maximize Transparency)
- 可扩展性(Extensibility)
- 可移植性(Portability)
- 策略一致性(Policy Uniformity)




<br/>
<br/>
<br/>




## 流量管理

[Traffic Management](https://istio.io/docs/concepts/traffic-management/)






























