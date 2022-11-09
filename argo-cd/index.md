# Argo-CD文档


参考:

- [argo-cd官方文档](https://argo-cd.readthedocs.io/en/stable/)




<br/>

---

<br/>




# 概述


<br/>


## argo-cd是什么



argo-cd是一个用于k8s的声明式、gitops持续交付(continuous delivery)工具。

argo-cd配置存在在k8s资源中，application和project是CRDs，cluster credentials存储为secret或configmap。不需要PVC/PV/VOLUME。

![argo-cd](/images/ArgoCD/argocd-ui.gif)


<br/>
<br/>


## 为什么是argo-cd

Why Argo CD

应用程序定义、配置和环境应该是声明式和版本控制的。应用程序的部署和生命周期管理应该自动化、可审核和易于理解。


<br/>
<br/>


## 入门

Getting Started

快速开始:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```


<br/>
<br/>


## 如何运作

How it works

argo-cd遵循gitops模式，使用git仓库作为定义期望的应用程序状态的真实来源。

k8s manifests可通过以下方式指定:

- kustomize
- helm chart
- jsonnet
- k8s yaml
- 任一自定义配置管理工具

<br/>

argo-cd在指定的目标环境中自动部署所需的应用程序状态。应用程序部署可以跟踪branch、tag或git commit固定到特定版本的清单。


<br/>
<br/>


## 架构

Architecture

[架构](/images/ArgoCD/argocd_architecture.png)

argocd被实现为一个k8s controller，它持续监视正在运行的应用程序并将当前的活动状态于所需的目标状态进行比较。实时状态偏离目标状态的已部署应用程序会被视为`OutOfSync`。argocd报告和可视化差异，同时提供自动或手动将实时状态同步到期望的目标状态。任何对git仓库中对期望状态所做的修改都可以自动应用并反映在指定的目标环境中。


<br/>
<br/>


## 功能

Features

- 将应用程序自动部署到指定的目标环境
- 支持多种配置管理工具/模板工具
- 能够管理和部署到多个集群
- SSO集成
- 用于授权的多租户和RBAC策略
- git仓库中对任一应用程序配置的提交，回滚或滚动
- 应用程序资源的健康状态分析
- 自动配置漂移检测和可视化
- 自动或手动同步应用到期望状态
- Web界面提供了应用程序活动的实时试图
- CLI用于自动化和CI集成
- Webhook集成
- Access Token
- 支持复杂应用程序推出的钩子：PreSync, Sync, PostSync
- 用于应用程序事件和API调用的审计跟踪
- Prometheus指标
- 参数可覆盖helm参数




<br/>

---

<br/>




# 基础知识

Basics

在使用argo-cd之前，你应该了解以下基础知识:

- git
- docker
- k8s
- helm/kustomize
- ci
- cd
- gitops
- ...




<br/>

---

<br/>




# 核心概念

Core Concepts

以下是argo-cd的一些概念：

- Application(应用): 由清单定义的一组k8s资源，CRD。
- Application source type(应用资源类型): 用于构建应用的工具。
- Target state(目标状态): 应用的期望状态，由git仓库中的文件来表示。
- Live state(实时状态): 应用的实时状态。
- Sync status(同步状态): 实时状态是否于目标状态相匹配。
- Sync(同步): 将应用转移到目标状态的过程。
- Sync operation status(同步操作状态): 同步是否成功。
- Refresh(刷新): 将git仓库中的最新代码与实时状态进行比较，弄清楚有什么不同。
- Health(健康状态): 应用的健康状态，是否正常运行？是否可以服务请求？




<br/>

---

<br/>




# 入门

Getting Started


<br/>


## 安装

> **注意**<br>
> 由于默认安装的argocd服务都没有资源限制，因此建议添加上资源限制。

<br/>

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

argo-cd的几个服务:

- argcd-server: 中枢程序
- argocd-repo-server: git仓库的本地缓存
- argocd-applicationset-controller: 一个k8s控制器，用于比较原版本和目标版本
- argocd-dex-server: SSO服务
- argocd-redis: 一次性缓存
- argocd-notifications-controller： 发送通知


<br/>
<br/>


## 下载argo-cd命令行

下载地址: <https://github.com/argoproj/argo-cd/releases/latest>


<br/>
<br/>


## 访问argo-cd的api地址

默认安装时，argo-cd并未提供外部访问地址。因此，可根据需要创建外部访问地址:

- ingress
- nodePort
- port forwarding

ingress如果未配置证书，会出现无线重定向问题。要么就配置上证书，要么就关闭argocd-server的安全选项，在启动参数添加`-insecure`。


<br/>
<br/>


## 使用命令行登录

默认`admin`用户的密码存在名为`argocd-initial-admin-secret`的密钥中。

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# 输入用户和密码进行登录
argocd login <ARGOCD_SERVER>
```

请注意，如果你修改了密码，请从此命名空间中删除`argocd-initial-admin-secret`这个密钥。


<br/>
<br/>


## 注册一个集群到部署

Register A Cluster To Deploy Apps To (Optional)

此步骤将集群的凭证注册到argo-cd中，并且至于在部署到外部集群时才有必要。当部署到内部集群（也就是argo-cd正在运行的同一个集群），`https://kubernetes.default.svc`将作为应用程序的k8s api的地址。

```bash
# 添加外部集群

kubectl config get-contexts -o name

# 安装一个serviceaccount(argocd-manager)到kube-system命名空间中
# 并绑定此服务账号到ClusterRole管理员级别
argocd cluster add xxx
```


<br/>
<br/>


## 从git仓库创建一个应用

Create An Application From A Git Repository

如从示例<https://github.com/argoproj/argocd-example-apps.git>来演示argo-cd是如何工作的。

<br/>

如果从git私有仓库拉取，需要配置私钥和unknown host。


<br/>


### 从命令行创建应用

Creating Apps Via CLI

```bash
argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
```


<br/>


### 从界面创建应用

登录argo-cd界面，创建应用，写入具体配置信息。


<br/>
<br/>


## 同步应用

Sync (Deploy) The Application

默认三分钟从仓库同步一次。


<br/>


### 通过命令行同步

Syncing via CLI

```bash
# 查看应用状态
argocd app get guestbook

# 同步状态
argocd app sync guestbook
```

之后可通过界面查看相关信息。




<br/>

---

<br/>




# 运维手册

Operator Manual























<br/>

---

<br/>











# 用户指南

User Guide













<br/>


## 最佳实践

Best Practices


<br/>


### 分离配置和源代码仓库

Separating Config Vs. Source Code Repositories

由于以下原因，强烈建议使用单独的git仓库来保存你的配置(k8s yaml, helm chart等)，并将配置与源代码分开：

- 提供了应用程序与应用程序配置的干净分离。有时候，你希望修改配置清单而不触发整个CI构建。
- 更干净的审计日志。处于审计目的，仅保留配置的仓库将具有更干净的git历史记录。
- 你的应用程序可能由多个git仓库的服务组成，但仅被部署为一个单元。将配置清单存储在单个组件的一个源代码仓库中可能没有意义。
- 访问分离。开发人员不一定是可以/应该有意/无意地推向生产环境的人。通过单独的仓库，可以控制不同的权限。
- 对于CI流水线来说也更细化和好控制。


<br/>


### Leaving Room For Imperativeness

由于某些不完善或自动化，可能希望离开房间(leave room)，并且不要在git仓库中定义所有内容。例如，如果想通过hpa来管理副本数量，那你肯定不希望在git中追踪`replica`。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  # do not include replicas in the manifests if you want replicas to be controlled by HPA
  # replicas: 1
  template:
    spec:
      containers:
      - image: nginx:1.7.9
        name: nginx
        ports:
        - containerPort: 80
...
```


<br/>


### 确保git修订中的配置清单是真正不变的

Ensuring Manifests At Git Revisions Are Truly Immutable

当使用helm或kustomize等模板工具时，表明可以配置清单将其含义从一天更改为另一天。这通常是由上游helm仓库或kustomize base的更改引起的。

如下面这个kustomization.yaml，默认指向远程仓库的HEAD revision。但这并不是一个稳定的目标，此仓库可能会突然发生变化。

```yaml
bases:
- github.com/argoproj/argo-cd//manifests/cluster-install
```

更好的版本是使用git tag或commit sha。如:

```yaml
bases:
- github.com/argoproj/argo-cd//manifests/cluster-install?ref=v0.11.1
```







































