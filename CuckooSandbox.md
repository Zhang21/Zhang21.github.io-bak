---
title: CuckooSandbox
date: 2019-04-16 15:25:45
categories: DevOps
tags:
  - Cuckoo
  - Sandbox
  - Security
  - Test
  - DynamicAnalysis
  - MalwareAnalysis
---


参考:
- github: <https://github.com/cuckoosandbox/cuckoo>
- docs: <https://cuckoo.sh/docs/>

<br>

环境:
- REL7x86_64
- Cuckoo v2.0.6








<br/>
<br/>

---

<!--more-->

<br/>
<br/>




















# 介绍

[Introduction](https://cuckoo.sh/docs/introduction/index.html)


本章节介绍 Cuckoo Sandbox。它解释了一些基本的恶意软件(malware)分析概念，什么是CuckooSanbox以及它如何适应恶意软件分析。


<br/>
<br/>


## 沙箱

[Sandboxing](https://cuckoo.sh/docs/introduction/sandboxing.html)


如维基百科所定义，“在计算机安全中，沙箱是一种用于分离正在运行的程序的安全机制。它通常用于执行未经验证的代码，或来自未经验证的第三方、供应商、不受信任的用户或网站或程序。”

这个概念也适用于恶意软件分析的沙箱：我们的目标是在隔离的环境中运行位置和不受信任的应用程序或文件，并获取有关它的功能的信息。

恶意软件沙箱是动态分析方法的实际应用：它不是静态分析二进制文件，而是实事执行和监视。

这种方法显然有利有弊，但它获取有关恶意软件的其它详细信息的有价值的技术(如网络行为)。因此，在检查恶意软件时执行**静态分析**(static)和**动态分析**(dynamic analysis)是一种很好的做法，以便更深入地了解它。

简单来说，Cuckoo是一个允许你执行沙箱恶意软件分析的工具。


<br/>
<br/>


### 使用沙箱

在考虑安装、配置和使用Cuckoo之前，你应该花时间考虑一下你希望用它实现什么功能，以及如何实现。

你应该考虑的一些问题:
- 我想分析那种文件？
- 我希望能够处理多少分析？
- 我想用哪个平台来运行我的分析？
- 我想要关于文件的哪些信息？

隔离环境(如虚拟机)的创建时沙箱部署中最关键和最重要的部分：应该仔细进行并进行适当的规划。

<br>

在掌握你选择的虚拟化产品之前，你应该已经有一个设计的计划:
- 使用哪种操作系统(os)、语言(language)和修补(patching)级别；
- 要安装哪个软件和版本。

<br>

考虑到自动恶意软件(malware)分析不是确定性的，它的成功可能取决于很多因素：你试图在虚拟化系统中运行恶意软件，就像在本机系统上运行一样，这可能很难实现，并且可能并不总是如此成功。你的目标应该是创建一个能够满足你所有要求的系统，并尽可能使其尽可能真实。

例如，你可以考虑留下一些正常使用的故意的痕迹(如历史记录、Cookie、文档、图像...)。如果恶意软件旨在操作、操纵、窃取此类文件，你将能够注意到它。

虚拟化操作系统通常带有很多痕迹，使它们容易被检测到。即使你不应高估此问题，也可能需要处理此问题并尝试因此尽可能多的虚拟化跟踪。互联网上有很多关于虚拟化检测技术和对策的文献。

完成设计和准备所需系统原型后，你可以继续创建并部署它。你将总是及时改变或略微修复它们，但要记住，一开始的良好规划意味着从长远来看减少麻烦。




<br/>
<br/>
<br/>




## Cuckoo是什么

[What is Cuckoo](https://cuckoo.sh/docs/introduction/what.html)


Cuckoo是一个开源的自动恶意软件分析系统。
它用于自动运行和分析文件，并收集全面的分析结果，概述恶意软件在隔离操作系统内运行时的作用。


<br/>
<br/>


### 用例

Use Cases

由于其机极为模块化的设计，Cuckoo既可以作为独立应用程序使用，也可以集成到更大的框架中。

它可用来分析:
- 通用Windows EXE可执行文件
- DLL文件
- PDF文档
- MS OFFICE文档
- URLs和HTML文件
- PHP脚本
- CPL文件
- VB脚本
- ZIP文件
- Python文件
- Almost anything else

由于其模块化和强大的脚本功能，使用Cuckoo可实现的目标没有限制。



<br/>
<br/>



### 架构

Architecture

Cuckoo Sandbox由一个处理样本执行(Execution)和分析(Analysis)的中央管理软件组成。

每个分析都在一个新的和隔离的虚拟(物理)机器中启动。Cuckoo架构的主要组件是**Host machine**(管理软件)和许多**Guest machines**(用于分析的虚拟机或物理机)。
Host运行整个沙箱的分析进程的核心组件，而Guest是实际执行和分析恶意软件样本的隔离环境。

<br>

Cuckoo的主要架构图:

![](/images/Cuckoo/architecture-main.png)



<br/>
<br/>



### 获取Cuckoo

Obtaining Cuckoo

虽然可以从官网上下载Cuckoo，也可从github下载，但还是建议使用`pip`安装。




<br/>
<br/>
<br/>




## 许可证

[License](https://cuckoo.sh/docs/introduction/license.html)


Cuckoo Foundation是一家非盈利组织，在荷兰成立，主要致力于支持开源的恶意软件分析系统Cuckoo Sandbox以及周边项目和计划的开发和发展。

该基金会致力于为软件项目提供财务和基础设施支持，并协调社区的发展和贡献。




<br/>
<br/>
<br/>




## 社区准则

[Community guidelines](https://cuckoo.sh/docs/introduction/community.html)


Cuckoo Sandbox是一个开源项目，我们感谢任何形式的贡献。这些指南旨在帮助你和我们尽快回答问题、解决问题和合并代码。所以，你正在阅读的这些指南是很棒的！


<br/>
<br/>


### 介绍

这些指南有:
- 创建Issue要包含的内容
  - Reporting bugs/errors/unexpected behavior
  - Feature suggestions/requests
- Contributing code/documentation











<br/>
<br/>

---

<br/>
<br/>



















# 安装

[Installation](https://cuckoo.sh/docs/installation/index.html)














































