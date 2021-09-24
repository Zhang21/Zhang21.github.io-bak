---
title: Istio
date: 2021-02-18 10:21:15
categories: DevOps
tags:
  - K8s
  - ServiceMesh
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
- Istio v1.9
- k8s v1.18










<br/>
<br/>

---

<!--more-->

<br/>
<br/>


















# 概念

[Concepts](https://istio.io/docs/concepts/)


<br>


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
- **金丝雀部署(canary rollouts)**
- **速率限制(rate limiting)**
- **访问控制(access control)**
- **端到端认证(end-to-end authentication)**

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
- 通过强大的基于身份的认证和授权，在集群中实现安全的服务到服务的通信。



<br/>
<br/>



### 核心功能

Core features

Istio 以统一的方式提供了许多跨服务网络的关键功能。


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

- Kubernetes 上的服务部署(Service deployment on Kubernetes)
- 基于 Consul 的服务注册(Services registered with Consul)
- 服务运行在独立的虚拟机上(Services running on individual virtual machines)


<br/>
<br/>


#### 整合和定制

Integration and customization


可以扩展和自定义Istio的策略实施组件，来与现有的ACL，日志记录，监控，配额，审计等方案集成。




<br/>
<br/>




### 架构

Architecture


Istio服务网格逻辑上分为**数据平面(data plane)**和**控制平面(data plane)**。

- 数据平面由一组以 sidecar 方式部署的**智能代理(Envoy)**组成。这些代理可以调节和控制微服务及Mixer之间所有的网络通信。
- 控制平面负责管理和配置代理来路由流量。此外控制平面配置Mixer以实施策略和收集遥测数据。

![](/images/Istio/arch.svg)


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

Istio的流量路由规则可以让你控制服务之间的API调用。Istio 简化了服务级别属性的配置，比如熔断器、超时和重试，并且能轻松的设置重要的任务，如 A/B 测试、金丝雀发布、基于流量百分比切分的概率发布等。

Istio 的流量管理模型源于和服务一起部署的 Envoy 代理。网格内服务发送和接收的所有流量（data plane流量）都经由 Envoy 代理。


<br>
<br>


### 介绍

为了在网格中导流，Istio 需要知道所有的 endpoint 在哪和属于哪个服务。为了定位到service registry(服务注册中心)，Istio 会连接到一个服务发现系统。例如，如果您在 Kubernetes 集群上安装了 Istio，那么它将自动检测该集群中的服务和 endpoint。

使用此服务注册中心，Envoy 代理可以将流量定向到相关服务。大多数基于微服务的应用程序，每个服务的工作负载都有多个实例来处理流量，称为负载均衡池。默认情况下，Envoy 代理基于轮询调度模型在服务的负载均衡池内分发流量，按顺序将请求发送给池中每个成员，一旦所有服务实例均接收过一次请求后，重新回到第一个池成员。

Istio 基本的服务发现和负载均衡能力为您提供了一个可用的服务网格，但它能做到的远比这多的多。在许多情况下，您可能希望对网格的流量情况进行更细粒度的控制。作为 A/B 测试的一部分，您可能想将特定百分比的流量定向到新版本的服务，或者为特定的服务实例子集应用不同的负载均衡策略。您可能还想对进出网格的流量应用特殊的规则，或者将网格的外部依赖项添加到服务注册中心。通过使用 Istio 的流量管理 API 将流量配置添加到 Istio，就可以完成所有这些甚至更多的工作。

和其他 Istio 配置一样，这些 API 也使用 Kubernetes 的自定义资源定义（CRDs）来声明。


<br>
<br>


### 虚拟服务

Virtual Service

虚拟服务和目标规则是Istio流量路由功能的关键拼图。

虚拟服务让您配置如何在服务网格内将请求路由到服务，这基于 Istio 和平台提供的基本的连通性和服务发现能力。每个虚拟服务包含一组路由规则，Istio 按顺序评估它们，Istio 将每个给定的请求匹配到虚拟服务指定的实际目标地址。您的网格可以有多个虚拟服务，也可以没有，取决于您的使用场景。


<br>
<br>


#### 为什么使用虚拟服务

虚拟服务在增强 Istio 流量管理的灵活性和有效性方面，通过对客户端请求的目标地址与真实响应请求的目标工作负载进行解耦来实现。虚拟服务同时提供了丰富的方式，为发送至这些工作负载的流量指定不同的路由规则。

如果没有虚拟服务，Envoy 会在所有的服务实例中使用轮询的负载均衡策略分发请求。使用虚拟服务，您可以为一个或多个主机名指定流量行为。在虚拟服务中使用路由规则，告诉 Envoy 如何发送虚拟服务的流量到适当的目标。路由目标地址可以是同一服务的不同版本，也可以是完全不同的服务。

一个典型的用例是将流量发送到被指定为服务子集的服务的不同版本。客户端将虚拟服务视为一个单一实体，将请求发送至虚拟服务主机，然后 Envoy 根据虚拟服务规则把流量路由到不同的版本。

虚拟服务可以让你：

- 通过单个虚拟服务处理多个应用程序服务。如果您的网格使用 Kubernetes，可以配置一个虚拟服务处理特定命名空间中的所有服务。映射单一的虚拟服务到多个“真实”服务特别有用，可以在不需要客户适应转换的情况下，将单体应用转换为微服务构建的复合应用系统。
- 和网关整合并配置流量规则来控制出入流量


<br>
<br>


#### 虚拟服务示例

下面的虚拟服务根据请求是否来自特定的用户，把它们路由到服务的不同版本。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v3
```

<br>

介绍:

- hosts字段
列举虚拟服务的主机——即用户指定的目标或是路由规则设定的目标。这是客户端向服务发送请求时使用的一个或多个地址。虚拟服务主机名可以是 IP 地址、DNS 名称，或者依赖于平台的一个简称（例如 Kubernetes 服务的短名称），隐式或显式地指向一个完全限定域名（FQDN）。

- 路由规则
在 http 字段包含了虚拟服务的路由规则，用来描述匹配条件和路由行为，它们把 HTTP/1.1、HTTP2 和 gRPC 等流量发送到 hosts 字段指定的目标。
route 部分的 destination 字段指定了符合此条件的流量的实际目标地址。与虚拟服务的 hosts 不同，destination 的 host 必须是存在于 Istio 服务注册中心的实际目标地址，否则 Envoy 不知道该将请求发送到哪里。

- 路由规则优先级
路由规则按从上到下的顺序选择，虚拟服务中定义的第一条规则有最高优先级。
我们建议提供一个默认的无条件或基于权重的规则作为每一个虚拟服务的最后一条规则，如案例所示，从而确保流经虚拟服务的流量至少能够匹配一条路由规则。


<br>
<br>


#### 路由规则的更多内容

路由规则是将特定流量子集路由到指定目标地址的强大工具。您可以在流量端口、header 字段、URI 等内容上设置匹配条件。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
    - bookinfo.com
  http:
  - match:
    - uri:
        prefix: /reviews
    route:
    - destination:
        host: reviews
  - match:
    - uri:
        prefix: /ratings
    route:
    - destination:
        host: ratings
...

  http:
  - match:
      sourceLabels:
        app: reviews
    route:
...
```

有些匹配条件可以使用精确的值，如前缀或正则。

可以使用 AND 向同一个 match 块添加多个匹配条件，或者使用 OR 向同一个规则添加多个 match 块。

另外，使用匹配条件您可以按百分比”权重“分发请求。这在 A/B 测试和金丝雀发布中非常有用：

```yaml
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 75
    - destination:
        host: reviews
        subset: v2
      weight: 25
```

您也可以使用路由规则在流量上执行一些操作，如：

- 添加或删除header
- 重写URL
- 为调用这一目标地址的请求设置重试策略


<br>
<br>


### 目标规则

Destination Rule

您可以将虚拟服务视为将流量如何路由到给定目标地址，然后使用目标规则来配置该目标的流量。在评估虚拟服务路由规则之后，目标规则将应用于流量的“真实”目标地址。

您可以使用目标规则来指定命名的服务子集，例如按版本为所有给定服务的实例分组。然后可以在虚拟服务的路由规则中使用这些服务子集来控制到服务不同实例的流量。

目标规则还允许您在调用整个目的地服务或特定服务子集时定制 Envoy 的流量策略，比如您喜欢的负载均衡模型、TLS 安全模式或熔断器设置。


<br>
<br>


#### 负载均衡选项

Load balancing options

默认情况下，Istio 使用轮询的负载均衡策略，实例池中的每个实例依次获取请求。Istio 同时支持如下的负载均衡模型：

- 轮询(round-robin)
- 随机(Random)：请求以随机的方式转到池中的实例。
- 权重(Weighted)：请求根据指定的百分比转到实例。
- 最少请求(Least requests)：请求被转到最少被访问的实例。


<br>
<br>


#### 目标规则示例

Destination rule example

在下面的示例中，目标规则为 my-svc 目标服务配置了 3 个具有不同负载均衡策略的子集：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: my-destination-rule
spec:
  host: my-svc
  trafficPolicy:
    loadBalancer:
      simple: RANDOM
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
    trafficPolicy:
      loadBalancer:
        simple: ROUND_ROBIN
  - name: v3
    labels:
      version: v3
```


<br>
<br>


### 网关

Gateways

使用网关为网格来管理入站和出站流量，可以让您指定要进入或离开网格的流量。网关配置被用于运行在网格边界的独立 Envoy 代理，而不是服务工作负载的 sidecar 代理。

与 Kubernetes Ingress API 这种控制进入系统流量的其他机制不同，Istio 网关让您充分利用流量路由的强大能力和灵活性。您可以这么做的原因是 Istio 的网关资源可以配置 4-6 层的负载均衡属性，如对外暴露的端口、TLS 设置等。作为替代应用层流量路由（L7）到相同的 API 资源，您绑定了一个常规的 Istio 虚拟服务到网关。这让您可以像管理网格中其他数据平面的流量一样去管理网关流量。

网关主要用于管理进入的流量，但您也可以配置出口网关。出口网关让您为离开网格的流量配置一个专用的出口节点，这可以限制哪些服务可以或应该访问外部网络，或者启用出口流量安全控制为您的网格添加安全性。您也可以使用网关配置一个纯粹的内部代理。

Istio 提供了一些预先配置好的网关代理部署（`istio-ingressgateway` 和 `istio-egressgateway`）供您使用。


<br>
<br>


#### gateway示例

下面的示例展示了一个外部 HTTPS 入口流量的网关配置：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: ext-host-gwy
spec:
  selector:
    app: my-gateway-controller
  servers:
  - port:
      number: 443
      name: https
      protocol: HTTPS
    hosts:
    - ext-host.example.com
    tls:
      mode: SIMPLE
      serverCertificate: /tmp/tls.crt
      privateKey: /tmp/tls.key
```

这个网关配置让 HTTPS 流量从 ext-host.example.com 通过 443 端口流入网格，但没有为请求指定任何路由规则。为想要工作的网关指定路由，您必须把网关绑定到虚拟服务上。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: virtual-svc
spec:
  hosts:
  - ext-host.example.com
  gateways:
    - ext-host-gwy
```


<br>
<br>


### 服务入口

Service entries

使用服务入口来添加一个入口到 Istio 内部维护的服务注册中心。添加了服务入口后，Envoy 代理可以向服务发送流量，就好像它是网格内部的服务一样。配置服务入口允许您管理运行在网格外的服务的流量，它包括以下几种能力：

- 为外部目标 redirect 和转发请求。
- 为外部目标定义重试、超时和故障注入策略。
- 添加一个运行在虚拟机的服务来扩展您的网格。
- 从逻辑上添加来自不同集群的服务到网格，在 Kubernetes 上实现一个多集群 Istio 网格。

您不需要为网格服务要使用的每个外部服务都添加服务入口。默认情况下，Istio 配置 Envoy 代理将请求传递给未知服务。但是，您不能使用 Istio 的特性来控制没有在网格中注册的目标流量。


<br>
<br>


#### 服务入口示例

下面示例的 mesh-external 服务入口将 ext-resource 外部依赖项添加到 Istio 的服务注册中心：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: svc-entry
spec:
  hosts:
  - ext-svc.example.com
  ports:
  - number: 443
    name: https
    protocol: HTTPS
  location: MESH_EXTERNAL
  resolution: DNS
```

您指定的外部资源使用 hosts 字段。可以使用完全限定名或通配符作为前缀域名。

您可以配置虚拟服务和目标规则，以更细粒度的方式控制到服务入口的流量，这与网格中的任何其他服务配置流量的方式相同。

例如，下面的目标规则配置流量路由以使用双向 TLS 来保护到 ext-svc.example.com 外部服务的连接，我们使用服务入口配置了该外部服务：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: ext-res-dr
spec:
  host: ext-svc.example.com
  trafficPolicy:
    tls:
      mode: MUTUAL
      clientCertificate: /etc/certs/myclientcert.pem
      privateKey: /etc/certs/client_private_key.pem
      caCertificates: /etc/certs/rootcacerts.pem
```


<br>
<br>


### Sidecar

默认情况下，Istio 让每个 Envoy 代理都可以访问来自和它关联的工作负载的所有端口的请求，然后转发到对应的工作负载。您可以使用 sidecar 配置去做下面的事情：

- 微调 Envoy 代理接受的端口和协议集。
- 限制 Envoy 代理可以访问的服务集合。

您可以指定将 sidecar 配置应用于特定命名空间中的所有工作负载，或者使用 workloadSelector 选择特定的工作负载。

例如，下面的 sidecar 配置将 bookinfo 命名空间中的所有服务配置为仅能访问运行在相同命名空间和 Istio 控制平面中的服务：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Sidecar
metadata:
  name: default
  namespace: bookinfo
spec:
  egress:
  - hosts:
    - "./*"
    - "istio-system/*"
```


<br>
<br>


### 网络弹性和测试

Network resilience and testing

除了为您的网格导流之外，Istio 还提供了可选的故障恢复和故障注入功能，您可以在运行时动态配置这些功能。使用这些特性可以让您的应用程序运行稳定，确保服务网格能够容忍故障节点，并防止局部故障级联影响到其他节点。


<br>
<br>


#### 超时

Timeouts

超时是 Envoy 代理等待来自给定服务的答复的时间量，以确保服务不会因为等待答复而无限期的挂起，并在可预测的时间范围内调用成功或失败。HTTP 请求的默认超时时间是 15 秒，这意味着如果服务在 15 秒内没有响应，调用将失败。

对于某些应用程序和服务，Istio 的缺省超时可能不合适。为了找到并使用最佳超时设置，Istio 允许您使用虚拟服务按服务轻松地动态调整超时，而不必修改您的业务代码。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - route:
    - destination:
        host: ratings
        subset: v1
    timeout: 10s
```


<br>
<br>


#### 重试

Retries

重试设置指定如果初始调用失败，Envoy 代理尝试连接服务的最大次数。HTTP 请求的默认重试行为是在返回错误之前重试两次。您可以在虚拟服务中按服务调整重试设置，而不必修改业务代码。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - route:
    - destination:
        host: ratings
        subset: v1
    retries:
      attempts: 3
      perTryTimeout: 2s
```


<br>
<br>


#### 熔断器

Circuit breakers

熔断器是 Istio 为创建具有弹性的微服务应用提供的另一个有用的机制。在熔断器中，设置一个对服务中的单个主机调用的限制，例如并发连接的数量或对该主机调用失败的次数。一旦限制被触发，熔断器就会“跳闸”并停止连接到该主机。使用熔断模式可以快速失败而不必让客户端尝试连接到过载或有故障的主机。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: reviews
spec:
  host: reviews
  subsets:
  - name: v1
    labels:
      version: v1
    trafficPolicy:
      connectionPool:
        tcp:
          maxConnections: 100
```


<br>
<br>


#### 故障注入

Fault injection

在配置了网络，包括故障恢复策略之后，可以使用 Istio 的故障注入机制来为整个应用程序测试故障恢复能力。故障注入是一种将错误引入系统以确保系统能够承受并从错误条件中恢复的测试方法。使用故障注入特别有用，能确保故障恢复策略不至于不兼容或者太严格，这会导致关键服务不可用。

与其他错误注入机制不同，Istio 允许在应用层注入错误。

您可以注入两种故障，它们都使用虚拟服务配置：

- 延迟(Delays)：延迟是时间故障。它们模拟增加的网络延迟或一个超载的上游服务。
- 终止(Aborts)：终止是崩溃失败。他们模仿上游服务的失败。终止通常以 HTTP 错误码或 TCP 连接失败的形式出现。

例如，下面的虚拟服务为千分之一的访问 ratings 服务的请求配置了一个 5 秒的延迟：

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - fault:
      delay:
        percentage:
          value: 0.1
        fixedDelay: 5s
    route:
    - destination:
        host: ratings
        subset: v1
```


<br>
<br>


#### 和您的应用程序一起运行

Working with your applications

Istio 故障恢复功能对应用程序来说是完全透明的。在返回响应之前，应用程序不知道 Envoy sidecar 代理是否正在处理被调用服务的故障。这意味着，如果在应用程序代码中设置了故障恢复策略，那么您需要记住这两个策略都是独立工作的，否则会发生冲突。

虽然 Istio 故障恢复特性提高了网格中服务的可靠性和可用性，但应用程序必须处理故障或错误并采取适当的回退操作。例如，当负载均衡中的所有实例都失败时，Envoy 返回一个HTTP 503代码。应用程序必须实现回退逻辑来处理HTTP 503错误代码。



<br>
<br>



## 安全

Security

将单一应用程序分解为微服务可提供各种好处，包括更好的灵活性、可伸缩性以及服务复用的能力。但是，微服务也有特殊的安全需求：

- 为了抵御中间人攻击，需要流量加密。
- 为了提供灵活的服务访问控制，需要双向 TLS 和细粒度的访问策略。
- 要确定谁在什么时候做了什么，需要审计工具。

Istio Security 尝试提供全面的安全解决方案来解决所有这些问题。

![](/images/Istio/overview.svg)

<br>

Istio 安全功能提供强大的身份，强大的策略，透明的 TLS 加密，认证，授权和审计（AAA）工具来保护你的服务和数据。Istio 安全的目标是：

- 默认安全：应用程序代码和基础设施无需更改
- 深度防御：与现有安全系统集成以提供多层防御
- 零信任网络：在不受信任的网络上构建安全解决方案


<br>
<br>


### 高级架构

High-level architecture

Istio 中的安全性涉及多个组件：

- 用于密钥和证书管理的证书颁发机构（CA）
- 配置 API 服务器分发给代理：
  - 认证策略
  - 授权策略
  - 安全命名信息
- Sidecar 和边缘代理作为 Policy Enforcement Points(PEPs) 以保护客户端和服务器之间的通信安全
- 一组 Envoy 代理扩展，用于管理遥测和审计

控制面处理来自 API server 的配置，并且在数据面中配置 PEPs。PEPs 用 Envoy 实现。下图显示了架构。

![](/images/Istio/arch-sec.svg)


<br>
<br>


### Istio身份

Istio identity

身份是任何安全基础架构的基本概念。在工作负载间通信开始时，双方必须交换包含身份信息的凭证以进行双向验证。

在客户端，根据安全命名信息检查服务器的标识，以查看它是否是该服务的授权运行程序。在服务器端，服务器可以根据授权策略确定客户端可以访问哪些信息，审计谁在什么时间访问了什么，根据他们使用的工作负载向客户收费，并拒绝任何未能支付账单的客户访问工作负载。

Istio 身份模型使用 service identity （服务身份）来确定一个请求源端的身份。这种模型有极好的灵活性和粒度，可以用服务身份来标识人类用户、单个工作负载或一组工作负载。在没有服务身份的平台上，Istio 可以使用其它可以对服务实例进行分组的身份，例如服务名称。

下面的列表展示了在不同平台上可以使用的服务身份：

- Kubernetes: Kubernetes service account
- GKE/GCE: GCP service account
- GCP: GCP service account
- AWS: AWS IAM user/role account
- 本地（非 Kubernetes）：用户帐户、自定义服务帐户、服务名称、Istio 服务帐户或 GCP 服务帐户。自定义服务帐户引用现有服务帐户，就像客户的身份目录管理的身份一样。


<br>
<br>


### 公钥基础设施

Identity and certificate management

Istio PKI 使用 X.509 证书为每个工作负载都提供强大的身份标识。可以大规模进行自动化密钥和证书轮换，伴随每个 Envoy 代理都运行着一个 istio-agent 负责证书和密钥的供应。

![](/images/Istio/id-prov.svg)

Istio 供应身份是通过 secret discovery service（SDS）来实现的，具体流程如下：

1. CA 提供 gRPC 服务以接受证书签名请求（CSRs）。
2. Envoy 通过 Envoy 秘密发现服务（SDS）API 发送证书和密钥请求。
3. 在收到 SDS 请求后，istio-agent 创建私钥和 CSR，然后将 CSR 及其凭据发送到 Istio CA 进行签名。
4. CA 验证 CSR 中携带的凭据并签署 CSR 以生成证书。
5. Istio-agent 通过 Envoy SDS API 将私钥和从 Istio CA 收到的证书发送给 Envoy。
6. 上述 CSR 过程会周期性地重复，以处理证书和密钥轮换。


<br>
<br>


### 认证

Authentication

Istio 提供两种类型的认证：

- Peer authentication：用于服务到服务的认证，以验证进行连接的客户端。
  - 为每个服务提供强大的身份，表示其角色，以实现跨群集和云的互操作性。
  - 保护服务到服务的通信。
  - 提供密钥管理系统，以自动进行密钥和证书的生成，分发和轮换。
- Request authentication：用于最终用户认证，以验证附加到请求的凭据。
  - ORY Hydra
  - Keycloak
  - Auth0
  - Firebase Auth
  - Google Auth

在所有情况下，Istio 都通过自定义 Kubernetes API 将认证策略存储在 Istio config store。


<br>
<br>


#### 双向TLS认证

Mutual TLS authentication

Istio 通过客户端和服务器端 PEPs 建立服务到服务的通信通道，PEPs 被实现为Envoy 代理。

当一个工作负载使用双向 TLS 认证向另一个工作负载发送请求时，该请求的处理方式如下：

1. Istio 将出站流量从客户端重新路由到客户端的本地 sidecar Envoy。
2. 客户端 Envoy 与服务器端 Envoy 开始双向 TLS 握手。在握手期间，客户端 Envoy 还做了安全命名检查，以验证服务器证书中显示的服务帐户是否被授权运行目标服务。
3. 客户端 Envoy 和服务器端 Envoy 建立了一个双向的 TLS 连接，Istio 将流量从客户端 Envoy 转发到服务器端 Envoy。
4. 授权后，服务器端 Envoy 通过本地 TCP 连接将流量转发到服务器服务。

<br>

**宽容模式**
Permissive mode

Istio 双向 TLS 具有一个宽容模式，允许服务同时接受纯文本流量和双向 TLS 流量。这个功能极大的提升了双向 TLS 的入门体验。

<br>

**安全命名**
Secure naming

服务器身份被编码在证书里，但服务名称通过服务发现或 DNS 被检索。安全命名信息将服务器身份映射到服务名称。身份 A 到服务名称 B 的映射表示“授权 A 运行服务 B“。控制平面监视 apiserver，生成安全命名映射，并将其安全地分发到 PEPs。

假设运行服务 datastore 的合法服务器仅使用 infra-team 身份。恶意用户拥有 test-team 身份的证书和密钥。恶意用户打算模拟服务以检查从客户端发送的数据。恶意用户使用证书和 test-team 身份的密钥部署伪造服务器。假设恶意用户成功攻击了发现服务或 DNS，以将 datastore 服务名称映射到伪造服务器。

当客户端调用 datastore 服务时，它从服务器的证书中提取 test-team 身份，并用安全命名信息检查 test-team 是否被允许运行 datastore。客户端检测到 test-team 不允许运行 datastore 服务，认证失败。

安全命名能够防止 HTTPS 流量受到一般性网络劫持，除了 DNS 欺骗外，它还可以保护 TCP 流量免受一般网络劫持。如果攻击者劫持了 DNS 并修改了目的地的 IP 地址，它将无法用于 TCP 通信。这是因为 TCP 流量不包含主机名信息，我们只能依靠 IP 地址进行路由，而且甚至在客户端 Envoy 收到流量之前，也可能发生 DNS 劫持。


<br>
<br>


#### 认证架构

Authentication architecture

您可以使用 peer 和 request 认证策略为在 Istio 网格中接收请求的工作负载指定认证要求。一有任何的策略变更，新策略都会转换为适当的配置，告知 PEP 如何执行所需的认证机制。

Istio 异步发送配置到目标端点。代理收到配置后，新的认证要求会立即生效。

![](/images/Istio/authn.svg)

Istio 将两种类型的身份验证以及凭证中的其他声明（如果适用）输出到授权。


<br>
<br>


#### 认证策略

Authentication policies

认证策略是对服务收到的请求生效的。

```yaml
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "example-peer-policy"
  namespace: "foo"
spec:
  selector:
    matchLabels:
      app: reviews
  mtls:
    mode: STRICT
```

<br>

**策略存储**
Policy storage

Istio 将网格范围的策略存储在根命名空间。这些策略使用一个空的 selector 适用于网格中的所有工作负载。具有名称空间范围的策略存储在相应的名称空间中。它们仅适用于其命名空间内的工作负载。如果你配置了 selector 字段，则认证策略仅适用于与您配置的条件匹配的工作负载。

kind字段的两个值，`PeerAuthentication`和`RequestAuthentication`。

<br>

**Selector**

使用 selector 字段来指定该策略适用的工作负载的标签

如果您没有为 selector 字段提供值，则 Istio 会将策略与策略存储范围内的所有工作负载进行匹配。

因此，selector 字段可帮助您指定策略的范围：

- 网格范围策略：为根名称空间指定的策略，不带或带有空的 selector 字段。
- 命名空间范围的策略：为非root命名空间指定的策略，不带有或带有空的 selector 字段。
- 特定于工作负载的策略：在常规名称空间中定义的策略，带有非空 selector 字段。

只能有一个网格范围的 Peer 认证策略，每个命名空间也只能有一个命名空间范围的 Peer 认证策略。当您为同一网格或命名空间配置多个网格范围或命名空间范围的 Peer 认证策略时，Istio 会忽略较新的策略。当多个特定于工作负载的 Peer 认证策略匹配时，Istio 将选择最旧的策略。

Istio 按照以下顺序为每个工作负载应用最窄的匹配策略：

1. 特定于工作负载的
2. 命名空间范围
3. 网格范围

Istio 可以将所有匹配的 request 认证策略组合起来，就像它们来自单个 request 认证策略一样。因此，您可以在网格或名称空间中配置多个网格范围或命名空间范围的策略。但是，避免使用多个网格范围或命名空间范围的 request 认证策略仍然是一个好的实践。

<br>

**Peer**

Peer 认证策略指定 Istio 对目标工作负载实施的双向 TLS 模式。支持以下模式：

- PERMISSIVE：工作负载接受双向 TLS 和纯文本流量。此模式在迁移因为没有 sidecar 而无法使用双向 TLS 的工作负载的过程中非常有用。一旦工作负载完成 sidecar 注入的迁移，应将模式切换为 STRICT。
- STRICT： 工作负载仅接收双向 TLS 流量。
- DISABLE：禁用双向 TLS。 从安全角度来看，除非您提供自己的安全解决方案，否则请勿使用此模式。

如果未设置模式，将继承父作用域的模式。未设置模式的网格范围的 peer 认证策略默认使用 PERMISSIVE 模式。

下面的 peer 认证策略要求命名空间 foo 中的所有工作负载都使用双向 TLS：

```yaml
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "example-policy"
  namespace: "foo"
spec:
  mtls:
    mode: STRICT
```

禁用特定工作负载：

```yaml
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "example-workload-policy"
  namespace: "foo"
spec:
  selector:
     matchLabels:
       app: example-app
  portLevelMtls:
    80:
      mode: DISABLE
```

<br>

**Requests**

Request 认证策略指定验证 JSON Web Token（JWT）所需的值。 这些值包括：

- token 在请求中的位置
- 请求的 issuer
- 公共 JSON Web Key Set（JWKS）

Istio 会根据 request 认证策略中的规则检查提供的令牌（如果已提供），并拒绝令牌无效的请求。当请求不带有令牌时，默认情况下将接受它们。要拒绝没有令牌的请求，请提供授权规则，该规则指定对特定操作（例如，路径或操作）的限制。

<br>

**Principals**

使用 peer 认证策略和双向 TLS 时，Istio 将身份从 peer 认证提取到 `source.principal` 中。同样，当您使用 request 认证策略时，Istio 会将 JWT 中的身份赋值给 `request.auth.principal`。


<br>
<br>


#### 更新认证策略

Updating authentication policies

您可以随时更改认证策略，Istio 几乎实时将新策略推送到工作负载。但是，Istio 无法保证所有工作负载都同时收到新政策。

以下建议有助于避免在更新认证策略时造成干扰：

- 将 peer 认证策略的模式从 DISABLE 更改为 STRICT 时，请使用 PERMISSIVE 模式来过渡，反之亦然。当所有工作负载成功切换到所需模式时，您可以将策略应用于最终模式。您可以使用 Istio 遥测技术来验证工作负载已成功切换。
- 将 request 认证策略从一个 JWT 迁移到另一个 JWT 时，将新 JWT 的规则添加到该策略中，而不删除旧规则。这样，工作负载接受两种类型的 JWT，当所有流量都切换到新的 JWT 时，您可以删除旧规则。但是，每个 JWT 必须使用不同的位置。


<br>
<br>


### 授权

Authorization

Istio 的授权功能为网格中的工作负载提供网格、命名空间和工作负载级别的访问控制。

这种控制层级提供了以下优点：

- 工作负载间和最终用户到工作负载的授权。
- 一个简单的 API：它包括一个单独的并且很容易使用和维护的 AuthorizationPolicy CRD
- 灵活的语义：运维人员可以在 Istio 属性上定义自定义条件，并使用 DENY 和 ALLOW 动作。
- 高性能：Istio 授权是在 Envoy 本地强制执行的。
- 高兼容性：原生支持 HTTP、HTTPS 和 HTTP2，以及任意普通 TCP 协议。


<br>


#### 授权架构

Authorization architecture

每个 Envoy 代理都运行一个授权引擎，该引擎在运行时授权请求。当请求到达代理时，授权引擎根据当前授权策略评估请求上下文，并返回授权结果 ALLOW 或 DENY。

![](/images/Istio/authz.svg)


<br>
<br>


#### 隐式启用

Implicit enablement

您无需显式启用 Istio 的授权功能。只需将授权策略应用于工作负载即可实施访问控制。对于未应用授权策略的工作负载，Istio 不会执行访问控制，放行所有请求。

授权策略支持 ALLOW 和 DENY 动作。 拒绝策略优先于允许策略。如果将任何允许策略应用于工作负载，则默认情况下将拒绝对该工作负载的访问，除非策略中的规则明确允许。当您将多个授权策略应用于相同的工作负载时，Istio 会累加地应用它们。


<br>
<br>


#### 授权策略

Authorization policies

要配置授权策略，请创建一个 AuthorizationPolicy 自定义资源。

一个授权策略包括选择器（selector），动作（action） 和一个规则（rules）列表：

- selector 字段指定策略的目标
- action 字段指定允许还是拒绝请求
- rules 指定何时触发动作
  - from 字段指定请求的来源
  - to 字段指定请求的操作
  - when 字段指定应用规则所需的条件

栗子：

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin
 namespace: foo
spec:
 selector:
   matchLabels:
     app: httpbin
     version: v1
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/sleep"]
   - source:
       namespaces: ["dev"]
   to:
   - operation:
       methods: ["GET"]
   when:
   - key: request.auth.claims[iss]
     values: ["https://accounts.google.com"]
```

下例显示了一个授权策略，如果请求来源不是命名空间 foo，请求将被拒绝。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin-deny
 namespace: foo
spec:
 selector:
   matchLabels:
     app: httpbin
     version: v1
 action: DENY
 rules:
 - from:
   - source:
       notNamespaces: ["foo"]
```

拒绝策略优先于允许策略。如果请求同时匹配上允许策略和拒绝策略，请求将被拒绝。Istio 首先评估拒绝策略，以确保允许策略不能绕过拒绝策略。

<br>

**策略目标**
Policy Target

您可以通过 `metadata/namespace` 字段和可选的 `selector` 字段来指定策略的范围或目标。根命名空间的值是可配置的，默认值为 `istio-system`。

栗子:

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-read
  namespace: default
spec:
  selector:
    matchLabels:
      app: products
  action: ALLOW
  rules:
  - to:
    - operation:
         methods: ["GET", "HEAD"]
```

<br>

**值匹配**
Value matching

授权策略中的大多数字段都支持以下所有匹配模式：

- 完全匹配
- 前缀匹配
- 后缀匹配
- 存在匹配

有一些例外，例如，以下字段仅支持完全匹配：

- `when` 部分下的 `key` 字段
- `source` 部分下 的 `ipBlocks`
- `to` 部分下的 `ports` 字段

以下示例策略允许访问前缀为 `/test/*` 或后缀为 `*/info` 的路径。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: tester
  namespace: default
spec:
  selector:
    matchLabels:
      app: products
  action: ALLOW
  rules:
  - to:
    - operation:
        paths: ["/test/*", "*/info"]
```

<br>

**排除匹配**
Exclusion matching

Istio 支持排除匹配。

以下示例：如果请求路径不是 `/healthz`，则要求从请求的 JWT 认证中导出的主体是有效的。 因此，该策略从 JWT 身份验证中排除对 `/healthz` 路径的请求：

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: disable-jwt-for-healthz
  namespace: default
spec:
  selector:
    matchLabels:
      app: products
  action: ALLOW
  rules:
  - to:
    - operation:
        notPaths: ["/healthz"]
    from:
    - source:
        requestPrincipals: ["*"]
```

下面的示例拒绝到 `/admin` 路径且不带请求主体的请求：

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: enable-jwt-for-admin
  namespace: default
spec:
  selector:
    matchLabels:
      app: products
  action: DENY
  rules:
  - to:
    - operation:
        paths: ["/admin"]
    from:
    - source:
        notRequestPrincipals: ["*"]
```

<br>

**全部允许和默认全部拒绝授权策略**
Allow-all and default deny-all authorization policies

以下示例显示了一个简单的 `allow-all` 授权策略，该策略允许完全访问 default 命名空间中的所有工作负载。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-all
  namespace: default
spec:
  action: ALLOW
  rules:
  - {}
```

以下示例显示了一个策略，该策略不允许任何对 admin 命名空间工作负载的访问。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: deny-all
  namespace: admin
spec:
  {}
```

<br>

**自定义条件**
Custom conditions

您还可以使用 when 部分指定其他条件。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin
 namespace: foo
spec:
 selector:
   matchLabels:
     app: httpbin
     version: v1
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/sleep"]
   to:
   - operation:
       methods: ["GET"]
   when:
   - key: request.headers[version]
     values: ["v1", "v2"]
```

<br>

**认证与未认证身份**
Authenticated and unauthenticated identity

如果要使工作负载可公开访问，则需要将 source 部分留空。这允许来自所有（经过认证和未经认证）的用户和工作负载的源。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin
 namespace: foo
spec:
 selector:
   matchLabels:
     app: httpbin
     version: v1
 action: ALLOW
 rules:
 - to:
   - operation:
       methods: ["GET", "POST"]
```

要仅允许经过认证的用户，请将 principal 设置为 `*`。

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: httpbin
 namespace: foo
spec:
 selector:
   matchLabels:
     app: httpbin
     version: v1
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["*"]
   to:
   - operation:
       methods: ["GET", "POST"]
```


<br>
<br>


#### 在普通TCP协议上使用Istio授权

Using Istio authorization on plain TCP protocols

Istio 授权支持工作负载使用任意普通 TCP 协议，如 MongoDB。 在这种情况下，您可以按照与 HTTP 工作负载相同的方式配置授权策略。不同之处在于某些字段和条件仅适用于 HTTP 工作负载。 这些字段包括：

- 授权策略对象 source 部分中的 request_principals 字段
- 授权策略对象 operation 部分中的 hosts、methods 和 paths 字段

如果您在授权策略中对 TCP 工作负载使用了任何只适用于 HTTP 的字段，Istio 将会忽略它们。

假设您在端口 27017 上有一个 MongoDB 服务，下例配置了一个授权策略，只允许 Istio 网格中的 bookinfo-ratings-v2 服务访问该 MongoDB 工作负载。

```yaml
apiVersion: "security.istio.io/v1beta1"
kind: AuthorizationPolicy
metadata:
  name: mongodb-policy
  namespace: default
spec:
 selector:
   matchLabels:
     app: mongodb
 action: ALLOW
 rules:
 - from:
   - source:
       principals: ["cluster.local/ns/default/sa/bookinfo-ratings-v2"]
   to:
   - operation:
       ports: ["27017"]
```


<br>
<br>


#### 对双向 TLS 的依赖

Dependency on mutual TLS

Istio 使用双向 TLS 将某些信息从客户端安全地传递到服务器。在使用授权策略中的以下任何字段之前，必须先启用双向 TLS：

```
source 部分下的 principals 字段
source 部分下的 namespaces 字段
source.principal 自定义条件
source.namespace 自定义条件
connection.sni 自定义条件
```



<br>
<br>



## 可观察性

Observability

Istio 为网格内所有的服务通信生成详细的遥测数据。这种遥测技术提供了服务行为的可观察性。通过 Istio，运维人员可以全面了解到受监控的服务如何与其他服务以及 Istio 组件进行交互。

Istio 生成以下类型的遥测数据，以提供对整个服务网格的可观察性：

- 指标(Metrics)。Istio 基于 4 个监控的黄金标识（延迟、流量、错误、饱和）生成了一系列服务指标。
- 分布式追踪(Distributed Traces)。Istio 为每个服务生成分布式追踪 span，运维人员可以理解网格内服务的依赖和调用流程。
- 访问日志(Access Logs)。当流量流入网格中的服务时，Istio 可以生成每个请求的完整记录，包括源和目标的元数据。


<br>
<br>


### 指标

Metrics

Istio 为服务网格中所有出入的服务流量都生成了指标。除了监控网格中服务的行为外，监控网格本身的行为也很重要。Istio 组件可以导出自身内部行为的指标，以提供对网格控制平面的功能和健康情况的洞察能力。


<br>


#### 代理级别指标

Proxy-level metrics

Istio 指标收集从 sidecar 代理（Envoy）开始。每个代理为通过它的所有流量（入站和出站）生成一组丰富的指标。代理还提供关于它本身管理功能的详细统计信息，包括配置信息和健康信息。


<br>
<br>


#### 服务级别指标

Service-level metrics

Istio 还提供了一组用于监控服务通信的面向服务的指标。这些指标涵盖了四个基本的服务监控需求：延迟、流量、错误和饱和情况。Istio 带有一组默认的仪表板，用于监控基于这些指标的服务行为。

默认的 Istio 指标由 Istio 提供的配置集定义并默认导出到 Prometheus。


<br>
<br>


#### 控制平面指标

Control plane metrics

每一个 Istio 的组件（Pilot、Galley、Mixer）都提供了对自身监控指标的集合。更多详情请参考各个组件的文档。


<br>
<br>


### 分布式追踪

Distributed traces

分布式追踪通过监控流经网格的单个请求，提供了一种监控和理解行为的方法。追踪使网格的运维人员能够理解服务的依赖关系以及在服务网格中的延迟源。

Istio 支持通过 Envoy 代理进行分布式追踪。代理自动为其应用程序生成追踪 span，只需要应用程序转发适当的请求上下文即可。

Istio 支持很多追踪系统，包括 Zipkin、Jaeger、LightStep、Datadog。

Istio 为一个请求生成的分布式追踪数据：

![](/images/Istio/istio-tracing-details-zipkin.png)


<br>
<br>


### 访问日志

Access logs

访问日志提供了一种从单个工作负载实例的角度监控和理解行为的方法。

Istio 可以以一组可配置的格式集生成服务流量的访问日志。访问日志可以在本地生成，或者导出到自定义的后端基础设施。



<br>
<br>



## 扩展性

Extensibility

WebAssembly 是一种沙盒技术，可以用于扩展 Istio 代理（Envoy）的能力。Proxy-Wasm 沙盒 API 取代了 Mixer 作为 Istio 主要的扩展机制。

WebAssembly 沙盒的目标：

- 效率(Efficiency) - 这是一种低延迟，低 CPU 和内存开销的扩展机制。
- 功能(Function) - 这是一种可以执行策略，收集遥测数据和执行有效荷载变更的扩展机制。
- 隔离(Isolation) - 一个插件中程序的错误或是崩溃不会影响其它插件。
- 配置(Configuration) - 插件使用与其它 Istio API 一致的 API 进行配置。可以动态的配置扩展。
- 运维(Operator) - 扩展可以以仅日志，故障打开或者故障关闭的方式进行访问和部署。
- 扩展开发者(Extension developer) - 可以用多种编程语言编写。


<br>
<br>


### 高级架构

Istio 扩展（Proxy-Wasm 插件）有几个组成部分：

- 过滤器服务提供商接口(Filter Service Provider Interface (SPI))， 用于为过滤器构建 Proxy-Wasm 插件。
- 沙盒 在 Envoy 中嵌入 V8 Wasm 运行时。
- 主机 API 用于处理请求头，尾和元数据。
- 调出 API 针对 gRPC 和 HTTP 请求。
- 统计和记录 API 用于度量统计和监控。

![](/images/Istio/extending.svg)



<br>
<br>



## 术语表

Glossary

<br>

**ADAPTERS**
适配器（adapter）是 Istio 策略和遥测组件 Mixer 的插件, 可使其与一组开放式基础架构后端交互，这些后端可提供核心功能，例如日志记录、监控、配额、ACL 检查等等。运行时所使用的精确的适配器集合是通过配置确定的，并可以针对新的或定制的基础架构后端轻松扩展。

**ANNOTATION**
注释是指附加到 Kubernetes annotation 的资源。

**ATTRIBUTE**
属性控制着网格中服务运行时的行为，是一堆有名字的、有类型的元数据，它们描述了 ingress 和 egress 流量，以及这些流量所在的环境。

**CLUSTER**
集群是运行容器化应用程序的一组计算节点。

**CONTROL PLANE**
控制平面是一组系统服务，这些服务配置网格或者网格的子网来管理工作负载实例之间的通信。

**CRDS**
自定义资源定义 (CRD) 是默认的 Kubernetes API 扩展。

**DATA PLANE**
数据平面是网格的一部分，直接控制工作负载实例之间的通信。Istio 的数据平面使用智能 Envoy 代理部署成 sidecar 去调节和控制服务网格中发送和接受的流量。

**DESTINATION**
目标服务 (destination) 是 envoy 代表一个源服务 工作负载与之打交道的远程上游服务。这些上游服务可以有多个服务版本，envoy 根据路由选择对应的版本。

**ENVOY**
Envoy 是在 Istio 里使用的高性能代理，用于为所有服务网格里的服务调度进出的流量。

**FAILURE DOMAIN**
故障域是计算环境中物理或者逻辑的一部分，当关键设备或服务遇到问题时，它也会受到负面影响。对于 Istio 部署而言，故障域可能包含平台中的多个可用性区域。

**IDENTITY**
身份是基本的安全基础结构概念。Istio 的身份模型是基于第一阶级的工作负载身份。在服务之间的通信开始时，双方使用身份信息交换证书来实现相互认证的目的。
客户端根据其安全的命名信息检查服务器的身份，以便确定服务器是否被授权运行服务。
服务器检查客户端的身份，以确定客户端可以访问的信息。服务器基于客户端的身份，来确定配置的策略。
通过使用身份，服务器可以审核访问信息的时间和特定客户端访问的信息内容。还可以根据客户使用的服务向他们收费，并拒绝任何未付款的客户访问服务。

**ISTIOD**
istiod组件是控制平面二进制，它封装了Pilot, Citadel, Mixer, Galley。

**MANAGED CONTROL PLANE**
托管控制平面是一个为客户提供管理的控制平面。 托管控制平面降低了用户部署的复杂性，并通常保证一定水平的性能和可用性。

**MESH FEDERATION**
网格联邦是在网格之间公开服务的一种行为，并且能跨越网格边界进行通信。每一个网格或许会公开其一部分的服务，使一个或多个其他网格使用此公开的服务。 您可以使用网格联邦来启用网格之间的通信。

**MICRO-SEGMENTATION**
一种安全技术，可在云部署中创建安全区域，使组织能够将工作负载彼此隔离，并分别保证它们的安全。

**MIXER**
Mixer 是 Istio 里的一个组件，它负责增强服务网格里的访问控制和使用策略。它还负责收集来自 envoy 和其他服务的遥测数据。

**MIXER HANDLER**
Handler 相当于配置完备的 Mixer 适配器。在 Mixer 运行时，Mixer 将 instances 路由到一个或多个 handlers。

**MIXER INSTANCE**
Mixer Instance 表示通过检查一组请求属性 ，并结合使用者提供的配置而生成的数据块。Mixer Instance 在随请求到达各种基础后端设施的途中，会被传递给各个处理程序。

**MULTI-MESH**
多个服务网格组成的部署模型。每个网格都有独立的命名管理和身份管理，但是您可以通过网格联邦来暴露 网格之间的服务, 最终构成一个多网格部署。

**MULTI-CLUSTER**
多个集群的网格组成

**MUTUAL TLS AUTHENTICATION**
双向 TLS 通过内置身份和凭证管理，提供强大的服务到服务身份验证。

**OPERATOR**
打包，部署和管理k8s应用程序的一种方法。

**PILOT**
Pilot 是 Istio 里的一个组件，它控制 Envoy 代理，负责服务发现、负载均衡和路由分发。

**PRIMARY CLUSTER**
主集群是具有控制平面 的集群。 一个网格可以有一个以上的主集群，以用于 HA 或需要低延迟的场景。 主集群可以充当从集群的控制平面。

**ROUTING RULES**
您在虚拟服务中配置的路由规则，遵循服务网格定义了请求的路径。使用路由规则，您可以定义将寻址到虚拟服务主机的流量路由到指定目标的工作负载。 路由规则使您可以控制流量，以实现如 A/B 测试、金丝雀发布以及按百分比分配流量的分阶段发布等任务。

**SECURE NAMING**
提供一个 service name 到 workload instance principals 的映射，这个工作负载实例被授权运行一个 workload instances，实现一个 service。

**SERVICE**
使用服务名称标识一组具有关联行为的服务服务网格， 并使用这些名称应用 Istio 策略。

**SERVICE CONSUMER**
服务消费者是使用 service 的代理。

**SERVICE ENDPOINT**
一个 service 的网络可达表现形式。

**SERVICE MESH**
服务网格 （简称 网格 ）是一个可管理、可观测以及支持工作负载实例之间进行安全通信的基础设施层。

**SERVICE OPERATOR**
是在 service mesh 里管理 service 的代理，它们通过操纵配置状态并通过各种仪表板监视服务的运行状况来管理这些服务。

**SERVICE PRODUCER**
创建服务的 pilot-agent。

**SERVICE REGISTRY**
Istio 维护了一个内部服务注册表 (service registry)，它包含在服务网格中运行的一组服务及其相应的服务 endpoints。Istio 使用服务注册表生成 Envoy 配置。
Istio 不提供服务发现，尽管大多数服务都是通过 Pilot adapter 自动加入到服务注册表里的，而且这反映了底层平台（Kubernetes、Consul、plain DNS）的已发现的服务。还有就是，可以使用 ServiceEntry 配置手动进行注册。

**SERVICE VERSION**
区分一系列服务，通常通过工作负载二进制文件的不同版本来帮助确定。在一些场景多服务版本是需要的，比如 A/B 测试和金丝雀发布。

**SOURCE**
Source 是 Envoy 代理的下游客户端。

**TLS ORIGINATION**
TLS源发生于一个被配置为接收内部未加密 HTTP 连接的 Istio 代理（sidecar 或 egress gateway）加密请求并使用简单或双向 TLS 将其转发至安全的 HTTPS 服务器时。这与 TLS 终止相反，后者发生于一个接受 TLS 连接的 ingress 代理解密 TLS 并将未加密的请求传递到网格内部的服务时。

**TRUST DOMAIN**
信任域对应于系统的信任根，并且是工作负载标识的一部分。
Istio 使用信任域在网格中创建所有身份。每个网格都有一个专用的信任域。

**WORKLOAD**

**WORKLOAD INSTANCE**

**WORKLOAD INSTANCE PRINCIPAL**
工作负载实例主体是工作负载实例的可验证权限。Istio 的服务到服务身份验证用于生成工作负载实例主体。









<br>
<br>

---

<br>
<br>








































# 安装

Setup


<br>
<br>


## 入门

Getting Started

本指南帮你快速评估 Istio。你需要有一个k8s集群。

按照以下步骤开始使用Istio:

1. 下载并安装Istio
2. 部署示例应用
3. 对外开放应用
4. 查看仪表盘


<br>
<br>


### 下载Istio

在github istio: <https://github.com/istio/istio/releases> 上下载对应发行版压缩包并解压。


<br>
<br>


### 安装Istio

对于本次安装，我们采用 demo 配置组合。 选择它是因为它包含了一组专为测试准备的功能集合，另外还有用于生产或性能测试的配置组合。

```sh
istioctl install --set profile=demo -y

# 给命名空间添加标签，指示 Istio 在部署应用的时候，自动的注入 Envoy sidecar
kubectl label namespace default istio-injection=enabled
```


<br>
<br>


### 部署示例应用

```sh
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

kubectl get services
kubectl get pods

kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -s productpage:9080/productpage | grep -o "<title>.*</title>"
```


<br>
<br>


### 开放外部访问

此时，BookInfo 应用已经部署，但还不能被外界访问。 要开放访问，你需要创建 Istio 入站网关（Ingress Gateway）, 它会在网格边缘把一个路径映射到路由。

```sh
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml

istioctl analyze
```

<br>

```yaml
# bookinfo-gateway.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: bookinfo-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
        exact: /productpage
    - uri:
        prefix: /static
    - uri:
        exact: /login
    - uri:
        exact: /logout
    - uri:
        prefix: /api/v1/products
    route:
    - destination:
        host: productpage
        port:
          number: 9080
```

<br>

关于上面的ingressgateway，可访问nodeport地址，来验证应用的外部访问。

访问`nodeport地址/productpage`，查看效果。


<br>
<br>


### 查看仪表板

View the dashboard

Istio 和几个遥测应用做了集成。 遥测能帮你了解服务网格的结构、展示网络的拓扑结构、分析网格的健康状态。

使用下面说明部署 Kiali 仪表板、 以及 Prometheus、 Grafana、 还有 Jaeger。

```
kubectl apply -f samples/addons

kubectl rollout status deployment/kiali -n istio-system

# 或者给这个svc创建一个外部访问内
istioctl dashboard kiali
```

![](/images/Istio/kiali-example2.png)


<br>
<br>


#### bookinfo的请求示例图

要在 Istio 中运行这一应用，无需对应用自身做出任何改变。 您只要简单的在 Istio 环境中对服务进行配置和运行，具体一点说就是把 Envoy sidecar 注入到每个服务之中。

所有的微服务都和 Envoy sidecar 集成在一起，被集成服务所有的出入流量都被 sidecar 所劫持，这样就为外部控制准备了所需的 Hook，然后就可以利用 Istio 控制平面为应用提供服务路由、遥测数据收集以及策略实施等功能。

![](/images/Istio/withistio.svg)



















<br>

---

<br>















# 任务

Tasks


<br>


## 流量管理

Traffic Management

演示 Istio 的流量路由功能的任务。


<br>


### 请求路由

Request Routing

将请求动态路由到微服务的多个版本。


<br>


#### 开始之前

安装Istio。安装Bookinfo示例应用。


<br>
<br>


#### 关于

Isito Bookinfo示例包含四个独立的微服务，每个微服务有多个版本。

此任务的最初目标是应用将所有流量路由到微服务的v1的规则。稍后，您将应用规则根据 HTTP 请求 header 的值路由流量。


<br>
<br>


#### 应用虚拟服务

Apply a virtual service

要仅路由到一个版本，请应用为微服务设置默认版本的 virtual service。在这种情况下，virtual service 将所有流量路由到每个微服务的 v1 版本。

如果您还没有应用 destination rule，请先[应用默认目标规则](https://istio.io/latest/zh/docs/examples/bookinfo/#apply-default-destination-rules)

```
# 没有启用双向 TLS
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml

kubectl get destinationrules
```

<br>

```yaml
# virtual-service-all-v1.yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: productpage
spec:
  hosts:
  - productpage
  http:
  - route:
    - destination:
        host: productpage
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - route:
    - destination:
        host: ratings
        subset: v1
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: details
spec:
  hosts:
  - details
  http:
  - route:
    - destination:
        host: details
        subset: v1
---
```

<br>

```sh
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

kubectl get virtualservices -o yaml

kubectl get destinationrules -o yaml
```

您已将 Istio 配置为路由到 Bookinfo 微服务的 v1 版本，最重要的是 reviews 服务的版本 1。


<br>
<br>


#### 测试路由

Test the new routing configuration

您可以通过再次刷新 Bookinfo 应用程序的 `/productpage` 轻松测试新配置。它将把所有流量路由到版本1。


<br>
<br>


#### 基于用户身份的路由

Route based on user identity

接下来，您将更改路由配置，以便将来自特定用户的所有流量路由到特定服务版本。在这，来自名为 Jason 的用户的所有流量将被路由到服务 `reviews:v2`。

请注意，Istio 对用户身份没有任何特殊的内置机制。事实上，productpage 服务在所有到 reviews 服务的 HTTP 请求中都增加了一个自定义的 end-user 请求头，从而达到了本例子的效果。

```yaml
# virtual-service-reviews-test-v2.yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

<br>

```sh
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml

kubectl get virtualservice reviews -o yaml
```

现在访问`/productpage`，并以jason用户登录。你将看到不通的版本。以其他用户登录，刷新后，看看是不是又不一样了。

您已成功配置 Istio 以根据用户身份路由流量。


<br>
<br>


#### 理解原理

Understanding what happened

在此任务中，您首先使用 Istio 将 100% 的请求流量都路由到了 Bookinfo 服务的 v1 版本。 然后设置了一条路由规则，它根据 productpage 服务发起的请求中的 end-user 自定义请求头内容，选择性地将特定的流量路由到了 reviews 服务的 v2 版本。



<br>
<br>



### 故障注入

Fault Injection

如何注入故障并测试应用程序的弹性。


<br>
<br>


#### 开始之前

在前面的请求路由中，配置了如下请求流程：

```
productpage → reviews:v2 → ratings (针对 jason 用户)
productpage → reviews:v1 (其他用户)
```


<br>
<br>


#### 注入http延迟故障

Injecting an HTTP delay fault

为了测试微服务应用程序 Bookinfo 的弹性，我们将为用户 jason 在 reviews v2 和 ratings 服务之间注入一个 7 秒的延迟。 这个测试将会发现一个故意引入 Bookinfo 应用程序中的 bug。

注意 reviews v2 服务对 ratings 服务的调用具有 10 秒的硬编码连接超时。 因此，尽管引入了7秒的延迟，我们仍然期望端到端的流程是没有任何错误的。

<br>

```sh
# 创建故障注入规则以延迟来自测试用户 jason 的流量
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml

kubectl get virtualservice ratings -o yaml
```

<br>

```yaml
# virtual-service-ratings-test-delay.yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    fault:
      delay:
        percentage:
          value: 100.0
        fixedDelay: 7s
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```


<br>
<br>


#### 测试延迟

Testing the delay configuration

使用jason用户访问 `/productpage`页面。你期望 Bookinfo 主页在大约 7 秒钟加载完成并且没有错误。

但是，出现了一个问题：Reviews 部分显示了错误消息：

```
Error fetching product reviews!
Sorry, product reviews are currently unavailable for this book.
```

打开浏览器开发工具、网络，重新加载productpage页面，会看到页面加载实际上用了大约 6s(x-envoy-upstream-service-time: 6031)。


<br>
<br>


#### 理解原理

Understanding what happened

你发现了一个 bug。微服务中有硬编码超时，导致 reviews 服务失败。

按照预期，我们引入的 7 秒延迟不会影响到 reviews 服务，因为 reviews 和 ratings 服务间的超时被硬编码为 10 秒。但是，在 productpage 和 reviews 服务之间也有一个 3 秒的硬编码的超时，再加 1 次重试，一共 6 秒。 结果，productpage 对 reviews 的调用在 6 秒后提前超时并抛出错误了。

这种类型的错误可能发生在典型的由不同的团队独立开发不同的微服务的企业应用程序中。 Istio 的故障注入规则可以帮助您识别此类异常，而不会影响最终用户。

请注意，此次故障注入限制为仅影响用户 jason。如果您以任何其他用户身份登录，则不会遇到任何延迟。


<br>
<br>


#### 错误修复

Fixing the bug

这种问题通常会这么解决：

1. 增加 productpage 与 reviews 服务之间的超时或降低 reviews 与 ratings 的超时
2. 终止并重启修复后的微服务
3. 确认 productpage 页面正常响应且没有任何错误

但是，reviews 服务的 v3 版本已经修复了这个问题。 reviews v3 服务已将 reviews 与 ratings 的超时时间从 10 秒降低为 2.5 秒，因此它可以兼容（小于）下游的 productpage 的请求。

如果您按照流量转移任务所述将所有流量转移到 reviews v3， 您可以尝试修改延迟规则为任何低于 2.5 秒的数值，例如 2 秒，然后确认端到端的流程没有任何错误。


<br>
<br>


#### 注入HTTP中止

Injecting an HTTP abort fault

测试微服务弹性的另一种方法是引入 HTTP abort 故障。 这个任务将给 ratings 微服务为测试用户 jason 引入一个 HTTP abort。

在这种情况下，我们希望页面能够立即加载，同时显示 Ratings service is currently unavailable 这样的消息。

<br>

```sh
# 为用户 jason 创建一个发送 HTTP abort 的故障注入规则
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml

kubectl get virtualservice ratings -o yaml
```

<br>

```yaml
# virtual-service-ratings-test-abort.yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    fault:
      abort:
        percentage:
          value: 100.0
        httpStatus: 500
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```


<br>
<br>


#### 测试http中止

Testing the abort configuration

使用jason用户登录到productpage页面。

如果规则成功传播到所有的 pod，您应该能立即看到页面加载并看到 Ratings service is currently unavailable 消息。

如果您注销用户 jason 或在匿名窗口（或其他浏览器）中打开 Bookinfo 应用程序， 您将看到 productpage 为除 jason 以外的其他用户调用了 reviews v1（完全不调用 ratings）。 因此，您不会看到任何错误消息。


<br>
<br>


### 流量转移

Traffic Shifting

本任务将向您展示如何逐步将流量从一个版本的微服务迁移到另一个版本。例如，您可以将流量从旧版本迁移到新版本。

一个常见的用例是将流量从一个版本的微服务逐渐迁移到另一个版本。在 Istio 中，您可以通过配置一系列规则来实现此目标， 这些规则将一定百分比的流量路由到一个或另一个服务。

在本任务中，您将会把 50％ 的流量发送到 reviews v1，另外 50％ 的流量发送到 reviews v3。然后，再把 100％ 的流量发送到 reviews v3 来完成迁移。


<br>
<br>


#### 基于权重的路由

Apply weight-based routing

```yaml
# 将所有流量路由到各个微服务的 v1 版本
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

# 把 50% 的流量从 reviews v1 转移到 reviews v3
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml

kubectl get virtualservice reviews -o yaml
```

<br>

```yaml
# virtual-service-reviews-50-v3.yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 50
    - destination:
        host: reviews
        subset: v3
      weight: 50
```

<br>

刷新浏览器的productpage页面，大约有 50% 的几率会看到页面中出带 红色 星级的评价内容。这是因为 v3 版本的 reviews 访问了带星级评级的 ratings 服务，但 v1 版本却没有。

<br>

```sh
# 如果您认为 reviews v3 微服务已经稳定，你可以通过应用此 virtual service 规则将 100% 的流量路由到 reviews v3
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-v3.yaml
```

现在，当您刷新 productpage 时，您将始终看到带有 红色 星级评分的书评。


<br>
<br>


#### 理解原理

在这项任务中，我们使用 Istio 的权重路由功能将流量从旧版本的 reviews 服务迁移到新版本。 请注意，这和使用容器编排平台的部署功能来进行版本迁移完全不同，后者使用了实例扩容来对流量进行管理。

使用 Istio，两个版本的 reviews 服务可以独立地进行扩容和缩容，而不会影响这两个服务版本之间的流量分发。



<br>
<br>



### TCP流量转移

TCP Traffic Shifting

本任务展示了如何逐步将 TCP 流量从微服务的一个版本迁移到另一个版本。例如，将 TCP 流量从旧版本迁移到新版本。
























