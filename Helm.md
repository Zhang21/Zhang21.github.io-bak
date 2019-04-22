---
title: Helm
date: 2018-12-25 09:11:22
categories: DevOps
tags:
  - Helm
  - K8s
---

环境：

- EL7x86_64

参考：

- HELM文档: <https://docs.helm.sh/>




<br/>
<br/>

<!--more-->

---

<br/>
<br/>



# 概述

**Helm**是Kubernetes生态系统中的一个软件包管理工具，主要用来管理**Charts**，有点类似于Ubuntu中的`apt`或CentOS中的`yum`。由go编写，是Deis公司发起的一个开源工具，有助于简化部署和管理Kubernetes应用。
在Kubernetes中，应用管理是需求最多、挑战最大的领域。Helm项目提供了一个统一软件打包方式，支持版本控制，可以大大简化Kubernetes应用分发与部署中的复杂性。

Helm Chart是用来封装 Kubernetes 原生应用程序的一系列 YAML 文件。可以在你部署应用的时候自定义应用程序的一些 Metadata，以便于应用程序的分发。
对于应用发布者而言，可以通过 Helm 打包应用、管理应用依赖关系、管理应用版本并发布应用到软件仓库。
对于使用者而言，使用 Helm 后不用需要编写复杂的应用部署文件，可以以简单的方式在 Kubernetes 上查找、安装、升级、回滚、卸载应用程序。

<br>

![](/images/Helm/Helm_Logo.png)













