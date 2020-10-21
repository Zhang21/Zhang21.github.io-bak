---
title: DevOps
date: 2019-02-13 05:22:15
categories: DevOps
tags:
  - 自动化运维
  - 运维开发
  - Auto DevOps
---


参考:

- 维基百科
- GitLab
- GitHub



<br/>
<br/>

---

<!--more-->

<br/>
<br/>




# 介绍


**DevOps**（Development和Operations的组合词）是一种重视 *软件开发人员（Dev）* 和 *IT运维技术人员（Ops）* 之间沟通合作的文化、运动或惯例。透过自动化 *软件交付* 和 *架构变更* 的流程，来使得构建、测试、发布软件能够更加地快捷、频繁和可靠。

![](/images/DevOps/DevOps.png)

![](/images/DevOps/devops-loop-and-spans-small.png)




<br/>
<br/>

---

<br/>
<br/>




# Auto DevOps


GitLab Auto DevOps:

- Auto Build
- Auto Test
- Auto Code Quality
- Auto SAST (Static Application Security Testing)
- Auto Dependency Scanning
- Auto License Management
- Auto Container Scanning
- Auto Review Apps
- Auto DAST (Dynamic Application Security Testing)
- Auto Deploy
- Auto Browser Performance Testing
- Auto Monitoring




<br/>
<br/>

---

<br/>
<br/>




# DevOps工具


下面介绍一些DevOps需要用到的工具，可能不够详细。


<br/>


## 基础环境

IaaS:

- VMware
- Xen
- KVM
- OpenStack
- 云平台
- ...



<br/>
<br/>



## 项目管理


Task:

- RedaMine
- Jira
- 禅道
- ...




<br/>
<br/>




## 代码


Code:

- git
- GitLab
- Gogs
- svn
- 云平台
- ...



<br/>
<br/>



## 持续集成/发布


CI/CD:

- Jenkins
- Jenkins X
- GitLab CICD
- Bamboo
- Maven
- 云平台
- ...



<br/>
<br/>



## 容器


Container:

- Docker
- K8s
- CoreOS
- Mesos
- Helm
- 云平台
- ...



<br/>
<br/>



## 测试


Test:

- **Selenium**
- **Katalon Studio**
- **Watir**
- **Jmeter**
- **Loadrunner**
- **LOCUST**


<br/>


### Selenium


- Website: <https://www.seleniumhq.org/>


Selenium是一个用于自动化测试Web apps的可移植框架。 Selenium提供了一种用于创作功能测试的回放工具，无需学习测试脚本语言。



<br/>
<br/>



### Katalon Studio


- Wetsite: <https://www.katalon.com/>


Simplify API, Web, Mobile Automation Tests.



<br/>
<br/>



### Watir


- Website: <http://watir.com/>

An open source Ruby library for automating tests. 
Watir interacts with a browser the same way people do: clicking links, filling out forms and validating text.



<br/>
<br/>



### JMeter


Apache JMeter应用程序是开源软件，纯Java应用程序，旨在加载测试功能行为和测量性能。它最初是为测试Web应用程序而设计的，但后来扩展到其他测试功能。

Apache JMeter可用于测试静态和动态资源，Web动态应用程序的性能。
它可用于模拟服务器，服务器组，网络或对象上的重负载，以测试其强度或分析不同负载类型下的整体性能。

<br>

Apache JMeter功能包括:

- Ability to load and performance test many different applications/server/protocol types
  - Web - HTTP, HTTPS (Java, NodeJS, PHP, ASP.NET, …)
  - SOAP / REST Webservices
  - FTP
  - Database via JDBC
  - LDAP
  - Message-oriented middleware (MOM) via JMS
  - Mail - SMTP(S), POP3(S) and IMAP(S)
  - Native commands or shell scripts
  - TCP
  - Java Objects
- Full featured Test IDE that allows fast Test Plan recording
- CLI mode to load test from any Java compatible OS
- Highly Extensible core
- ...



<br/>
<br/>



### LoadRunner


- Website: <https://www.microfocus.com>

LoadRunner is a Load Testing Software



<br/>
<br/>



### LOCUST

