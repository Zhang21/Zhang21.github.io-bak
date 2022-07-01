# RKE


参考:

- [RKE文档](https://docs.rancher.cn/docs/rke/_index)

<br/>

版本:

- rke1




<br/>

---

<br/>



# 介绍

Rancher Kubernetes Engine，简称 RKE，是一个经过 CNCF 认证的 Kubernetes 安装程序。




<br/>
<br/>


# 要求

RKE对操作系统、软件、端口、SSH和硬件等的要求。

<br/>

**总体要求**：

- SSH用户：使用SSH访问节点的用户必须是节点`docker`用户组成员。如`usermod -aG docker <user_name>`
- 禁用swap
- 修改sysctl配置: `net.bridge.bridge-nf-call-iptables=1`
- 内核版本
- k8s和docker版本
- 查看和选择网络插件
  - Calico
  - Flannel
  - Weave

<br/>

**软件要求**：

- OpenSSH
- Kubernetes：参考rke版本支持的k8s版本
- Docker：请注意k8s支持的docker版本

<br/>

**硬件要求**：

- 测试环境plane(etcd, controplane)节点和worker节点可以复用
- 正式环境建议分开部署
- 硬件至少4c8g起步

<br/>


**端口要求**:

- 节点之间需要放开6443端口

<br/>

**SSH Server配置**

```
# /etc/ssh/sshd_config
# 允许TCP转发
AllowTcpForwarding yes
```



<br/>

---

<br/>




# 安装介绍

<br/>

## 下载二进制


<br/>
<br/>


## 为k8s集群准备节点


<br/>
<br/>


## 创建集群配置文件

RKE使用集群配置文件`cluster.yml`来规划集群中的节点。如集群应该包含哪些节点，节点角色，如何部署k8s等等。

```bash
# 创建配置我呢见
rke config --name cluster.yml
```

<br/>
<br/>


### 高可用集群

可在配置中配置多个**controlplane**节点来启用rke的高可用集群。RKE会把master节点的组件部署在所有的controlplane节点上（一般是三个），同时把kubelets的默认连接地址配置为`127.0.0.1:6443`，这个地址是`nginx-proxy`请求的master节点的地址。

```
镜像: rancher/rke-tools
容器: nginx-proxy
它通过nginx本地6443端口代理到controlplane(k8s master)节点上的多个apiserver地址
```


<br/>
<br/>


### 证书

默认情况下，Kubernetes 集群需要用到证书，而 RKE 会自动为所有集群组件生成证书。


<br/>
<br/>


## 部署k8s集群

在配置文件`cluster.yml`文件目录下，运行`rke up`命令部署集群。

集群部署成功后，会生成名称为`kube_config_cluster.yml`的`kubeconfig`集群控制文件。


<br/>
<br/>


## 保存文件

> 注意<br>
> 请保存下面列出来的重要文件，这些文件用于维护集群，排查问题和升级集群。

<br/>

请将这些文件复制并保存到安全的地方：

- `cluster.yml`：RKE集群的配置文件。
- `kube_config_cluster.yml`：k8s集群所有权限的认证凭据。
- `cluster.rkestate`：k8s集群状态文件，包含了获取该集群所有权限的认证凭据。

k8s集群状态文件用配置文件`cluster.yml`以及集群中的组件证书组成。


<br/>
<br/>


## 操作集群

可以将`kube_config_cluster.yml`复制到`~/.kube/config`里，然后用kubectl操作集群。

通过命令行参数指定也可以：

```bash
kubectl --kubeconfig kube_config_cluster.yml version
```




<br/>

---

<br/>




# 自定义证书

[自定义证书](https://docs.rancher.cn/docs/rke/installation/certs/_index)

<br/>

默认情况下，Kubernetes 集群需要用到证书，而 RKE 会自动为所有集群组件生成证书。您也可以使用自定义证书。




<br/>

---

<br/>




# 升级指南

使用 RKE 部署 Kubernetes 后，您可以升级 Kubernetes 集群中组件的版本、编辑Kubernetes services 列表或编辑插件。




<br/>

---

<br/>


# 备份和恢复指南

RKE 集群可以自动备份 etcd 节点的快照。在灾难场景下，您可以使用这些快照恢复集群。RKE 将快照保存本地`/opt/rke/etcd-snapshots`路径下。

<br/>

## 创建一次性快照

一些更详细的参数请看[rke etcd snapshot-save的可配置参数](https://docs.rancher.cn/docs/rke/etcd-snapshots/one-time-snapshots/_index#rke-etcd-snapshot-save-%E5%91%BD%E4%BB%A4%E7%9A%84%E5%8F%AF%E9%85%8D%E7%BD%AE%E5%8F%82%E6%95%B0)

运行下面命令时，RKE会创建一个用于备份快照的容器。完成备份后，RKE会删除该容器。

```bash
# 创建了一个快照，保存在/opt/rke/etcd-snapshots路径下
# 不指定--config，则默认使用cluster.yml文件
rke etcd snapshot-save --config cluster.yml --name snapshot-name
```


<br/>
<br/>


## 创建定时快照

[Etcd-Snapshot 服务的可配置参数](https://docs.rancher.cn/docs/rke/etcd-snapshots/recurring-snapshots/_index#etcd-snapshot-%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%8F%AF%E9%85%8D%E7%BD%AE%E5%8F%82%E6%95%B0)

RKE默认是每12小时自动备份etcd节点的快照。

运行已经启用`etcd-snapshot`的集群时，您可以在命令行工具中输入`docker logs etcd-rolling-snapshots`，查看`etcd-rolling-snapshots`日志，确认集群是否已开启定时快照功能。


<br/>
<br/>


## 恢复集群

如果你的k8s集群发生了灾难，你可以使用`rke etcd snapshot-restore`来恢复您的etcd。这个命令可以将 etcd 恢复到特定的快照，应该在遭受灾难的特定集群的 etcd 节点上运行。

<br>

> **警告**:
> 在运行`rke etcd snapshot-restore`之前，您应该备份集群中的任何重要数据，因为该命令会删除你当前的kubernetes集群，并用新的集群替换。


<br/>


### 从本地快照恢复的示例

运行以下命令，从本地快照中还原etcd；

```bash
rke etcd snapshot-restore --config cluster.yml --name mysnapshot
```


<br/>
<br/>


## 示例

主要分为以下步骤：

- 备份集群
- 模拟节点failure
- 新建etcd节点
- 使用备份恢复新建节点的数据
- 确认恢复后的集群处于正常状态

```
# 备份
rke etcd snapshot-save --name snapshot.db --config cluster.yml

# 关闭node2节点，让它不可用

# 新建etcd节点
nodes:
  - address: 10.0.0.1
    hostname_override: node1
    user: ubuntu
    role:
      - controlplane
      - worker
  #    - address: 10.0.0.2
  #      hostname_override: node2
  #      user: ubuntu
  #      role:
  #       - etcd
  - address: 10.0.0.3
    hostname_override: node3
    user: ubuntu
    role:
      - etcd

# 使用备份恢复新建节点的数据
# 先决条件：开始恢复节点前，请确保您的cluster.rkestate文件有效，因为该文件包含了集群所需的证书数据。
rke etcd snapshot-restore --name snapshot.db --config cluster.yml

# 确认集群状态
kubectl get pod
```



<br/>

---

<br/>



# 证书管理

证书是 Kubernetes 集群的重要组成部分，所有的 Kubernetes 组件都需要用到证书。您可以使用RKE的`rke cert`命令管理证书。




<br/>

---

<br/>




# 节点管理


<br/>


## 添加节点

修改`cluster.yml`文件内容，添加额外的节点，并指定它们在k8s集群中的角色。

```bash
# 使用--update-only添加或删除 worker 节点时，可能会触发插件或其他组件的重新部署或更新。
rke up --update-only
```




<br/>

---

<br/>




# k8s配置项

编辑RKE的cluster.yml文件时，您可以在文件中配置多种不同的选项，控制 RKE 如何启动 Kubernetes。


<br/>


## 集群选项

```yaml
# 集群名称
# 默认为 local
cluster_name: mycluster

# 检查docker版本
# 默认情况下，RKE 会检查所有主机上已安装的 Docker 的版本号，如果 Kubernetes 不支持该版本的 Docker，会导致 RKE 运行失败并且报错。
# 默认为 false
ignore_docker_version: false

# k8s版本
# 可通过rke config --list-version -all
# RKE 支持升级，但目前不支持回滚 Kubernetes 版本
kubernetes_version: v1.1x-xxx

# 前缀路径
# 默认路径前缀: /opt/rke
prefix_path: /opt/custom_path

# 集群层面的SSH密钥路径
# 如果在集群级和节点级都定义了 ssh 密钥路径，那么 RKE 会优先使用节点层级的密钥。
ssh_key_path: ~/.ssh/id_rsa

# SSH Agent
# 默认false
ssh_agent_auth: false

# 插件job连接超时
# 默认30s
```


<br/>
<br/>


## 节点选项

RKE 用来指定集群节点、用于访问节点的 ssh 凭证以及这些节点在 Kubernetes 集群中的角色。


<br/>


## 角色

k8s角色:

- controlplane
- etcd
- worker

<br/>

etcd保存着集群的状态，是最重要的组件，是集群的单一真相来源。etcd节点的污点如下：

| Taint Key | Taint Value | Taint Effect |
| - | - | - |
| `node-role.kubernetes.io/etcd` | `true` | `NoExecute` |

<br/>

Controlplane，k8s master的apiserver, scheduler, controller。controlplane节点的污点如下：

| Taint Key | Taint Value | Taint Effect |
| - | - | - |
| `node-role.kubernetes.io/controlplane` | `true` | `NoSchedule` |

<br/>

Worker， 任何Pod都会落在这些节点上。


<br/>
<br/>


## 节点选项

- `address`：用于设置主机名或IP地址，RKE必须能够连接到此地址。
- `internal_address`：提供了让具有多个地址的节点设置一个特定的地址用于私有网络上的主机间通信的能力。没有设置，则使用`address`进行主机键通信。
- `hostname_override`：能够提供一个友好的名称。没有设置，则使用`address`值。
- `role`：k8s角色
- `port`：ssh端口，默认22
- `user`：ssh用户，连接到节点的用户，必须是docker组成员。
- `ssh_key_path`：用于连接到节点要使用的私钥，默认是`~/.ssh/id_rsa`。
- `ssh_key`：可以不设置SSH密钥的路径，而是指定实际的密钥内容。
- `ssh_cert_path`：用于连接到这个节点时要使用的签名 SSH 证书路径
- `ssh_cert`：SSH证书内容
- `docker_socket`：docker套接字
- `labels`： 为节点添加标签。
- `taints`： 为节点添加污点。


<br/>
<br/>


## 节点配置示例

```yaml
# cluster.yml
# 节点配置示例
nodes:
  - address: 1.1.1.1
    user: ubuntu
    role:
      - controlplane
      - etcd
    ssh_key_path: /home/user/.ssh/id_rsa
    port: 2222
  - address: 2.2.2.2
    user: ubuntu
    role:
      - worker
    ssh_key: |-
      -----BEGIN RSA PRIVATE KEY-----

      -----END RSA PRIVATE KEY-----
  - address: 3.3.3.3
    user: ubuntu
    role:
      - worker
    ssh_key_path: /home/user/.ssh/id_rsa
    ssh_cert_path: /home/user/.ssh/id_rsa-cert.pub
  - address: 4.4.4.4
    user: ubuntu
    role:
      - worker
    ssh_key_path: /home/user/.ssh/id_rsa
    ssh_cert: |-
      ssh-rsa-cert-v01@openssh.com AAAAHHNza...
    taints: # Available as of v0.3.0
      - key: test-key
        value: test-value
        effect: NoSchedule
  - address: example.com
    user: ubuntu
    role:
      - worker
    hostname_override: node3
    internal_address: 192.168.1.6
    labels:
      app: ingress
```


<br/>
<br/>


## 私有镜像仓库

RKE支持在`cluster.yml`中配置多个私有镜像仓库，然后从私有镜像仓库拉取镜像。

```yaml
# 不需要加上https
private_registries:
  - url: registry.com
    user: Username
    password: password
  - url: myregistry.com
    user: myuser
    password: mypassword
```



<br/>
<br/>



## 系统镜像

RKE在部署k8s时，会从镜像仓库中拉取k8s系统组件镜像。

从`v0.1.6`开始，多个系统镜像的功能被整合到一个`rancher/rke-tools`镜像中，以简化和加快部署过程。镜像很多，所以可以提前在每个节点上先把镜像拉取下来。

你可以配置网络插件、ingress controller和dns provider这些附加组件和选项。

<br/>

Kubernetes的默认版本是与特定版本的系统镜像绑定的：

- 对于RKE v0.2.x及以下版本，版本和系统镜像版本位于： <https://github.com/rancher/types/blob/release/v2.2/apis/management.cattle.io/v3/k8s_defaults.go>
- 对于RKE v0.3.0及以上版本，版本和系统镜像版本位于：<https://github.com/rancher/kontainer-driver-metadata/blob/master/rke/k8s_rke_system_images.go>

<br/>

随着RKE版本的发布，镜像的标签也是会变化。下面是`v1.10.3-rancher2`的示例：

```yaml
system_images:
  etcd: rancher/coreos-etcd:v3.2.24
  alpine: rancher/rke-tools:v0.1.24
  nginx_proxy: rancher/rke-tools:v0.1.24
  cert_downloader: rancher/rke-tools:v0.1.24
  kubernetes: rancher/hyperkube:v1.13.1-rancher1
  kubernetes_services_sidecar: rancher/rke-tools:v0.1.24
  pod_infra_container: rancher/pause-amd64:3.1

  # kube-dns images
  kubedns: rancher/k8s-dns-kube-dns-amd64:1.15.0
  dnsmasq: rancher/k8s-dns-dnsmasq-nanny-amd64:1.15.0
  kubedns_sidecar: rancher/k8s-dns-sidecar-amd64:1.15.0
  kubedns_autoscaler: rancher/cluster-proportional-autoscaler-amd64:1.0.0

  # CoreDNS images
  coredns: coredns/coredns:1.2.6
  coredns_autoscaler: rancher/cluster-proportional-autoscaler-amd64:1.0.0

  # Flannel images
  flannel: rancher/coreos-flannel:v0.10.0
  flannel_cni: rancher/coreos-flannel-cni:v0.3.0

  # Calico images
  calico_node: rancher/calico-node:v3.4.0
  calico_cni: rancher/calico-cni:v3.4.0
  calico_controllers: ""
  calico_ctl: rancher/calico-ctl:v2.0.0

  # Canal images
  canal_node: rancher/calico-node:v3.4.0
  canal_cni: rancher/calico-cni:v3.4.0
  canal_flannel: rancher/coreos-flannel:v0.10.0

  # Weave images
  weave_node: weaveworks/weave-kube:2.5.0
  weave_cni: weaveworks/weave-npc:2.5.0

  # Ingress controller images
  ingress: rancher/nginx-ingress-controller:0.21.0-rancher1
  ingress_backend: rancher/nginx-ingress-controller-defaultbackend:1.4

  # Metrics server image
  metrics_server: rancher/metrics-server-amd64:v0.3.1
```



<br/>
<br/>



## 默认的k8s服务

k8s的核心服务：

- etcd
- kube-apiserver
- kube-controller-manager
- kube-scheduler
- kubelet
- kube-proxy

目前，RKE 并不支持scheduler服务的任何特定选项。RKE 不支持 "kubeproxy "服务的任何特定选项。栗子：

```yaml
services:
  # kube-apiserver
  kube-api:
    # 在Kubernetes上创建的服务的IP范围。
    # 必须与kube-controller中的service_cluster_ip_range匹配
    service_cluster_ip_range: 10.43.0.0/16
    # 为NodePort服务提供不同的端口范围
    service_node_port_range: 30000-32767
    pod_security_policy: false
    # 启用AlwaysPullImagesAdmission controller插件
    # v0.2.0或更新版本可用
    always_pull_images: false
    secrets_encryption_config:
      enabled: true
  # kube-controller-manager
  kube-controller:
    # CIDR pool used to assign IP addresses to pods in the cluster
    cluster_cidr: 10.42.0.0/16
    # IP range for any services created on Kubernetes
    # This must match the service_cluster_ip_range in kube-api
    service_cluster_ip_range: 10.43.0.0/16
  # kubelet
  kubelet:
    # Base domain for the cluster
    cluster_domain: cluster.local
    # IP address for the DNS service endpoint
    cluster_dns_server: 10.43.0.10
    # Fail if swap is on
    fail_swap_on: false
    # Generate per node serving certificate
    generate_serving_certificate: false
```


<br/>
<br/>


### 自定义参数

RKE支持用户添加自定义参数、挂载存储卷和添加额外的环境变量。

对于任何一个k8s服务，可以使用`extra_args`来改变现有的默认值。可以使用`extra_binds`参数为服务添加额外的存储卷绑定。可以使用`extra_env`参数为服务添加额外的环境变量。


<br/>
<br/>


### 外部etcd

RKE不接受外部 etcd 服务器与节点一起使用etcd角色。



<br/>
<br/>



## 事件速率限制

使用`EventRateLimit`接纳控制对 API 服务器在特定时间段内接受的事件数量进行限制。在一个大型多租户集群中，可能会有一小部分租户用事件请求淹没服务器，这可能会对集群的整体性能产生重大影响。因此，建议限制 API 服务器接受事件的速率。

可以为服务器、命名空间、用户或源和对象的组合配置速率限制。



<br/>
<br/>



## RKE插件

包括:

- 网络插件
- Ingress controller插件
- DNS提供商插件
- Metrics Server插件

<br/>

对于每个Kubernetes版本，都有与每个插件相关联的默认镜像版本，但这些镜像可以通过更改`system_images`中的镜像来覆盖。

RKE使用kubernetes job的方式部署插件。


<br/>


### 网络插件

RKE 提供了以下网络插件，作为附加组件部署：

- Flannel
- Calico
- Cannal
- Weave

在你启动集群后，你不能改变你的网络供应商。因此，仔细选择你要使用的网络供应商，因为Kubernetes不允许在网络供应商之间切换。一旦用网络提供商创建了一个集群，改变网络提供商将需要你拆掉整个集群及其所有的应用程序。

```yaml
# rke默认使用canal网络插件
network:
  plugin: canal
```


<br/>
<br/>


### DNS提供商

RKE提供了以下三种DNS提供商，作为附加组件部署：

- NodeLocal DNS
- CoreDNS
- kube-dns

```yaml
# CoreDNS只能在Kubernetes v1.14.0及以上版本上使用。
# 调度dns到特定节点
nodes:
  - address: 1.1.1.1
    role: [controlplane, worker, etcd]
    user: root
    labels:
      app: dns

dns:
  provider: coredns
  # 节点选择器
  node_selector:
    app: dns
  # 上游DNS服务器
  upstreamnameservers:
    - 1.1.1.1
    - 8.8.4.4
  # 容忍度
  # tolerations
```


<br/>
<br/>


### K8s Ingress Controllers

默认情况下，RKE会在所有worker节点上部署nginx ingress controller。

RKE 将以 DaemonSet 的形式部署 Ingress Controller，并使用 `hostnetwork: true`，因此在部署控制器的每个节点上都会打开 80和443端口。

如果只想在特定的节点上部署Ingress Controller，可以设置节点选择器(`node_selector`)将其调度到特定的节点上。

```yaml
ingress:
  provider: nginx
  options:
    key: xxx
  extra_args:
    key: xxx
  # 默认启用了默认后端(default backend 404)
  default_backend: true
```


<br/>
<br/>


### Metrics Server

默认情况下，RKE 会部署 Metrics Server来提供集群中资源的指标。




<br/>

---

<br/>




# 配置文件示例

一个`cluster.yml`配置文件示例：

```yaml
nodes:
  - address: 1.1.1.1
    user: ubuntu
    role:
      - controlplane
      - etcd
      port: 2222
      docker_socket: /var/run/docker.sock
    - address: 2.2.2.2
      user: ubuntu
      role:
        - worker
      ssh_key_path: /home/user/.ssh/id_rsa
      ssh_key: |-
        -----BEGIN RSA PRIVATE KEY-----
        -----END RSA PRIVATE KEY-----
      ssh_cert_path: /home/user/.ssh/test-key-cert.pub
      ssh_cert: |-
        ssh-rsa-cert-v01@openssh.com AAAAHHNzaC1yc2EtY2VydC12MDFAb3Bl....
    - address: example.com
      user: ubuntu
      role:
        - worker
      hostname_override: node3
      internal_address: 192.168.1.6
      labels:
        app: ingress
      taints:
        - key: test-key
          value: test-value
          effect: NoSchedule
# If set to true, RKE will not fail when unsupported Docker version
# are found
ignore_docker_version: false

# Enable running cri-dockerd
# Up to Kubernetes 1.23, kubelet contained code called dockershim 
# to support Docker runtime. The replacement is called cri-dockerd 
# and should be enabled if you want to keep using Docker as your
# container runtime
# Only available to enable in Kubernetes 1.21 and higher
enable_cri_dockerd: true

# Cluster level SSH private key
# Used if no ssh information is set for the node
ssh_key_path: ~/.ssh/test
# Enable use of SSH agent to use SSH private keys with passphrase
# This requires the environment `SSH_AUTH_SOCK` configured pointing
#to your SSH agent which has the private key added
ssh_agent_auth: true
# List of registry credentials
# If you are using a Docker Hub registry, you can omit the `url`
# or set it to `docker.io`
# is_default set to `true` will override the system default
# registry set in the global settings
private_registries:
     - url: registry.com
       user: Username
       password: password
       is_default: true
# Bastion/Jump host configuration
bastion_host:
    address: x.x.x.x
    user: ubuntu
    port: 22
    ssh_key_path: /home/user/.ssh/bastion_rsa
# or
#   ssh_key: |-
#     -----BEGIN RSA PRIVATE KEY-----
#
#     -----END RSA PRIVATE KEY-----
# Set the name of the Kubernetes cluster
cluster_name: mycluster
# The Kubernetes version used. The default versions of Kubernetes
# are tied to specific versions of the system images.
#
# For RKE v0.2.x and below, the map of Kubernetes versions and their system images is
# located here:
# https://github.com/rancher/types/blob/release/v2.2/apis/management.cattle.io/v3/k8s_defaults.go
#
# For RKE v0.3.0 and above, the map of Kubernetes versions and their system images is
# located here:
# https://github.com/rancher/kontainer-driver-metadata/blob/master/rke/k8s_rke_system_images.go
#
# In case the kubernetes_version and kubernetes image in
# system_images are defined, the system_images configuration
# will take precedence over kubernetes_version.
kubernetes_version: v1.10.3-rancher2
# System Images are defaulted to a tag that is mapped to a specific
# Kubernetes Version and not required in a cluster.yml.
# Each individual system image can be specified if you want to use a different tag.
#
# For RKE v0.2.x and below, the map of Kubernetes versions and their system images is
# located here:
# https://github.com/rancher/types/blob/release/v2.2/apis/management.cattle.io/v3/k8s_defaults.go
#
# For RKE v0.3.0 and above, the map of Kubernetes versions and their system images is
# located here:
# https://github.com/rancher/kontainer-driver-metadata/blob/master/rke/k8s_rke_system_images.go
#
system_images:
    kubernetes: rancher/hyperkube:v1.10.3-rancher2
    etcd: rancher/coreos-etcd:v3.1.12
    alpine: rancher/rke-tools:v0.1.9
    nginx_proxy: rancher/rke-tools:v0.1.9
    cert_downloader: rancher/rke-tools:v0.1.9
    kubernetes_services_sidecar: rancher/rke-tools:v0.1.9
    kubedns: rancher/k8s-dns-kube-dns-amd64:1.14.8
    dnsmasq: rancher/k8s-dns-dnsmasq-nanny-amd64:1.14.8
    kubedns_sidecar: rancher/k8s-dns-sidecar-amd64:1.14.8
    kubedns_autoscaler: rancher/cluster-proportional-autoscaler-amd64:1.0.0
    pod_infra_container: rancher/pause-amd64:3.1
services:
    etcd:
      # Custom uid/guid for etcd directory and files
      uid: 52034
      gid: 52034
      # if external etcd is used
      # path: /etcdcluster
      # external_urls:
      #   - https://etcd-example.com:2379
      # ca_cert: |-
      #   -----BEGIN CERTIFICATE-----
      #   xxxxxxxxxx
      #   -----END CERTIFICATE-----
      # cert: |-
      #   -----BEGIN CERTIFICATE-----
      #   xxxxxxxxxx
      #   -----END CERTIFICATE-----
      # key: |-
      #   -----BEGIN PRIVATE KEY-----
      #   xxxxxxxxxx
      #   -----END PRIVATE KEY-----
    # Note for Rancher v2.0.5 and v2.0.6 users: If you are configuring
    # Cluster Options using a Config File when creating Rancher Launched
    # Kubernetes, the names of services should contain underscores
    # only: `kube_api`.
    kube-api:
      # IP range for any services created on Kubernetes
      # This must match the service_cluster_ip_range in kube-controller
      service_cluster_ip_range: 10.43.0.0/16
      # Expose a different port range for NodePort services
      service_node_port_range: 30000-32767
      pod_security_policy: false
      # Encrypt secret data at Rest
      # Available as of v0.3.1
      secrets_encryption_config:
        enabled: true
        custom_config:
          apiVersion: apiserver.config.k8s.io/v1
          kind: EncryptionConfiguration
          resources:
          - resources:
            - secrets
            providers:
            - aescbc:
                keys:
                - name: k-fw5hn
                  secret: RTczRjFDODMwQzAyMDVBREU4NDJBMUZFNDhCNzM5N0I=
            - identity: {}
      # Enable audit logging
      # Available as of v1.0.0
      audit_log:
        enabled: true
        configuration:
          max_age: 6
          max_backup: 6
          max_size: 110
          path: /var/log/kube-audit/audit-log.json
          format: json
          policy:
            apiVersion: audit.k8s.io/v1 # This is required.
            kind: Policy
            omitStages:
              - "RequestReceived"
            rules:
              # Log pod changes at RequestResponse level
              - level: RequestResponse
                resources:
                - group: ""
                  # Resource "pods" doesn't match requests to any subresource of pods,
                  # which is consistent with the RBAC policy.
                  resources: ["pods"]
      # Using the EventRateLimit admission control enforces a limit on the number of events
      # that the API Server will accept in a given time period
      # Available as of v1.0.0
      event_rate_limit:
        enabled: true
        configuration:
          apiVersion: eventratelimit.admission.k8s.io/v1alpha1
          kind: Configuration
          limits:
          - type: Server
            qps: 6000
            burst: 30000
      # Enable AlwaysPullImages Admission controller plugin
      # Available as of v0.2.0
      always_pull_images: false
      # Add additional arguments to the kubernetes API server
      # This WILL OVERRIDE any existing defaults
      extra_args:
        # Enable audit log to stdout
        audit-log-path: "-"
        # Increase number of delete workers
        delete-collection-workers: 3
        # Set the level of log output to debug-level
        v: 4
    # Note for Rancher 2 users: If you are configuring Cluster Options
    # using a Config File when creating Rancher Launched Kubernetes,
    # the names of services should contain underscores only:
    # `kube_controller`. This only applies to Rancher v2.0.5 and v2.0.6.
    kube-controller:
      # CIDR pool used to assign IP addresses to pods in the cluster
      cluster_cidr: 10.42.0.0/16
      # IP range for any services created on Kubernetes
      # This must match the service_cluster_ip_range in kube-api
      service_cluster_ip_range: 10.43.0.0/16
      # Add additional arguments to the kubernetes API server
      # This WILL OVERRIDE any existing defaults
      extra_args:
        # Set the level of log output to debug-level
        v: 4
        # Enable RotateKubeletServerCertificate feature gate
        feature-gates: RotateKubeletServerCertificate=true
        # Enable TLS Certificates management
        # https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/
        cluster-signing-cert-file: "/etc/kubernetes/ssl/kube-ca.pem"
        cluster-signing-key-file: "/etc/kubernetes/ssl/kube-ca-key.pem"
    kubelet:
      # Base domain for the cluster
      cluster_domain: cluster.local
      # IP address for the DNS service endpoint
      cluster_dns_server: 10.43.0.10
    # Fail if swap is on
    fail_swap_on: false
    # Configure pod-infra-container-image argument
      pod-infra-container-image: "k8s.gcr.io/pause:3.2"
      # Generate a certificate signed by the kube-ca Certificate Authority
      # for the kubelet to use as a server certificate
      # Available as of v1.0.0
      generate_serving_certificate: true
      extra_args:
        # Set max pods to 250 instead of default 110
        max-pods: 250
        # Enable RotateKubeletServerCertificate feature gate
        feature-gates: RotateKubeletServerCertificate=true
      # Optionally define additional volume binds to a service
      extra_binds:
        - "/usr/libexec/kubernetes/kubelet-plugins:/usr/libexec/kubernetes/kubelet-plugins"
    scheduler:
      extra_args:
        # Set the level of log output to debug-level
        v: 4
    kubeproxy:
      extra_args:
        # Set the level of log output to debug-level
        v: 4
# Currently, only authentication strategy supported is x509.
# You can optionally create additional SANs (hostnames or IPs) to
# add to the API server PKI certificate.
# This is useful if you want to use a load balancer for the
# control plane servers.
authentication:
  strategy: x509
  sans:
    - "10.18.160.10"
    - "my-loadbalancer-1234567890.us-west-2.elb.amazonaws.com"

# Kubernetes Authorization mode
# Use `mode: rbac` to enable RBAC
# Use `mode: none` to disable authorization
authorization:
  mode: rbac

# If you want to set a Kubernetes cloud provider, you specify
# the name and configuration
cloud_provider:
  name: aws

# Add-ons are deployed using kubernetes jobs. RKE will give
# up on trying to get the job status after this timeout in seconds..
addon_job_timeout: 30

# Specify network plugin-in (canal, calico, flannel, weave, or none)
network:
  plugin: canal
  # Specify MTU
  mtu: 1400
  options:
    # Configure interface to use for Canal
    canal_iface: eth1
    canal_flannel_backend_type: vxlan
    # Available as of v1.2.6
    canal_autoscaler_priority_class_name: system-cluster-critical
    canal_priority_class_name: system-cluster-critical
  # Available as of v1.2.4
  tolerations:
  - key: "node.kubernetes.io/unreachable"
    operator: "Exists"
    effect: "NoExecute"
    tolerationseconds: 300
  - key: "node.kubernetes.io/not-ready"
    operator: "Exists"
    effect: "NoExecute"
    tolerationseconds: 300
  # Available as of v1.1.0
  update_strategy:
    strategy: RollingUpdate
    rollingUpdate:
      maxUnavailable: 6

# Specify DNS provider (coredns or kube-dns)
dns:
  provider: coredns
  # Available as of v1.1.0
  update_strategy:
    strategy: RollingUpdate
    rollingUpdate:
      maxUnavailable: 20%
      maxSurge: 15%
  linear_autoscaler_params:
    cores_per_replica: 0.34
    nodes_per_replica: 4
    prevent_single_point_failure: true
    min: 2
    max: 3
# Specify monitoring provider (metrics-server)
monitoring:
  provider: metrics-server
  # Available as of v1.1.0
  update_strategy:
    strategy: RollingUpdate
    rollingUpdate:
      maxUnavailable: 8
# Currently only nginx ingress provider is supported.
# To disable ingress controller, set `provider: none`
# `node_selector` controls ingress placement and is optional
ingress:
  provider: nginx
  node_selector:
    app: ingress
  # Available as of v1.1.0
  update_strategy:
    strategy: RollingUpdate
    rollingUpdate:
      maxUnavailable: 5
# All add-on manifests MUST specify a namespace
addons: |-
  ---
  apiVersion: v1
  kind: Pod
  metadata:
    name: my-nginx
    namespace: default
  spec:
    containers:
    - name: my-nginx
      image: nginx
      ports:
      - containerPort: 80

addons_include:
  - https://raw.githubusercontent.com/rook/rook/master/cluster/examples/kubernetes/rook-operator.yaml
  - https://raw.githubusercontent.com/rook/rook/master/cluster/examples/kubernetes/rook-cluster.yaml
  - /path/to/manifest
```




