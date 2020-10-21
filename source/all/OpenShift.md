---
title: OpenShift
date: 2019-03-26 17:50:11
mathjax: true
categories: DevOps
tags:
  - OpenShift
  - PaaS
  - DevOps
---


参考:

- Wikipedia
- OpenShift docs: <https://docs.openshift.com>

<br>

环境:

- RHELx86_64
- OpenShift v4.1




<br/>
<br/>

---

<!--more-->

<br/>
<br/>













# OpenShift是什么

[What is Red Hat OpenShift?](https://www.openshift.com/learn/what-is-openshift)


Red Hat OpenShift is a hybrid cloud(混合云), enterprise Kubernetes application platform.

![](/images/OpenShift/whatIsOpenShift.png)

<br>

功能和特点：

- **Container host and runtime**
它为k8s Master提供了企业级的Linux CoreOS，并为工作节点提供了企业级Linux支持。它支持标准的Docker和CRI-O runtime。

- **Enterprise Kubernetes**
它包含针对多个上游K8s版本的缺陷、安全和性能问题的修复程序。它采用了多种技术进行测试，是一个强大而紧密的集成平台，支持9年的生命周期。

- **Validated integrations**
它包含了软件定义的网络，并验证其它常见的网络解决方案。

- **Integrated container registry**
它附带了一个集成的私有容器注册中心。

- **Developer workflows**
它包含了简化的工作流程，以帮助团队更快地投入到生产。包括内置的Jenkins Pipeline和source-to-image，从应用程序代码直接到容器。它也可以扩展像Istio和Knative这样的新框架。

- **Easy access to services**
它通过嵌入式OperatorHub帮助管理员和支持团队，服务代理（包括直接访问AWS服务），经过验证的第三方解决方案和Kubernetes操作员。

