- Website: <https://locust.io/>
- GitHub: <https://github.com/locustio/locust/>

<br>

An open source load testing tool.

Define user behaviour with Python code, and swarm your system with millions of simultaneous users.




<br/>
<br/>
<br/>




## 质量与安全


Quality and Security:

- infer
- SonarQube
- Cuckoo Sandbox
- OWASP ZAProxy
- Mobile-Security-Framework-MobSF
- Clair


<br/>


### Infer


- GitHub: <https://github.com/facebook/infer>
- Website: <https://fbinfer.com/>

<br>

**Infer** 是一个 `Java`，`C ++`，`Objective-C` 和 `C` 的代码静态分析工具。它会产生一个潜在的bug列表。任何人都可以使用Infer在发送给用户之前拦截关键错误，并帮助防止崩溃或性能不佳。

infer 主要用于 APP 端，也就是 Android/IOS App。




<br/>
<br/>



### SonarQube


- GitHub: <https://github.com/SonarSource/sonarqube>
- Website: <https://www.sonarqube.org/>

<br>

**SonarQube** 是一个开源平台，通过代码的自动化静态分析不断的检查代码质量。 SonarQube 支持20多种语言的分析，并在各种类型的项目中输出和存储问题。通过不同的插件对这些结果进行再加工处理，通过量化的方式度量代码质量的变化，从而可以方便地对不同规模和种类的工程进行代码质量管理。



<br/>
<br/>



### MobSF


- GitHub: <https://github.com/MobSF/Mobile-Security-Framework-MobSF>

<br>

Mobile Security Framework is an automated, all-in-one mobile application (Android/iOS/Windows) pen-testing framework capable of performing static analysis, dynamic analysis, malware analysis and web API testing.



<br/>
<br/>



### Clair

- GitHub: <https://github.com/coreos/clair>

Vulnerability Static Analysis for Containers.
Clair is an open source project for the static analysis of vulnerabilities in application containers (currently including appc and docker).




<br/>
<br/>
<br/>




## 配置管理


Configuration Management:

- Ansible
- ZooKeeper
- CFEngine
- Chef
- MAAS
- Puppet
- SaltStack
- Vagrant
- Rundeck
- Rudder
- 云平台
- ...



<br/>
<br/>
<br/>



## 数据分析

Data Analysis:

- Hadoop
- Ambari
- Avro
- Flume
- HBase
- Hive
- Spark
- Sqoop
- ZooKeeper




<br/>
<br/>
<br/>




## 日志


Log:

- ElasticStack
  - Elasticsearch
  - Logstash
  - Beat
- Hadoop, Hive - 与ELK类似的方案
- Flume
- Fluentd
- Splunk
- Kafka
- Loggly
- Papertrail
- 云平台
- ...



<br/>
<br/>



## 流


Stream:

- Kafka
- Apex
- Flink
- Heron
- Spark
- Heka



<br/>
<br/>



## Api网关


Api Gateway:

- Gloo
- Ambassador
- Spring Cloud
- Kong
- Netflix Zuul
- 云平台
- ...



<br/>
<br/>



## 性能


Performance:

- NetData
- Pinpoint
- Datadog
- AppDynamics
- Apache JMeter
- ab(ApacheBench)
- Gatling




<br/>
<br/>



## 监控


Monitoring:

- Zabbix
- Nagios
- Prometheus
- Grafana
- Netdata
- Graphite
- Cacti
- Glances
- Collectd
- Ganglia
- Kibana
- Sensu




<br/>
<br/>


## 备份


Backup:

- 全量
- 增量








<br/>
<br/>

---

<br/>
<br/>



# 灰度发布

> ps:
> 参考百度百科!

<br>

灰度发布（金丝雀发布）是指在黑与白之间，能够平滑过渡的一种发布方式。在其上可以进行A/B testing，即让一部分用户继续用产品特性A，一部分用户开始用产品特性B，如果用户对B没有什么反对意见，那么逐步扩大范围，把所有用户都迁移到B上面来。
灰度发布可以保证整体系统的稳定，在初始灰度的时候就可以发现、调整问题，以保证其影响度。

灰度期：灰度发布开始到结束期间的这一段时间，称为灰度期。








