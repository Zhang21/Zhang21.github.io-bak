---
title: Flagger
date: 2021-01-14 19:25:19
mathjax: true
categories: DevOps
tags:
  - K8s
  - CNCF
  - DevOps
  - GitOps
  - Canary
  - ServiceMesh
---


参考:

- Flagger github: <https://github.com/fluxcd/flagger>
- Flagger docs: <https://docs.flagger.app/>


<br>

---

<!--more-->

<br>



# 介绍

Introduction

Flagger是一个渐进式交付的k8s operator，可使用Istio, Linkerd, App Mesh, Nginx, Sgipper, Contour, Gloo, Traefik路由流量漂移(traffic shifting)，自动化进行金丝雀部署，Prometheus指标用于金丝雀分析。金丝雀分析可通过webhook扩展，用于运行系统集成/测试验收、负载测试，或其它自定义验证。

Flagger实现了一个控制循环(control loop)，逐渐向金丝雀转移流量，同时测量如HTTP请求成功率、请求平均持续时间、pod健康度等关键性能指标。基于对KPIs的分析，提升(promoted)或中止(aborted)金丝雀，分析结果发布到Slack等其它软件。

<br>

Flagger架构图：

![](/images/Flagger/flagger-canary-overview.png)

<br>

Flagger可以k8s CRD进行配置，并与任何用于k8s ci/cd 方案兼容。由于Flagger是声明性的，并对k8s events做出反应，它可与Flux CD或JenkinsX一起用于GitOps管道中。

Flagger是一个CNCF项目。



<br>

---

<br>




# 快速开始

Getting started

要开始使用Flagger，请选择一个受支持的路由提供商，并安装它(Helm/Kustmoze)。

安装Flagger之后，可从以下指导手册开始：

**Service mesh**

- Istio
- Linkerd
- aws app mesh

**Ingress controller**

- Contour
- Gloo
- Nginx ingress
- skipper ingress
- Traefik

**Hand-on Gitops**

- Istio
- Linkerd
- aws app mesh




<br>

---

<br>




# 安装

Install

<br>

## k8s

Flagger Install on Kubernetes

<br>

### 先决条件

Prerequisites

k8s v1.16+


<br>
<br>


### Helm安装Flagger

Install Flagger with Helm


你可在任意namespace安装Flagger，只要它能够访问Prometheus。

```bash
# 添加helm 仓库
helm repo add flagger https://flagger.app


# 安装Flagger Canary CRD
kubectl apply -f https://raw.githubusercontent.com/fluxcd/flagger/main/artifacts/flagger/crd.yaml


# Deploy Flagger for Istio
# Flagger依赖Istio telemetry和Prometheus
helm upgrade -i flagger flagger/flagger \
--namespace=istio-system \
--set crd.create=false \
--set meshProvider=istio \
--set metricsServer=http://prometheus:9090
```

<br>

对于Istio多集群共享控制面板，你可在每个远程集群上安装Flagger，并设置Istio控制面板host cluster kubeconfig:

```bash
helm upgrade -i flagger flagger/flagger \
--namespace=istio-system \
--set crd.create=false \
--set meshProvider=istio \
--set metricsServer=http://istio-cluster-prometheus:9090 \
--set istio.kubeconfig.secretName=istio-kubeconfig \
--set istio.kubeconfig.key=kubeconfig
```

注意，Istio kubeconfig必须存储为k8s secret，其数据键名称为`kubeconfig`。关于如果配置Istio multi-cluster credential的详细信息，请参考Istio文档: <https://istio.io/docs/setup/install/multicluster/shared-vpn/#credentials>

<br>

```bash
# Deploy Flagger for Linkerd
helm upgrade -i flagger flagger/flagger \
--namespace=linkerd \
--set crd.create=false \
--set meshProvider=linkerd \
--set metricsServer=http://linkerd-prometheus:9090
```

<br>

```bash
# Deploy Flagger for App Mesh
helm upgrade -i flagger flagger/flagger \
--namespace=appmesh-system \
--set crd.create=false \
--set meshProvider=appmesh \
--set metricsServer=http://appmesh-prometheus:9090
```

<br>

对于ingress controlles，安装指南如下：

- [Contour](https://docs.flagger.app/tutorials/contour-progressive-delivery)
- [Gloo](https://docs.flagger.app/tutorials/gloo-progressive-delivery)
- [Nginx](https://docs.flagger.app/tutorials/nginx-progressive-delivery)
- [Skipper](https://docs.flagger.app/tutorials/skipper-progressive-delivery)
- [Traefik](https://docs.flagger.app/tutorials/traefik-progressive-delivery)


<br>
<br>


### Helm安装Grafana

Install Grafana with Helm

Flagger使用Grafana来监测金丝雀分析。

```bash
# Deploy Grafana in the istio-system namespace
helm upgrade -i flagger-grafana flagger/grafana \
--namespace=istio-system \
--set url=http://prometheus.istio-system:9090 \
--set user=admin \
--set password=change-me


# 使用端口转发访问Grafana
kubectl -n istio-system port-forward svc/flagger-grafana 3000:80
```


<br>
<br>


### Kustomize安装Flagger



<br>
<br>



## gke istio



<br>
<br>



## eks app mesh



<br>

---

<br>




# 使用

Usage


<br>
<br>


## 它如何工作

How it works

Flagger可以配置使用名为`canary`的自定义资源来自动化k8s工作负载发布过程。


<br>


### Canary resource

canary CRD定义了运行在k8s上的程序的发布过程，它是跨集群、服务网格、Ingress提供商可移植。

对一个名为podinfo的deployment，具有渐进式流量迁移的金丝雀发布(canary release)如下所定义。

```yaml
apiVersion: flagger.app/v1beta1
kind: Canary
metadata:
  name: podinfo
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: podinfo
  service:
    port: 9898
  analysis:
    interval: 1m
    threshold: 10
    maxWeight: 50
    stepWeight: 5
    metrics:
      - name: request-success-rate
        thresholdRange:
          min: 99
        interval: 1m
      - name: request-duration
        thresholdRange:
          max: 500
        interval: 1m
    webhooks:
      - name: load-test
        url: http://flagger-loadtester.test/
        metadata:
          cmd: "hey -z 1m -q 10 -c 2 http://podinfo-canary.test:9898/"
```

当部署一个应用的新版本时，Flagger逐渐地向金丝雀(canary)转移流量，同时测量请求成功率以及响应持续时间。你可以使用自定义指标扩展金丝雀分析，接受和负载测试来硬化应用发布过程的验证过程。

如果你在同一个集群内运行了多个服务网格(service mesh)或ingress controller，则可以使用`spec.provider`配置金丝雀来覆盖全局配置。


<br>
<br>


### Canary target

一个金丝雀资源可以目标到一个k8s deployment或daemonset。

k8s deployment示例:

```yaml
spec:
  progressDeadlineSeconds: 60
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: podinfo
  autoscalerRef:
    apiVersion: autoscaling/v2beta2
    kind: HorizontalPodAutoscaler
    name: podinfo
```

根据上述配置，Flagger生成如下k8s对象:

- `deployment/<targetRef.name>-primary`
- `hpa/<autoscalerRef.name>-primary`

primary deployment被视为应用稳定版本，默认情况下所有流量都路由到此版本，并且target deployment将缩放为0。Flagger会检测targer deployment（包括secrets和configmaps）的更改，并在新版本替换为主版本(primary)之前执行金丝雀分析。

注意，target deployment必须具有这种格式的单个标签(single label): `app: <deployment-name>`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: podinfo
spec:
  selector:
    matchLabels:
      app: podinfo
  template:
    metadata:
      labels:
        app: podinfo
```

除了`app`，Flagger支持`name`和`app.kubernetes.io/name`选择器(selector)。如果使用不同的惯例，你可以在Flagger deployment的容器参数下指定`-selector-labels=my-app-label`命令行标志，或在使用Helm安装Flagger时设置`--set selectorLabels=my-app-label`，来指定特定的标签。

如果target deployment使用了secrets或configmaps，Flagger将使用`--primary`后缀创建每个对象的副本，并将在primary deployment中引用这些对象。如果在configmap, secret中注释了`flagger.app/config-tracking: disabled`，Flagger将为primary deployment使用相同的对象，而不是做一个primary的副本。可在Flagger deployment的容器参数中使用`-enable-config-tracking=false`命令行标志，或在使用Helm安装Flagger时设置`--set configTracking.enabled=false`来全局禁用secrets/configmaps tracking。不过不建议全局禁用，建议使用注释(annotation)。

自动伸缩(autoscaler)引用是可选的，当指定时，在target和primary deployment在伸缩时，Flagger将暂停流量增加(traffic increase)。HPA可以帮助减少金丝雀分析期间的资源使用情况。

进度截止时间表示canary deployment在回滚之前进行的最大时间，默认为十分钟。


<br>
<br>


### Canary service

金丝雀资源决定了如何在集群内公开工作负载。金丝雀目标应该公开一个TCP端口，由Flagger去创建一个ClusterIP服务。

```yaml
spec:
  service:
    name: podinfo
    port: 9898
    portName: http
    targetPort: 9898
    portDiscovery: true
```

如果工作负载使用gRPC（默认是http），则portName应设置为`grpc`。

如果启用了端口发现(port discovery)，Flagger扫描目标工作负载并提取排除了canary service和service mesh sidecar端口的容器端口。生成CluterIP服务时会使用这些端口。

基于canary spec service，Flagger创建如下的k8s ClusterIP service:

- `<service.name>.<namespace>.svc.cluster.local`，selector `app=<name>-primary`
- `<service.name>-primary.<namespace>.svc.cluster.local`，selector `app=<name>-primary`
- `<service.name>-canary.<namespace>.svc.cluster.local`，selector `app=<name>`

这可确保流量到`podinfo.test:9898`将被路由到应用的最新稳定版本。`podinfo-canary.test:9898`这个地址仅在金丝雀分析期间可用，可用于符合测试或负载测试。

可以配置Flagger为service生成annotations和labels：

```yaml
spec:
  service:
    port: 9898
    apex:
      annotations:
        test: "test"
      labels:
        test: "test"
    canary:
      annotations:
        test: "test"
      labels:
        test: "test"
    primary:
      annotations:
        test: "test"
      labels:
        test: "test"
```

除了端口映射和元数据，service spec可包含URI和重定向规则，超时和重试策略：

```yaml
spec:
  service:
    port: 9898
    match:
      - uri:
          prefix: /
    rewrite:
      uri: /
    retries:
      attempts: 3
      perTryTimeout: 1s
    timeout: 5s
```

当使用Istio时，你还可以指定HTTP headers, CORS, traffic policies, Istio gateway, hosts。Istio的路由配置请参考: <https://docs.flagger.app/faq#istio-routing>


<br>
<br>


### Canary status

可使用kubectl来获取canary deployment的当前状态:

```
kubectl get canaries --all-namespaces

NAMESPACE   NAME      STATUS        WEIGHT   LASTTRANSITIONTIME
test        podinfo   Progressing   15       2019-06-30T14:05:07Z
prod        frontend  Succeeded     0        2019-06-30T16:15:07Z
prod        backend   Failed        0        2019-06-30T17:05:07Z
```

状态条件反映了金丝雀分析的最后一个已知状态:

```
kubectl -n test get canary/podinfo -oyaml | awk '/status/,0'
```

一个成功的rollout状态:

```
status:
  canaryWeight: 0
  failedChecks: 0
  iterations: 0
  lastAppliedSpec: "14788816656920327485"
  lastPromotedSpec: "14788816656920327485"
  conditions:
  - lastTransitionTime: "2019-07-10T08:23:18Z"
    lastUpdateTime: "2019-07-10T08:23:18Z"
    message: Canary analysis completed successfully, promotion finished.
    reason: Succeeded
    status: "True"
    type: Promoted
```

CD样例:

```
# update the container image
kubectl set image deployment/podinfo podinfod=stefanprodan/podinfo:3.0.1

# wait for Flagger to detect the change
ok=false
until ${ok}; do
    kubectl get canary/podinfo | grep 'Progressing' && ok=true || ok=false
    sleep 5
done

# wait for the canary analysis to finish
kubectl wait canary/podinfo --for=condition=promoted --timeout=5m

# check if the deployment was successful 
kubectl get canary/podinfo | grep Succeeded
```


<br>
<br>


### Canary finalizers

Flagger对金丝雀删除(canary deletion)的默认行为是 离开(leave)不受当前状态控制器拥有的资源。这简化了删除操作，避免了在资源最终确定期间可能的死锁。如果金丝雀被引入现有资源，他们将在初始化阶段变异(mutated)，并且不再反映它们的初始状态。如果删除时所需的功能是将资源还原到他们的初始阶段，则可以启动`revertOnDeletion`属性。

```yaml
spec:
  revertOnDeletion: true
```

当一个删除操作提交到集群，Flagger经尝试恢复(revert)以下资源：

- canary target副本将更新到primary副本数
- canary service选择器将被恢复
- mesh/ingress 流量路由到target

推荐的方法是禁用金丝雀分析，将使用`skipAnalysis`属性，这限制了资源和解(reconciliation)的需求。当不再计划依赖Flagger部署管理时，应该启用`revertOnDeletion`属性。


<br>
<br>


### Canary analysis

金丝雀部署定义了：

- 部署策略的类型
- 验证金丝雀版本的指标
- 一致性测试、负载测试和手动门控的webhooks
- 告警设置

spec:

```yaml
  analysis:
    # schedule interval (default 60s)
    interval:
    # max number of failed metric checks before rollback
    threshold:
    # max traffic percentage routed to canary
    # percentage (0-100)
    maxWeight:
    # canary increment step
    # percentage (0-100)
    stepWeight:
    # promotion increment step
    # percentage (0-100)
    stepWeightPromotion:
    # total number of iterations
    # used for A/B Testing and Blue/Green
    iterations:
    # canary match conditions
    # used for A/B Testing
    match:
      - # HTTP header
    # key performance indicators
    metrics:
      - # metric check
    # alerting
    alerts:
      - # alert provider
    # external checks
    webhooks:
      - # hook
```

金丝雀分析定期运行，直到它达到最大流量权重或迭代次数。在每个运行时，Flagger调用webhook，检查指标，如果达到失败的检查阈值，则停止分析并回滚金丝雀。如果配置了告警，Flagger将发送分析结果。



<br>
<br>


## 部署策略

Deployment Strategies: <https://docs.flagger.app/usage/deployment-strategies>

Flagger可以为以下部署策略自动运行应用分析(application analysis)，提升(promotion)，回滚(rollback):

- 金丝雀发布(Canary Release, progressive traffic shifting): Istio, Linkerd, App Mesh, NGINX, Skipper, Contour, Gloo Edge, Traefik
- AB测试(A/B Testing, HTTP headers and cookies traffic routing): Istio, App Mesh, NGINX, Contour, Gloo Edge
- 蓝绿(Blue/Green, traffic switching): Kubernetes CNI, Istio, Linkerd, App Mesh, NGINX, Contour, Gloo Edge
- 蓝绿镜像(Blue/Green Mirroring, traffic shadowing): Istio

<br>

对于金丝雀发布和A/B测试，你应该使用7层流量管理方案，如service mesh或ingress controller。
对于蓝绿部署，不需要service mesh 或 ingress controller。

可通过改变以下对象来触发一个金丝雀分析：

- Deployment PodSpec (container image, command, ports, env, resources, etc)
- ConfigMaps mounted as volumes or mapped to environment variables
- Secrets mounted as volumes or mapped to environment variables


<br>
<br>


### 金丝雀发布

Canary Release

Flagger实现了一个控制循环(control loop)，可以逐渐地将流量转移到金丝雀，同时测量关键性能指标（如http请求成功率，请求平均耗时，Pod健康状态...）。

<br>

Flagger金丝雀阶段：

![](/images/Flagger/flagger-canary-steps.png)

金丝雀分析会定期运行，直到达到最大流量权重或检查失败。

<br>

spec:

```yaml
  analysis:
    # schedule interval (default 60s)
    interval: 1m
    # max number of failed metric checks before rollback
    threshold: 10
    # max traffic percentage routed to canary
    # percentage (0-100)
    maxWeight: 50
    # canary increment step
    # percentage (0-100)
    stepWeight: 2
    # promotion increment step (default 100)
    # percentage (0-100)
    stepWeightPromotion: 100
  # deploy straight to production without
  # the metrics and webhook checks
  skipAnalysis: false
```

以上分析，如果成功，将运行25分钟，每分钟验证HTTP指标和webhokk。

你可以使用以下供使确定验证和升级金丝雀部署所需的最短时间：

```
interval * (maxWeight / stepWeight)

# 上面
1 * ( 50 / 2)
```

当指标检测失败时，金丝雀回滚需要的时间：

```
interval * threshold

# 上面
1 * 10
```

当指定了`stepWeightPromotion`，promotion阶段发生在stages中，流量以逐步的方式路由回primary pod，主权重逐渐增加直到100%。

<br>

在紧急情况下，你可能希望提升分析阶段并直接应用变更到生产环境。你随时都可以设置`spec.skipAnalysis: true`。当启用了跳过分析，Flagger会检查金丝雀部署是否健康，并在不分析它的情况下提升它(promote)。如果正在进行分析，则Flagger回取消分析并运行提升(promotion)。

Gated canary promotion stages:

- 扫描金丝雀部署（scan for canary deployments）
- 检查主和金丝雀部署状态（check primary and canary deployment status）
  - 如果正在进行滚动更新，则停止（halt advancement if a rolling update is underway）
  - 如果pod不健康，则停止（halt advancement if pods are unhealthy）
- 确认部署并检查结果（call confirm-rollout webhooks and check results）
  - 如果任意hook返回非http 2xx结果，则停止提升（halt advancement if any hook returns a non HTTP 2xx result）
- 预览部署并检查结果（call pre-rollout webhooks and check results）
  - 如果任意hook返回非http 2xx结果，则停止提升
  - 递增失败的检查计数器（increment the failed checks counter）
- 增加金丝雀流量权重（increase canary traffic weight percentage from 0% to 2% (step weight)）
- 调用推出并检查结果（call rollout webhooks and check results）
- 检查金丝雀HTTP请求成功率和延迟（call rollout webhooks and check results）
  - 如果任意指标都在指定的阈值下，则停止提升（halt advancement if any metric is under the specified threshold）
  - 递增失败的检查计数器
- 检查失败的检查的数量是否达到了阈值（check if the number of failed checks reached the threshold）
  - 路由所有流量到主（route all traffic to primary）
  - 将金丝雀部署缩减为0并标记为失败（scale to zero the canary deployment and mark it as failed）
  - 调用推出后的钩子（call post-rollout webhooks）
  - 发送分析结果（post the analysis result to Slack）
  - 等待金丝雀部署更新或重新开始（wait for the canary deployment to be updated and start over）
- 增加金丝雀流量直到最大（increase canary traffic weight by 2% (step weight) till it reaches 50% (max weight) ）
  - 如果任意狗子调用失败，则停止提升（halt advancement if any webhook call fails）
  - 金丝雀请求成功率在阈值之下，则停止提升（halt advancement while canary request success rate is under the threshold）
  - 金丝雀请求持续时间超过阈值，则停止提升（halt advancement while canary request duration P99 is over the threshold）
  - 任意自定义指标检查失败，则停止提升（halt advancement while any custom metric check fails）
  - 如果primary或金丝雀部署不健康，则停止提升（halt advancement if the primary or canary deployment becomes unhealthy ）
  - 金丝雀部署正通过HPA进行伸缩，则停止提升（halt advancement while canary deployment is being scaled up/down by HPA）
- 调用确认提升钩子并检查结果（call confirm-promotion webhooks and check results）
  - 如果任意hook返回非http 2xx结果，则停止提升
- 提升金丝雀为主（promote canary to primary）
  - 将configmap和secret从金丝雀复制到主（copy ConfigMaps and Secrets from canary to primary）
  - 复制金丝雀部署spec模板（copy canary deployment spec template over primary）
- 等待主滚动更新完成（wait for primary rolling update to finish）
  - 如果pod不健康，则停止提升
- 路由所有流量到主（route all traffic to primary）
- 将金丝雀部署伸缩为0（scale to zero the canary deployment）
- 标记推出为完成（mark rollout as finished）
- 调用推出后钩子（call post-rollout webhooks）
- 发送金丝雀分析结果通知（send notification with the canary analysis result）
- 等待金丝雀部署更新并重新开始（wait for the canary deployment to be updated and start over）


<br>
<br>


#### Rollout Weights

默认情况下，Flagger对提升使用线性权重值（开始值，步进值，最大权重）。

栗子:

```yaml
canary:
  analysis:
    promotion:
      maxWeight: 50
      stepWeight: 20
```

此配置执行分析，从20开始，每次递增20，直到权重到达50。

```
20 (20 : 80)
40 (40 : 60)
60 (60 : 40)
promotion
```

<br>

要启用非线性提升，可以使用`stepWeights`参数（在金丝雀提升期间使用有序权重数组）。

栗子:

```
canary:
  analysis:
    promotion:
      stepWeights: [1, 2, 10, 80]
```

此配置分析，从1开始，经过有序权重直到80。

```
1 (1 : 99)
2 (2 : 98)
10 (10 : 90)
80 (20 : 60)
promotion
```


<br>
<br>


### AB测试

A/B Testing

对于需要会话关联(session affinity)的前端应用，你应该使用HTTP headers或cookie匹配条件，以便在整个金丝雀分析期间，确保一组用户将保持同一应用版本。

![](/images/flagger/flagger-abtest-steps.png)

<br>

你可以通过指定HTTP匹配条件和迭代次数来启用AB测试。如果Flagger发现HTTP匹配条件，它会忽略`maxWeight`和`stepWeight`设置。

Istio栗子：

```yaml
  analysis:
    # schedule interval (default 60s)
    interval: 1m
    # total number of iterations
    iterations: 10
    # max number of failed iterations before rollback
    threshold: 2
    # canary match condition
    match:
      - headers:
          x-canary:
            regex: ".*insider.*"
      - headers:
          cookie:
            regex: "^(.*?;)?(canary=always)(;.*)?$"
```

```yaml
  analysis:
    interval: 1m
    threshold: 10
    iterations: 2
    match:
      - headers:
          x-canary:
            exact: "insider"
      - headers:
          cookie:
            regex: "^(.*?;)?(canary=always)(;.*)?$"
      - sourceLabels:
          app.kubernetes.io/name: "scheduler"
```

header key必须小写，并使用作为分隔符。header value是大小写敏感的。

- `exact: "value"` for exact string match
- `prefix: "value"` for prefix-based match
- `suffix: "value"` for suffix-based match
- `regex: "value"` for RE2 style regex-based match

请注意，只有当网格网关包含在`canary.service.gateways`列表时，才适用`sourceLabels`匹配条件。

App Mesh栗子：

```yaml
  analysis:
    interval: 1m
    threshold: 10
    iterations: 2
    match:
      - headers:
          user-agent:
            regex: ".*Chrome.*"
```

注意，App Mesh支持单个条件。

<br>

轮廓栗子:

```yaml
  analysis:
    interval: 1m
    threshold: 10
    iterations: 2
    match:
      - headers:
          user-agent:
            prefix: "Chrome"
```

请注意，contour不支持正则，你可以使用prefix, suffix, exact。

<br>

nginx栗子：

```yaml
  analysis:
    interval: 1m
    threshold: 10
    iterations: 2
    match:
      - headers:
          x-canary:
            exact: "insider"
      - headers:
          cookie:
            exact: "canary"
```

请注意，nginx ingress controller仅支持cookies的精确匹配，其中值必须设置为`always`。从nginx ingress v0.31开始，header values支持正常匹配。

以上配置将会路由带有x-canary header或canary cookie的用户:

```bash
curl -H 'X-Canary: insider' http://app.example.com
curl -b 'canary=always' http://app.example.com
```



<br>
<br>



### 蓝绿部署

Blue/Green Deployments

对于未使用服务网格部署的应用，Flagger可使用k8s 四层网络的蓝绿格式的部署。使用Istio，你可以在蓝绿之间选择镜像流量。

![](/images/Flagger/flagger-bluegreen-steps.png)

<br>

你可以在`analysis` spec中使用`iterations`替换`stepWeight/maxWeight`来使用蓝绿部署策略：

```yaml
  analysis:
    # schedule interval (default 60s)
    interval: 1m
    # total number of iterations
    iterations: 10
    # max number of failed iterations before rollback
    threshold: 2
```

上面的配置，Flagger将在canary pod上运行一致性和负载测试10分钟。如果指标分析成功，当金丝雀提升时，实时流量(live traffic)将从旧版本切换到新版本。

蓝绿部署策略支持多个服务网格提供商。

服务网格的蓝绿推出步骤（Blue/Green rollout steps for service mesh）：

- 检测新的修订（detect new revision (deployment spec, secrets or configmaps changes)）
- 扩展金丝雀（scale up the canary (green)）
- 运行一致性测试（run conformance tests for the canary pods）
- 每分钟为canary pods运行负载测试和指标检测（run load tests and metric checks for the canary pods every minute）
- 如果达到失败阈值，则中止金丝雀发布（abort the canary release if the failure threshold is reached）
- 路由流量到金丝雀（route traffic to canary）
- 提升主的canary spec（promote canary spec over primary (blue)）
- 等待主推出（wait for primary rollout）
- 路由流量到主（route traffic to primary）
- 缩小金丝雀（scale down canary）

分析完成后，在触发主(primary, blue)滚动更新之前，流量被路由到金丝雀(canary, green)，这确保了在k8s部署推出期间避免删除飞行中(in-flight)的请求，平滑过渡到新版本。



<br>
<br>



### 流量镜像的蓝绿部署

Blue/Green with Traffic Mirroring

流量镜像是金丝雀（逐渐流量漂移）或蓝绿部署策略的前阶段(pre-stage)。流量镜像将复制每个入请求，将请求分别发送给主和金丝雀服务。来自主的响应将发送回用户，来自金丝雀的响应将被丢弃。两个请求都会收集指标，以便在金丝雀指标健康时，部署会继续进行。

镜像应该用于幂等的请求或能够进行两次处理（一次主，一次金丝雀）。读取是幂等的。在请求上使用镜像之前，请求可能写入，你应该考虑会发生什么，如果一个写重复并且由主和金丝雀处理。

要使用镜像，将`spec.analysis.mirror`设置为f`true`。

Istio栗子：

```yaml
  analysis:
    # schedule interval (default 60s)
    interval: 1m
    # total number of iterations
    iterations: 10
    # max number of failed iterations before rollback
    threshold: 2
    # Traffic shadowing (compatible with Istio only)
    mirror: true
    # Weight of the traffic mirrored to your canary (defaults to 100%)
    mirrorWeight: 100
```

<br>

服务网格的镜像推出步骤（Mirroring rollout steps for service mesh）：

- 检测新的改动（detect new revision (deployment spec, secrets or configmaps changes)）
- 伸展金丝雀（scale from zero the canary deployment）
- 等待HPA设置金丝雀最小副本数（wait for the HPA to set the canary minimum replicas）
- 检查金丝雀pod健康状态（check canary pods health）
- 运行验收测试（run the acceptance tests）
- 如果测试失败，则中止金丝雀发布（abort the canary release if tests fail）
- 启动负载测试（start the load tests）
- 从主镜像流量到金丝雀（mirror 100% of the traffic from primary to canary）
- 检查请求成功率和每分钟请求耗时（check request success rate and request duration every minute）
- 如果达到失败阈值，则中止金丝雀发布（abort the canary release if the failure threshold is reached）
- 如果达到迭代次数，则停止流量镜像（stop traffic mirroring after the number of iterations is reached）
- 路由实时流量到金丝雀pod（route live traffic to the canary pods）
- 提升金丝雀（promote the canary (update the primary secrets, configmaps and deployment spec)）
- 等待主推出完成（wait for the primary deployment rollout to finish）
- 等待HPA设置主的最小副本数（wait for the HPA to set the primary minimum replicas）
- 检查主pod健康状况（check primary pods health）
- 将实时流量切换回主（switch live traffic back to primary）
- 缩小金丝雀（scale to zero the canary）
- 发送金丝雀分析结果通知（send notification with the canary analysis result）



<br>
<br>



## 指标分析

Metrics Analysis

作为分析过程的一部分，Flagger可以根据应用特定指标验证其可用性、错误率、平均响应时间和任何其它目标级别的服务(SLOs, service level objectives)。如果在SLOs分期期间注意到性能下降，则发布将自动回滚以最小化影响用户。


<br>
<br>


### 内置指标

Builtin metrics

Flaggger有两个内置的指标检查：HTTP请求成功率和持续时间。

```yaml
  analysis:
    metrics:
    - name: request-success-rate
      interval: 1m
      # minimum req success rate (non 5xx responses)
      # percentage (0-100)
      thresholdRange:
        min: 99
    - name: request-duration
      interval: 1m
      # maximum req duration P99
      # milliseconds
      thresholdRange:
        max: 500
```

内置的检查指标支持每个service mesh、ingress controller，并可使用Prometheus进行查询。


<br>
<br>


### 自定义指标

Custom metrics

金丝雀分析可以通过自定义检测指标进行扩展。使用`MetricTemplate`自定义资源，你可以配置Flagger连接到一个指标提供商（如Prometheus），并运行一个查询，它返回一个`float64`值。查询结果用于基于特定阈值范围来验证金丝雀。

```yaml
apiVersion: flagger.app/v1beta1
kind: MetricTemplate
metadata:
  name: my-metric
spec:
  provider:
    type: # can be prometheus or datadog
    address: # API URL
    secretRef:
      name: # name of the secret containing the API credentials
  query: # metric query
```

<br>

以下变量可用于查询模板：

- `name` (canary.metadata.name)
- `namespace` (canary.metadata.namespace)
- `target` (canary.spec.targetRef.name)
- `service` (canary.spec.service.name)
- `ingress` (canary.spec.ingresRef.name)
- `interval` (canary.spec.analysis.metrics[].interval)

金丝雀分析指标可以使用`templateRef`引用模板：

```yaml
  analysis:
    metrics:
      - name: "my metric"
        templateRef:
          name: my-metric
          # namespace is optional
          # when not specified, the canary namespace will be used
          namespace: flagger
        # accepted values
        thresholdRange:
          min: 10
          max: 1000
        # metric query time window
        interval: 1m
```


<br>
<br>


### Prometheus

你可以使用如Prometheus，并编写PromQL查询，来创建自定义检查指标目标。

Prometheus template栗子：

```yaml
apiVersion: flagger.app/v1beta1
kind: MetricTemplate
metadata:
  name: not-found-percentage
  namespace: istio-system
spec:
  provider:
    type: prometheus
    address: http://prometheus.istio-system:9090
  query: |
    100 - sum(
        rate(
            istio_requests_total{
              reporter="destination",
              destination_workload_namespace="{{ namespace }}",
              destination_workload="{{ target }}",
              response_code!="404"
            }[{{ interval }}]
        )
    )
    /
    sum(
        rate(
            istio_requests_total{
              reporter="destination",
              destination_workload_namespace="{{ namespace }}",
              destination_workload="{{ target }}"
            }[{{ interval }}]
        )
    ) * 100
```

在金丝雀分析中引用模板：

```yaml
  analysis:
    metrics:
      - name: "404s percentage"
        templateRef:
          name: not-found-percentage
          namespace: istio-system
        thresholdRange:
          max: 5
        interval: 1m
```

上面的配置验证金丝雀，通过检查HTTP 404 req/sec 百分比是否低于5%。如果大于5%阈值，那么金丝雀失败。

<br>


Prometheus gRPC错误率栗子：

```yaml
apiVersion: flagger.app/v1beta1
kind: MetricTemplate
metadata:
  name: grpc-error-rate-percentage
  namespace: flagger
spec:
  provider:
    type: prometheus
    address: http://flagger-prometheus.flagger-system:9090
  query: |
    100 - sum(
        rate(
            grpc_server_handled_total{
              grpc_code!="OK",
              kubernetes_namespace="{{ namespace }}",
              kubernetes_pod_name=~"{{ target }}-[0-9a-zA-Z]+(-[0-9a-zA-Z]+)"
            }[{{ interval }}]
        )
    )
    /
    sum(
        rate(
            grpc_server_started_total{
              kubernetes_namespace="{{ namespace }}",
              kubernetes_pod_name=~"{{ target }}-[0-9a-zA-Z]+(-[0-9a-zA-Z]+)"
            }[{{ interval }}]
        )
    ) * 100
```


<br>
<br>


### Prometheus认证

Prometheus authentication

如果Prometheus启用了认证，你可以在相同的命名空间内创建一个包含认证信息的secret。

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: prom-basic-auth
  namespace: flagger
data:
  username: your-user
  password: your-password
```

在`MetricTemplate`中引用secret：

```yaml
apiVersion: flagger.app/v1beta1
kind: MetricTemplate
metadata:
  name: my-metric
  namespace: flagger
spec:
  provider:
    type: prometheus
    address: http://prometheus.monitoring:9090
    secretRef:
      name: prom-basic-auth
```


<br>
<br>


### DataDdog



<br>
<br>



### Amazon CloudWatch


<br>
<br>


### New Relic



<br>
<br>



## Webhooks

url: <https://docs.flagger.app/usage/webhooks>


金丝雀分析可以用webhooks进行扩展延伸。如果金丝雀失败，Flagger会调用每个webhook url并检测响应状态码。

有几种类型的钩子：

- **confirm-rollout**：在扩展金丝雀部署之前执行，并可用于手动批准。推出是暂停的，直到钩子返回一个成功的HTTP状态码。
- **pre-rollout**：在将流量路由到金丝雀之前执行。如果钩子失败，并且失败次数达到阈值，金丝雀进展是将暂停， 并回滚。
- **rollout**：在指标检查之前在分析期间执行。如果失败，金丝雀进展将暂停并最终回滚。
- **confirm-promotion**：在提升之前执行。金丝雀提升是暂停的，直到钩子返回HTTP 200。当提升暂停时，Flagger将继续运行指标检查和rollout钩子。
- **post-rollout**：在金丝雀被提升或回滚之后执行。如果失败，则记录错误。
- **rollback**：在金丝雀部署处于进度和等待状态时执行。这提供了在分析期间回滚或等待确认的能力。如果它返回一个成功的HTTP状态码，Flagger将停止分析并将金丝雀发布标记为失败。
- **event**：在每次Flagger发出k8s事件时执行。配置后，在Flagger在金丝雀部署期间的每个动作都将通过HTTP POST作为JSON发送。

spec：

```yaml
  analysis:
    webhooks:
      - name: "start gate"
        type: confirm-rollout
        url: http://flagger-loadtester.test/gate/approve
      - name: "helm test"
        type: pre-rollout
        url: http://flagger-helmtester.flagger/
        timeout: 3m
        metadata:
          type: "helmv3"
          cmd: "test podinfo -n test"
      - name: "load test"
        type: rollout
        url: http://flagger-loadtester.test/
        timeout: 15s
        metadata:
          cmd: "hey -z 1m -q 5 -c 2 http://podinfo-canary.test:9898/"
      - name: "promotion gate"
        type: confirm-promotion
        url: http://flagger-loadtester.test/gate/approve
      - name: "notify"
        type: post-rollout
        url: http://telegram.bot:8080/
        timeout: 5s
        metadata:
          some: "message"
      - name: "rollback gate"
        type: rollback
        url: http://flagger-loadtester.test/rollback/check
      - name: "send to Slack"
        type: event
        url: http://event-recevier.notifications/slack
```

webhook payload(http post)：

```yaml
{
    "name": "podinfo",
    "namespace": "test",
    "phase": "Progressing", 
    "metadata": {
        "test":  "all",
        "token":  "16688eb5e9f289f1991c"
    }
}
```

响应状态码：

- 200-202，通过增加流量权重来提升金丝雀
- timeout或非2xx，停止提升并增加失败检查

在一个非2xx响应，Flagger将包含response body在失败检查日志和k8s事件中。

event payload(http post)：

```yaml
{
  "name": "string (canary name)",
  "namespace": "string (canary namespace)",
  "phase": "string (canary phase)",
  "metadata": {
    "eventMessage": "string (canary event message)",
    "eventType": "string (canary event type)",
    "timestamp": "string (unix timestamp ms)"
  }
}
```

事件接收器可以根据接收阶段创建告警（可能的值：Initialized, Waiting, Progressing, Promoting, Finalising, Succeeded或Failed）。


<br>
<br>


### 负载测试

Load Testing

对于未接收恒定流量的工作负载，Flagger可以使用webhook配置，即在调用时，为目标工作负载启动一个负载测试。如果工作负载在金丝雀分析期间没有接收任何流量，Flagger指标检查将失败（no values found for metric request-success-rate）。

Flagger的负载测试服务基于[rakyll/hey](https://github.com/rakyll/hey)，当配置为webhooks时会在分析期间生成流量。

![](/images/Flagger/flagger-load-testing.png)

<br>

```bash
# First you need to deploy the load test runner in a namespace with sidecar injection enabled
kubectl apply -k https://github.com/fluxcd/flagger//kustomize/tester?ref=main

# 或使用helm
helm repo add flagger https://flagger.app

helm upgrade -i flagger-loadtester flagger/loadtester \
--namespace=test \
--set cmd.timeout=1h

# When deployed the load tester API will be available at http://flagger-loadtester.test/
```

<br>

现在，你可以将webhook添加到金丝雀分析规范中：

```yaml
webhooks:
  - name: load-test-get
    url: http://flagger-loadtester.test/
    timeout: 5s
    metadata:
      type: cmd
      cmd: "hey -z 1m -q 10 -c 2 http://podinfo-canary.test:9898/"
  - name: load-test-post
    url: http://flagger-loadtester.test/
    timeout: 5s
    metadata:
      type: cmd
      cmd: "hey -z 1m -q 10 -c 2 -m POST -d '{test: 2}' http://podinfo-canary.test:9898/echo"
```

当金丝雀分析开始时，如果它们尚未运行，Flagger将调用webhook，负载测试器将在后台运行hey命令。这将确保在分析期间，`podinfo-canary.test`服务将收到稳定的GET和POST请求流。

<br>

对于gRPC服务，你可以使用[bojand/ghz](https://github.com/bojand/ghz)工具，它是一个类似于`hey`但用于gRPC测试的工具。

```yaml
webhooks:
  - name: grpc-load-test
    url: http://flagger-loadtester.test/
    timeout: 5s
    metadata:
      type: cmd
      cmd: "ghz -z 1m -q 10 -c 2 --insecure podinfo.test:9898"
```

`ghz`使用反射(reflection)来确定要调用哪种gRPC方法。如果你不希望为gRPC服务启用反射，你可以从grpc-proto library实现标准化的健康检查。要使用不带反射的健康检查架构，亦可以将参数传递给`ghz`：

```yaml
webhooks:
  - name: grpc-load-test-no-reflection
    url: http://flagger-loadtester.test/
    timeout: 5s
    metadata:
      type: cmd
      cmd: "ghz --insecure --proto=/tmp/ghz/health.proto --call=grpc.health.v1.Health/Check podinfo.test:9898"
```

<br>

负载测试器可以运行任何命令，只要容器镜像中存在二进制文件即可。例如，如果你想用其它命令替代`hey`，你可以创建自己的Dockerfile和镜像：

```Dockerfile
FROM weaveworks/flagger-loadtester:<VER>

RUN curl -Lo /usr/local/bin/my-cli https://github.com/user/repo/releases/download/ver/my-cli \
    && chmod +x /usr/local/bin/my-cli
```


<br>
<br>


### 负载测试委托

Load Testing Delegation

负载测试器同样可以转发测试任务到外部工具，目前支持nGrinder。


<br>
<br>


### 集成测试

Integration Testing

Flagger附带的测试服务，当配置为webhook是，可以运行helm测试、Bash测试或Concord测试。

```bash
# 部署一个helm测试器
helm repo add flagger https://flagger.app

helm upgrade -i flagger-helmtester flagger/loadtester \
--namespace=kube-system \
--set serviceAccountName=tiller

# When deployed the Helm tester API will be available at http://flagger-helmtester.kube-system/
```

现在，你可以向金丝雀分析规范中添加pre-rollout webhook：

```yaml
  analysis:
    webhooks:
      - name: "smoke test"
        type: pre-rollout
        url: http://flagger-helmtester.kube-system/
        timeout: 3m
        metadata:
          type: "helm"
          cmd: "test {{ .Release.Name }} --cleanup"
```

当金丝雀分析启动时，在路由流量到金丝雀之前，Flagger将调用pre-rollout webhook。如果helm测试失败，Flagger将重试直到到达分析阈值，并回滚金丝雀。

如果使用Helm v3，你必须在此命名空间创建专用的service account，并将命名空间添加到命令：

```yaml
  analysis:
    webhooks:
      - name: "smoke test"
        type: pre-rollout
        url: http://flagger-helmtester.kube-system/
        timeout: 3m
        metadata:
          type: "helmv3"
          cmd: "test {{ .Release.Name }} --timeout 3m -n {{ .Release.Namespace }}"
```

如果测试挂起或错误日志暗示权限不足，它可能与RBAC有关。

<br>

作为helm的替代方案，你可以使用Bash Automated Testing System来运行你的测试。

```yaml
  analysis:
    webhooks:
      - name: "acceptance tests"
        type: pre-rollout
        url: http://flagger-batstester.default/
        timeout: 5m
        metadata:
          type: "bash"
          cmd: "bats /tests/acceptance.bats"
```

使用Bats测试，你应该创建一个ConfigMap，并将其挂载到测试器容器内。

<br>

你同样可以配置测试器来启动Concord程序：

```yaml
  analysis:
    webhooks:
      - name: "concord integration test"
        type: pre-rollout
        url: http://flagger-concordtester.default/
        timeout: 60s
        metadata:
          type: "concord"
          org: "your-concord-org"
          project: "your-concord-project"
          repo: "your-concord-repo"
          entrypoint: "your-concord-entrypoint"
          apiKeyPath: "/tmp/concord-api-key"
          endpoint: "https://canary-endpoint/"
          pollInterval: "5"
          pollTimeout: "60"
```


<br>
<br>


### 手动门控

Manual Gating

对于手动批准(manual approval)金丝雀部署，你可以使用`confirm-rollout`和`confirm-promotion` webhooks。confirm rollout钩子在pre-rollout钩子之前执行。Flagger会中止金丝雀流量漂移并分析，直到confirm钩子返回HTTP 200。

对于手动回滚(manual rollback)金丝雀部署，你可以使用`rollback` webhook。此钩子将在分析和确认状态期间被调用。如果回滚钩子返回成功的HTTP状态码，Flagger将漂移所有流量回主实例(primary)并失败金丝雀。

使用Flagger测试器手动门控：

```yaml
# /gate/halt会返回http 403，因此会阻止推出
  analysis:
    webhooks:
      - name: "gate"
        type: confirm-rollout
        url: http://flagger-loadtester.test/gate/halt
```

<br>

如果你启用了通知，如果金丝雀推出等待批准，Flagger将会给你推送消息。

```yaml
# 启动金丝雀分析
  analysis:
    webhooks:
      - name: "gate"
        type: confirm-rollout
        url: http://flagger-loadtester.test/gate/approve
```

<br>

```yaml
# 手动门控可通过Flagger测试器API驱动
  analysis:
    webhooks:
      - name: "ask for confirmation"
        type: confirm-rollout
        url: http://flagger-loadtester.test/gate/check
```

<br>

默认情况下，门控是关闭的，你可以开启或恢复金丝雀推出：

```bash
kubectl -n test exec -it flagger-loadtester-xxxx-xxxx sh

curl -d '{"name": "podinfo","namespace":"test"}' http://localhost:8080/gate/open
```

<br>

你可以在任意时间暂停推出：

```bash
curl -d '{"name": "podinfo","namespace":"test"}' http://localhost:8080/gate/close
```

<br>

如果一个金丝雀分析暂停，状态将改变为等待：

```yaml
kubectl get canary/podinfo

NAME      STATUS        WEIGHT
podinfo   Waiting       0
```

<br>

`confirm-promotion`钩子类型可用于手动批准金丝雀提升。当提升暂停时，Flagger将继续运行指标检查和负载测试。

```yaml
  analysis:
    webhooks:
      - name: "promotion gate"
        type: confirm-promotion
        url: http://flagger-loadtester.test/gate/halt
```

<br>

`rollback`钩子类型可用于手动回滚金丝雀提升。与门控一样，通过将回滚URL设置为`/rollback/check`，回滚可以被Flagger测试器API驱动

```yaml
  analysis:
    webhooks:
      - name: "rollback"
        type: rollback
        url: http://flagger-loadtester.test/rollback/check
```

<br>

默认情况下，回滚是关闭的。

```bash
# 你可以回滚一个金丝雀推出
kubectl -n test exec -it flagger-loadtester-xxxx-xxxx sh
curl -d '{"name": "podinfo","namespace":"test"}' http://localhost:8080/rollback/open

# 你可以关闭回滚
curl -d '{"name": "podinfo","namespace":"test"}' http://localhost:8080/rollback/close
```

如果你启用了通知，如果一个金丝雀已经回滚，Flagger会发送通知消息。



<br>
<br>



## 告警

Alerting: <https://docs.flagger.app/usage/alerting>

Flagger可配置来将告警发送到各种聊天平台。你可以在全局配置，也可以在金丝雀层面配置。


<br>


### 全局配置

Global configuration

全局配置通知：

```bash
# Slack全局配置
helm upgrade -i flagger flagger/flagger \
--set slack.url=https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK \
--set slack.channel=general \
--set slack.user=flagger


# Microsoft Teams全局配置
helm upgrade -i flagger flagger/flagger \
--set msteams.url=https://outlook.office.com/webhook/YOUR/TEAMS/WEBHOOK
```


<br>
<br>


### 金丝雀层面配置

Canary configuration

全局配置通知不够细化，为了使告警更加灵活，可在金丝雀层面配置。

Slack栗子：

```yaml
apiVersion: flagger.app/v1beta1
kind: AlertProvider
metadata:
  name: on-call
  namespace: flagger
spec:
  type: slack
  channel: on-call-alerts
  username: flagger
  # webhook address (ignored if secretRef is specified)
  address: https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK
  # secret containing the webhook address (optional)
  secretRef:
    name: on-call-url
---
apiVersion: v1
kind: Secret
metadata:
  name: on-call-url
  namespace: flagger
data:
  address: <encoded-url>
```

金丝雀分析的告警列表：

```yaml
  analysis:
    alerts:
      - name: "on-call Slack"
        severity: error
        providerRef:
          name: on-call
          namespace: flagger
      - name: "qa Discord"
        severity: warn
        providerRef:
          name: qa-discord
      - name: "dev MS Teams"
        severity: info
        providerRef:
          name: dev-msteams
```


<br>
<br>


### AlertMnager

Prometheus AlertMnager

当金丝雀部署失败时，你可以使用AlertManger来触发告警：

```yaml
  - alert: canary_rollback
    expr: flagger_canary_status > 1
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "Canary failed"
      description: "Workload {{ $labels.name }} namespace {{ $labels.namespace }}"
```



<br>
<br>



## 监控

Monitoring: <https://docs.flagger.app/usage/monitoring>


<br>


### Grafana

Flagger配有针对金丝雀分析的Grafana仪表盘。

```bash
# Install Grafana with Helm
helm upgrade -i flagger-grafana flagger/grafana \
--set url=http://prometheus:9090
```


<br>
<br>


### Logging

金丝雀错误和延迟尖峰被记录为k8s事件，并被Flagger以json格式记录：

```
kubectl -n istio-system logs deployment/flagger --tail=100 | jq .msg

Starting canary deployment for podinfo.test
Advance podinfo.test canary weight 5
Advance podinfo.test canary weight 10
Advance podinfo.test canary weight 15
Advance podinfo.test canary weight 20
Advance podinfo.test canary weight 25
Advance podinfo.test canary weight 30
Advance podinfo.test canary weight 35
Halt podinfo.test advancement success rate 98.69% < 99%
Advance podinfo.test canary weight 40
Halt podinfo.test advancement request duration 1.515s > 500ms
Advance podinfo.test canary weight 45
Advance podinfo.test canary weight 50
Copying podinfo.test template spec to podinfo-primary.test
Halt podinfo-primary.test advancement waiting for rollout to finish: 1 old replicas are pending termination
Scaling down podinfo.test
Promotion completed! podinfo.test
```


<br>
<br>


### Event Webhook

Flagger可以配置来发送event payloads到特定webhook：

```bash
helm upgrade -i flagger flagger/flagger \
--set eventWebhook=https://example.com/flagger-canary-event-webhook
```

栗子：

```
{
  "name": "podinfo",
  "namespace": "default",
  "phase": "Progressing",
  "metadata": {
    "eventMessage": "New revision detected! Scaling up podinfo.default",
    "eventType": "Normal",
    "timestamp": "1578607635167"
  }
}
```

<br>

在金丝雀层面，event webhook可被覆盖：

```yaml
  analysis:
    webhooks:
      - name: "send to Slack"
        type: event
        url: http://event-recevier.notifications/slack
```


<br>
<br>


### Metrics

Flagger公开了可用于确定金丝雀分析状态和目标权重值的Prometheus metrics：

```yaml
# Flagger version and mesh provider gauge
flagger_info{version="0.10.0", mesh_provider="istio"} 1

# Canaries total gauge
flagger_canary_total{namespace="test"} 1

# Canary promotion last known status gauge
# 0 - running, 1 - successful, 2 - failed
flagger_canary_status{name="podinfo" namespace="test"} 1

# Canary traffic weight gauge
flagger_canary_weight{workload="podinfo-primary" namespace="test"} 95
flagger_canary_weight{workload="podinfo" namespace="test"} 5

# Seconds spent performing canary analysis histogram
flagger_canary_duration_seconds_bucket{name="podinfo",namespace="test",le="10"} 6
flagger_canary_duration_seconds_bucket{name="podinfo",namespace="test",le="+Inf"} 6
flagger_canary_duration_seconds_sum{name="podinfo",namespace="test"} 17.3561329
flagger_canary_duration_seconds_count{name="podinfo",namespace="test"} 6
```



<br>

---

<br>



# 教程

Tutorials


<br>


## Isito金丝雀部署

Istio Canary Deployments: <https://docs.flagger.app/tutorials/istio-progressive-delivery>



<br>
<br>



## Istio AB测试

Istio A/B Testing: <https://docs.flagger.app/tutorials/istio-ab-testing>



<br>
<br>



## Nginx 金丝雀部署

NGINX Canary Deployments: <https://docs.flagger.app/tutorials/nginx-progressive-delivery>



<br>
<br>



## 蓝绿部署

Blue/Green Deployments: <https://docs.flagger.app/tutorials/kubernetes-blue-green>



<br>
<br>



## 使用Prometheus Operator的金丝雀分析

Canary analysis with Prometheus Operator: <https://docs.flagger.app/tutorials/prometheus-operator>



<br>
<br>


## 零停机时间部署

Zero downtime deployments: <https://docs.flagger.app/tutorials/zero-downtime-deployments>

如果你想最大限度地减少滚动更新(rolling updates)和缩小(downscaling)带来的影响，当处理高流量生产环境时，有下面一些事情你应该考虑。


<br>


### 部署策略

Deployment strategy

在滚动更新期间限制不可用pod的数量：

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  progressDeadlineSeconds: 120
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
```

一个部署的默认的进度截止日期为十分钟。你应该考虑调整此值以使部署过程更快地失败。


<br>
<br>


### 存活度健康检查

Liveness health check

你的应用程序应该公开一个http endpoint，让k8s能够调用它来检测应用是否转换为无法恢复的断开状态，并且需要重新启动。

```yaml
livenessProbe:
  exec:
    command:
    - wget
    - --quiet
    - --tries=1
    - --timeout=4
    - --spider
    - http://localhost:8080/healthz
  timeoutSeconds: 5
  initialDelaySeconds: 5
```


<br>
<br>



### 准备度健康检查

Readiness health check

你的应用程序应该公开一个http endpoint，让k8s可以调用它来确认应用是否准备好接收流量。

```yaml
readinessProbe:
  exec:
    command:
    - wget
    - --quiet
    - --tries=1
    - --timeout=4
    - --spider
    - http://localhost:8080/readyz
  timeoutSeconds: 5
  initialDelaySeconds: 5
  periodSeconds: 5
```

如果你的应用依赖于外部服务，在允许k8s路由流量到应用实例之前，你应该检查服务是否可用。请记住， envoy sidecar可以比应用慢得多启动。这意味着在应用程序开始时，你应该重试至少几秒钟任意外部连接。


<br>
<br>


### 优雅地关闭

Graceful shutdown

在pod terminated(终止)之前，k8s发送`SIGTERM`信号到每个容器，并等待所有容器的一段时间(默认30s)来优雅地退出。如果你的应用不处理`SIGTERM`信号，或如果它不在宽限期内退出，k8s将杀死容器和任意正在处理的流量，这意味着你的应用处理将会失败。

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  template:
    spec:
      terminationGracePeriodSeconds: 60
      containers:
      - name: app
        lifecycle:
          preStop:
            exec:
              command:
              - sleep
              - "10"
```

你的应用应该具有延迟容器的关闭的`preStop`钩子。这将允许服务网格耗尽流量，并在应用不可用之前从所有其它envoy sidecar移除此pod。


<br>
<br>


### 推迟envoy关闭

Delay Envoy shutdown

即使你的应用对`SIGTERM`信号做出反应并尝试完全在关闭之前完成请求，并不意味着响应会将结果返回给调用者。如果envoy sidecar在你的应用之前关闭，则调用者会收到503错误。

要缓解此问题，你可以将`preStop`钩子添加到Istio代理，并等待主应用程序在envoy之前退出。

```yaml
#!/bin/bash
set -e
if ! pidof envoy &>/dev/null; then
  exit 0
fi

if ! pidof pilot-agent &>/dev/null; then
  exit 0
fi

while [ $(netstat -plunt | grep tcp | grep -v envoy | wc -l | xargs) -ne 0 ]; do
  sleep 1;
done

exit 0
```

你必须使用上面的脚本构建你自己的envoy docker image，并使用`preStop`指令修改Istio来注入webhook。


<br>
<br>


### 资源请求和限制

Resource requests and limits

如果你正在运行生产环境，则为所有工作负载设置CPU和MEM的请求和限制是必需的步骤。如果没有资源限制，由于CPU或MEM耗尽，你的节点可能会反应迟钝。如果没有资源请求，k8s scheduler将无法做出将pod调度到哪个节点的决定。

```yaml
apiVersion: apps/v1
kind: Deployment
spec:
  template:
    spec:
      containers:
      - name: app
        resources:
          limits:
            cpu: 1000m
            memory: 1Gi
          requests:
            cpu: 100m
            memory: 128Mi
```

请注意，如果没有资源请求(requests)，hpa无法确定何时扩展你的应用程序。换句话来说，hpa是根据requests来进行伸缩的，而不是limits。


<br>
<br>


### 自动伸缩

Autoscaling

生产环境应该能够处理突发流量(traffic burst)而不会影响服务质量(quality of service)。这可以通过k8s自动伸缩功能来实现。k8s的自动伸缩有亮哥维度：集群层面的节点伸缩和负载层面Pod数量自动伸缩。

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app
  minReplicas: 2
  maxReplicas: 4
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageValue: 900m
  - type: Resource
    resource:
      name: memory
      targetAverageValue: 768Mi
```


<br>
<br>


### ingress重试

Ingress retries

为了最大限度地减少缩小操作的影响，你可以利用Envoy retry功能。

```yaml
apiVersion: flagger.app/v1beta1
kind: Canary
spec:
  service:
    port: 9898
    gateways:
    - public-gateway.istio-system.svc.cluster.local
    hosts:
    - app.example.com
    retries:
      attempts: 10
      perTryTimeout: 5s
      retryOn: "gateway-error,connect-failure,refused-stream"
```

当hpa缩小应用副本数是，用户可能会获取到503错误。上面的配置将使Envoy重试由于网关错误而失败的http请求。



<br>
<br>



## 推出权重

Rollout Weights: <https://docs.flagger.app/tutorials/rollout-weights>


















































