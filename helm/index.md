# Helm



参考：

- docs: <https://docs.helm.sh/>
- github: <https://github.com/helm>

环境：

- el7x86_64
- helm v3.3









<br/>
<br/>

<!--more-->

---

<br/>
<br/>









# 概述

**Helm**是Kubernetes生态系统中的一个软件包管理工具，主要用来管理**Charts**，有点类似于Ubuntu中的apt或CentOS中的yum。由go编写，是Deis公司发起的一个开源工具，有助于简化部署和管理Kubernetes应用。

在Kubernetes中，应用管理是需求最多、挑战最大的领域。Helm项目提供了一个统一软件打包方式，支持版本控制，可以大大简化Kubernetes应用分发与部署中的复杂性。

Helm Chart是用来封装 Kubernetes原生应用程序的一系列YAML文件。可以在你部署应用的时候自定义应用程序的一些 Metadata，以便于应用程序的分发。

对于应用发布者而言，可以通过 Helm 打包应用、管理应用依赖关系、管理应用版本并发布应用到软件仓库。

对于使用者而言，使用 Helm 后不用需要编写复杂的应用部署文件，可以以简单的方式在 Kubernetes 上查找、安装、升级、回滚、卸载应用程序。

<br>

![](/images/Helm/Helm_Logo.png)

<br>

The package manager for Kubernetes.

Helm is the best way to find, share, and use software built for Kubernetes.
















<br>
<br>

---

<br>
<br>










# 术语

Glossary: <https://helm.sh/docs/glossary/>

<br>

**Chart**

Helm包涵盖了将k8s资源安装到k8s集群所需的足够多的信息。
Charts包含了`Chart.yaml`文件核模板，默认值(`values.yaml`)，以及相关依赖。
Charts开发设计了良好定义的目录结构，并打包为chart archive。

<br>

**Chart Archive**

Chart包是被`tar`和`gzip`压缩（可选签名）的chart。

<br>

**Chart Dependency(Subcharts)**

Chart可以依赖于其它chart。依赖有两种方式：

- 软依赖(soft): 如果另一个chart没有在集群中安装，chart可能会无法使用
- 硬依赖(hard): chart包含它所依赖的chart。（在`charts/`目录中）

当一个chart打包(`helm package`)时，所有的依赖都会和它绑定。

<br>

**Chart Version**

每个chart都需要版本号。

<br>

**Chart.yaml**

chart的信息说明被存储在一个特定文件(`Chart.yaml`)。每个chart都必须有这个文件。

<br>

**helm**

Helm是k8s包管理器。作为一个操作系统包管理器，使其很容易在操作系统中安装工具。Helm使得k8s集群中安装应用和资源变得异常简单。

<br>

**Helm Configuration Files**

Helm将配置文件存储在XDG目录中。`helm`第一次运行，会自动生成。

**Kube Config(KUBECONFIG)**

helm客户端通过Kube config配置文件来理解k8s集群。默认`$HOME/.kube/config`。

<br>

**Lint**

Helm代码规范，规范一个chart是去验证其遵照Helm chart的标准规范和要求。Helm提供了`helm lint`命令。

<br>

**Provenance**

Helm chart可以由来源文件(provenance file)提供chart的出处以及它所包含的内容。

来源文件(`.prov`)是Helm安全的一部分。一个来源包含chart包文件的加密哈希值，`Chart.yaml`数据，一个签名块。当再加上一个钥匙链(keychain)时，可为chart用户提供以下能力：

- 验证chart被可信第三方签名
- 验证chart文件没有被篡改
- 验证chart的元数据内容(`Chart.yaml`)
- 快速匹配chart的数据来源

<br>

**Release**

发行版本。chart安装之后，Helm库会创建一个release来跟踪这个安装。

单个chart可以在同一个集群中安装多次，并能创建多个不同的版本。

<br>

**Release Number/Version**

单个版本号可以被升级多次。通过连续技术来跟踪升级发布版本。

<br>

**Rollback**

每一次发布会更新chart或者配置。当生成发布历史后，一次发布也可以被 rolled back 之前的发布版本号。回滚使用`helm rollback`命令。

重要的是, 每一次回滚版本会生成一个新的发布版本号。

```
操作			版本号
install		release 1
upgrade		release 2
upgrade		release 3
rollback 1	release 4 (但使用release 1的配置)
```

<br>

**Helm Library(SDK)**

Helm库（或SDK）涉及到go代码，可以直接与k8s API服务交互进行安装、升级、查询 以及移除k8s资源。

<br>

**Repository**

Helm chart可以被存储到专用的HTTP服务器上，称之为chart仓库。

Helm客户端可以指向零个或多个chart仓库。默认没有配置仓库，可使用`helm repo add`添加。

<br>

**Values**

Values 提供了一种使用您自己的信息覆盖模板默认值的方式。

Helm Chart是参数化的, 这意味着chart开发者可以在安装时显式配置。比如说，chart可以暴露`username`字段， 允许为服务设置一个用户名。这些可暴露的变量在Helm用语中称为`values`。

Values可在`helm install`, `helm upgrage`时设置。也可以在`values.yaml`文件中设置。


























<br>
<br>

---

<br>
<br>












# 介绍

Introduction: <https://helm.sh/docs/intro/>


<br>


## 快速入门

Quickstart: <https://helm.sh/docs/intro/quickstart/>

如何快速安装核使用Helm。


<br>


### 先决条件

Prerequisites

使用Helm的前置条件：

- k8s集群
  - 建议最新k8s稳定版
  - `kubectl`
- 安装的安全配置(如果有的话)
- 安装和配置Helm

注意Helm版本对应支持的k8s版本。


<br>
<br>


### 安装

Install: <https://helm.sh/docs/intro/install/>

从源码、或二进制安装Helm CLI。


<br>


#### 从Helm项目

From The Helm Project


<br>

**从二进制包:**

- 下载特定版本包: <https://github.com/helm/helm/releases>
- 解压
- 添加到PATH

```sh
wget https://get.helm.sh/helm-v3.3.3-linux-amd64.tar.gz

tar -zxvf helm-v3.3.3-linux-amd64.tar.gz

mv helm /usr/local/bin/helm
```


<br>

**从脚本:**

```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```


<br>
<br>


#### 从源码

```sh
git clone https://github.com/helm/helm.git
cd helm
make
```


<br>
<br>


### 初始化Helm chart

Initialize a Helm Chart Repository

Helm安装好之后，你可以添加一个chart仓库。

```sh
# 添加Helm官方仓库
helm repo add stable https://kubernetes-charts.storage.googleapis.com/


# 查看安装的charts列表
helm search repo stable
NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION
stable/acs-engine-autoscaler            2.2.2           2.1.1                           DEPRECATED Scales worker nodes within agent pools
stable/aerospike                        0.2.8           v4.5.0.5                        A Helm chart for Aerospike in Kubernetes
stable/airflow                          4.1.0           1.10.4                          Airflow is a platform to programmatically autho...
stable/ambassador                       4.1.0           0.81.0                          A Helm chart for Datawire Ambassador
```


<br>
<br>


### 安装Chart

Install an Example Chart

可以通过`helm install`命令安装chart。

```sh
helm repo update

# helm install，都会创建一个新的release
# 所以一个chart在同一个集群里面可以被安装多次，每一个都可以被独立的管理和升级
helm install stable/mysql --generate-name
NAME: mysql-1600679719
LAST DEPLOYED: Mon Sep 21 17:15:23 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
mysql-1600679719.default.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default mysql-1600679719 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h mysql-1600679719 -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=127.0.0.1
    MYSQL_PORT=3306

    # Execute the following command to route the connection:
    kubectl port-forward svc/mysql-1600679719 3306

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}

#查看此chart的基本信息
helm show chart stable/mysql
helm show all stable/mysql

```


<br>
<br>


### Releases

```sh
# 查看chart发行版
helm ls
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
mysql-1600679719        default         1               2020-09-21 17:15:23.169811348 +0800 CST deployed        mysql-1.6.7     5.7.30


# 列出所有部署的发行
helm list
```


<br>
<br>


### 卸载Release

使用`helm uninstall`命令卸载realease。

```sh
helm uninstall mysql-1600679719
release "mysql-1600679719" uninstalled
```

它会删除和该release相关的所有资源。使用`--keep-history`选项，Helm将保存release history。所以你可以审计集群历史甚至使用`helm rollback`回滚release。














<br>
<br>

---

<br>
<br>


























# 主题

Topic Guides: <https://helm.sh/docs/topics/>


<br>


## Charts

Charts: <https://helm.sh/docs/topics/charts/>


Helm使用的包格式称为charts。chart就是一个描述k8s相关资源的文件集合。单个chart可以用来部署简单或复杂的服务。

Chart是作为特定目录布局的文件被创建，它们可以打包到要部署的版本存档中。

```
# 下载一个chart，但不安装
helm pull xxx
```


<br>
<br>


### 文件结构

chart是一个组织在文件目录中的集合。目录名称就是chart名称(没有版本信息)。

示例:

```
wordpress/
  Chart.yaml          # 包含了chart信息的YAML文件
  LICENSE             # 可选: 包含chart许可证的纯文本文件
  README.md           # 可选: 可读的README文件
  values.yaml         # chart 默认的配置值
  values.schema.json  # 可选: 一个使用JSON结构的values.yaml文件
  charts/             # 包含chart依赖的其他chart
  crds/               # 自定义资源的定义
  templates/          # 模板目录， 当和values 结合时，可生成有效的Kubernetes manifest文件
  templates/NOTES.txt # 可选: 包含简要使用说明的纯文本文件
```


<br>
<br>


### Chart.yaml

`Chart.yaml`文件是chart必须的。包含以下字段。

```yaml
apiVersion: chart API 版本 （必需）
name: chart名称 （必需）
version: 语义化2 版本（必需）
kubeVersion: 兼容Kubernetes版本的语义化版本（可选）
description: 一句话对这个项目的描述（可选）
type: chart类型 （可选）
keywords:
  - 关于项目的一组关键字（可选）
home: 项目home页面的URL （可选）
sources:
  - 项目源码的URL列表（可选）
dependencies: # chart 必要条件列表 （可选）
  - name: chart名称 (nginx)
    version: chart版本 ("1.2.3")
    repository: 仓库URL ("https://example.com/charts") 或别名 ("@repo-name")
    condition: （可选） 解析为布尔值的yaml路径，用于启用、禁用chart (e.g. subchart1.enabled )
    tags: # （可选）
      - 用于一次启用/禁用 一组chart的tag
    enabled: （可选） 决定是否加载chart的布尔值
    import-values: # （可选）
      - ImportValue 保存源值到导入父键的映射。每项可以是字符串或者一对子/父列表项
    alias: （可选） chart中使用的别名。当你要多次添加相同的chart时会很有用
maintainers: # （可选）
  - name: 维护者名字 （每个维护者都需要）
    email: 维护者邮箱 （每个维护者可选）
    url: 维护者URL （每个维护者可选）
icon: 用做icon的SVG或PNG图片URL （可选）
appVersion: 包含的应用版本（可选）。不需要是语义化的
deprecated: 不被推荐的chart （可选，布尔值）
annotations:
  example: 按名称输入的批注列表 （可选）.
```


<br>

**Chart和版本控制**

每个chart都必须有版本号。版本必须遵循SemVer2标准。

```
# nginx chart的版本字段version: 1.2.3
# 按照名称设置为
nginx-1.2.3.tgz
```

`Chart.yaml`文件中的`version`字段被很多Helm工具使用。当生成一个包时，`helm package`命令可以用`Chart.yaml`文件中找到的版本号作为包名的token。系统假设chart包名中的版本号可以与`Chart.yaml`文件中的版本号匹配。如果不满足这一假设会导致错误。

<br>

**apiVersion字段**

对于至少需要Helm3的chart，`apiVersion`字段应该是`v2`。

<br>

**kubeVersion字段**

可选的`kubeVersion`字段可以在支持的k8s版本上定义语义约束，Helm 在安装chart时会验证这个版本约束， 并在集群运行不支持的k8s版本时显示失败。

版本约束可以包含空格、比较操作符、逻辑操作符。

```
>= 1.13.0 < 1.15.0

>= 1.13.0 < 1.14.0 || >= 1.14.1 < 1.15.0

1.1 - 2.3.4
```

<br>

**deprecated字段**

在Chart仓库管理chart时，有时需要废弃一个chart。`deprecated`字段可用来标记已弃用的chart。如果latest版本被标记为已弃用，则所有的chart都会被认为是已弃用的。以后可以通过发布未标记为已弃用的新版本来重新使用chart名称。

`kubernetes/charts`项目遵循的弃用charts的流程为：

- 升级chart的`Chart.yaml`文件，将这个文件标记为已弃用，并更改版本
- 在chart仓库中发布新版的chart
- 从源仓库中移除这个chart

<br>

**type字段**

`type`字段定义了chart的类型。有两种类型：

- `application`：默认类型，是可以完全操作的标准chart。
- `library`：不能安装，提供针对chart构建的实用程序和功能。通常不包含任何资源对象。

应用类型chart 可以作为库类型chart使用。可以通过将类型设置为`library`来实现。 然后这个库就被渲染成了一个库类型chart，所有的实用程序和功能都可以使用。所有的资源对象不会被渲染。


<br>
<br>


### 许可证和描述

Chart LICENSE, README and NOTES

Chart也可以包含描述安装、配置和使用的文件，以及chart许可证。

LICENSE是一个包含了chart license的纯文本文件。chart可以包含一个许可证，因为在模板里不只是配置，还可能有编码逻辑。如果需要，还可以为chart安装的应用程序提供单独的许可证。

README自述文件，一般包含：

- chart提供的应用或服务的描述
- 运行chart的先决条件或要求
- `values.yaml`的可选项和默认值的描述
- 与chart的安装或配置相关的其它信息

chart也会包含一个简短的纯文本`templates/NOTES.txt`文件，这会在安装后及查看版本状态时打印出来。由于此文件是在运行`helm install`或`helm status`时打印到STDOUT的，因此建议保持内容简短，并指向自述文件以获取更多详细信息。


<br>
<br>


### 依赖

Chart Dependencies

Helm中，chart可能会依赖其它任意个chart。这些依赖可使用`dependencies`字段(`Chart.yaml`)动态链接，或写入`charts/`目录。

<br>

**dependencies字段**

当前chart依赖的其它chart会在`dependencies`字段定义为一个列表。

```yaml
dependencies:
  - name: apache
    version: 1.2.3
    repository: https://example.com/charts
  - name: mysql
    version: 3.2.1
    repository: https://another.example.com/charts
```

必须使用`helm repo add`在本地添加仓库。

一旦你定义好了依赖，运行`helm dependency update`就会使用你的依赖文件下载所有你指定的chart到你的`charts/`目录。

<br>

**alias字段**

为依赖chart添加一个别名，会使用别名作为新依赖chart的名称。 需要使用其他名称访问chart时可以使用`alias`。

```
dependencies:
  - name: subchart
    repository: http://localhost:10191
    version: 0.1.0
    alias: new-subchart-1
  - name: subchart
    repository: http://localhost:10191
    version: 0.1.0
    alias: new-subchart-2
  - name: subchart
    repository: http://localhost:10191
    version: 0.1.0
```

<br>

**tags和condition字段**

```yaml
dependencies:
  - name: subchart1
    repository: http://localhost:10191
    version: 0.1.0
    condition: subchart1.enabled, global.subchart1.enabled
    tags:
      - front-end
      - subchart1
  - name: subchart2
    repository: http://localhost:10191
    version: 0.1.0
    condition: subchart2.enabled,global.subchart2.enabled
    tags:
      - back-end
      - subchart2
```

```yaml
#values.yaml
subchart1:
  enabled: true
tags:
  front-end: false
  back-end: true
```

```
# 可以在CLI使用--set参数来设置标签和条件值
helm install --set tags.front-end=true --set subchart2.enabled=false
```

<br>

**通过依赖导入sub values**

在某些情况下，允许子chart的值作为公共默认传递到父chart中是值得的。

```yaml
# parent's Chart.yaml file

dependencies:
  - name: subchart
    repository: http://localhost:10191
    version: 0.1.0
    import-values:
      - data
```

```yaml
# child's values.yaml file

exports:
  data:
    myint: 99
```

<br>

**通过charts目录手动管理依赖**

如果对依赖进行更多控制，通过将有依赖关系的chart复制到`charts/`目录中来显式表达这些依赖关系。

要将依赖放入`charts/`目录，使用`helm pull`命令。



<br>
<br>


### Templates and Values

Helm Chart模板是按照[Go模板语言](https://golang.org/pkg/text/template/)书写。让我想起了Django模板语言，Jinja2模板语言。

所有模板语言存放在chart的`templates/`目录下。当Helm渲染chart时，它会通过模板引擎遍历目录中的每个文件。

模板的Value通过两种方式提供：

- 通过`values.yaml`文件提供，此文件包含了默认值。
- 用户可以提供一个包含value的yaml文件，在`helm install`时使用它。

当用户提供自定义的value时，会覆盖`values.yaml`中的值。

<br>

**模板文件示例**

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: deis-database
  namespace: deis
  labels:
    app.kubernetes.io/managed-by: deis
spec:
  replicas: 1
  selector:
    app.kubernetes.io/name: deis-database
  template:
    metadata:
      labels:
        app.kubernetes.io/name: deis-database
    spec:
      serviceAccount: deis-database
      containers:
        - name: deis-database
          image: {{ .Values.imageRegistry }}/postgres:{{ .Values.dockerTag }}
          imagePullPolicy: {{ .Values.pullPolicy }}
          ports:
            - containerPort: 5432
          env:
            - name: DATABASE_STORAGE
              value: {{ default "minio" .Values.storage }}
```

<br>

**预定义的Values**

以下值是预定义的，对每个模板都有效，并且可以被覆盖。和所有值一样，名称 区分大小写：

- `Release.Name`: 版本名称(非chart的)
- `Release.Namespace`: 发布的chart版本的命名空间
- `Release.Service`: 组织版本的服务
- `Release.IsUpgrade`: 如果当前操作是升级或回滚，设置为true
- `Release.IsInstall`: 如果当前操作是安装，设置为true
- `Chart`: `Chart.yaml`的内容。因此，chart的版本可以从`Chart.Version`获得， 并且维护者在`Chart.Maintainers`里
- `Files`：chart中的包含了非特殊文件的类图对象
- `Capabilities`: 包含了Kubernetes版本信息的类图对象

<br>

**范围**

Scope, Dependencies, and Values

Values文件可以声明顶级chart的值，以及`charts/`目录中包含的其他任意chart。

<br>

**全局Values**

Helm支持特殊的`global`值。

```yaml
global:
  app: MyWordPress
```

这个值以`.Values.global.app`在所有chart中有效。

<br>

**架构文件**

有时候，chart容器可能想基于它们的values值定义一个结构，这可以在`values.schema.json`文件中定义一个架构实现。

示例：

```json
{
  "$schema": "https://json-schema.org/draft-07/schema#",
  "properties": {
    "image": {
      "description": "Container Image",
      "properties": {
        "repo": {
          "type": "string"
        },
        "tag": {
          "type": "string"
        }
      },
      "type": "object"
    },
    "name": {
      "description": "Service name",
      "type": "string"
    },
    "port": {
      "description": "Port",
      "minimum": 0,
      "type": "integer"
    },
    "protocol": {
      "type": "string"
    }
  },
  "required": [
    "protocol",
    "port"
  ],
  "title": "Values",
  "type": "object"
}
```

这个架构会应用values值并验证它。当执行以下任意命令时会进行验证： `helm install, helm upgrage, helm lint, helm template`。



<br>
<br>


### 用户自定义资源

Custom Resource Definitions

k8s提供了一种声明k8s新类型对象的机制。使用CustomResourceDefinition（CRD），k8s开发者可以声明自定义资源类型。

Helm3中，CRD被视为一种特殊的对象。它们被安装在chart的其他部分之前，并受到一些限制。

CRD YAML文件应被放置在chart的`crds/`目录中。 多个CRD(用YAML的开始`---`和结束符`...`分隔)可以被放置在同一个文件中。Helm会尝试加载CRD目录中所有的文件到k8s。

当Helm安装新chart时，会上传CRD，暂停安装直到CRD可以被API服务使用，然后启动模板引擎， 渲染chart其他部分，并上传k8s。

<br>

**CRD的限制**

不像大部分k8s对象，CRD是全局安装的。因此Helm管理CRD时会采取非常谨慎的方式。 CRD受到以下限制：

- CRD从不重新安装。 如果Helm确定`crds/`目录中的CRD已经存在（忽略版本），Helm不会安装或升级。
- CRD从不会在升级或回滚时安装。Helm只会在安装时创建CRD。
- CRD从不会被删除。自动删除CRD会删除集群中所有命名空间中的所有CRD内容。因此Helm不会删除CRD。

希望升级或删除CRD的操作员应该谨慎地手动执行此操作。


<br>
<br>


### 管理chart

Using Helm to Manage Charts

`helm`工具有一些命令用来处理chart。

```
# 创建新chart
helm create mychart

# 打包
helm package mychart

# 格式信息
helm lint mychart
```


<br>
<br>


### 仓库

Chart Repositories

当`helm`用来管理本地chart目录时， 共享chart时，首选的机制就是使用chart仓库。

仓库的主要特征存在一个名为`index.yaml`的特殊文件，文件中包含仓库提供的包的完整列表， 以及允许检索和验证这些包的元数据。

在客户端，仓库使用`helm repo`命令管理。然而，Helm不提供上传chart到远程仓库的工具。 这是因为这样做会给执行服务器增加大量的必要条件，也就增加了设置仓库的障碍。


<br>
<br>


### Starter Packs

`helm create`命令可以附带一个可选的`--starter`选项指定一个starter chart。Starter就只是普通chart，但是被放置在`$XDG_DATA_HOME/helm/starters`。



<br>
<br>



## Hooks

Chart Hooks: <https://helm.sh/docs/topics/charts_hooks/>

Helm提供了一个hook机制，使chart开发者在发行版(release)生命周期的特定点进行干预。你可以使用hooks做以下事情：

- 安装过程中，在chart载入之前载入configmap或secret。
- 在安装一个新chart之前，执行一个作业(job)来备份数据库，然后执行第二个作业还原数据库。
- 在删除一个release之气，运行一个作业，在移除之前，来优雅地取出服务轮询。

hooks工作像常规模板，但它们有特殊的注释(写在annotations下)，因此helm可以不同地使用它们。本章节，我们将介绍hooks的基本使用模式。

```yaml
annotations:
  "helm.sh/hook": post-install
```


<br>
<br>


### 可用的hooks

| Annotation | Value | Description |
|  -  |  -  |  -  |
| `pre-install` |  -  | 模板渲染之后执行，但在k8s创建任何资源之前 |
| `post-install` |  -  | 所有资源载入k8s后执行 |
| `pre-delete` |  -  | 在从k8s删除任意资源前，执行一个删除请求 |
| `post-delete` |  -  | 在所有release的资源被删除后，执行一个删除请求 |
| `pre-upgrade` |  -  | 在模板渲染后，执行一个升级请求，但在任意资源升级之前 |
| `post-upgrade` |  -  | 在所有资源都升级后，执行一个升级 |
| `pre-rollback` |  -  | 在模板渲染后，执行一个回滚请求，但在任意资源回滚前 |
| `post-rollback` |  -  | 在所有资源都被修改后，执行一个回滚请求 |
| `test` |  -  | 当heml test子命令调用时执行 |




<br>
<br>




## 测试

Chart Tests: <https://helm.sh/docs/topics/chart_tests/>

chart包含许多k8s资源和协同工作的组件。作为包作者，你可能想编写一个测试，来验证包安装时是否如预期那样工作。

helm chart中的测试位于`templates/`目录下，是一个作业(job)定义，指定一个容器运行特定的命令。容器成功退出(exit 0)，被认为测试成功。作业定义必须包含`helm.sh/hook: test`的注释。

示例测试：

- 验证`values.yaml`文件被正确配置
- 验证服务、负载均衡正常
- 等等

可在helm中运行预定义测试，在release上使用`helm test <RELEASE_NAME>`命令。对于包的使用者，这是一个检测release of chart工作正常的方式。


<br>


### 示例

Example Test

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm pull bitnami/wordpress --untar

wordpress/
  Chart.yaml
  README.md
  values.yaml
  charts/
  templates/
  templates/tests/test-mariadb-connection.yaml
```

`templates/tests/test-mariadb-connection.yaml`的内容：

```yaml
{{- if .Values.mariadb.enabled }}
apiVersion: v1
kind: Pod
metadata:
  name: "{{ .Release.Name }}-credentials-test"
  annotations:
    "helm.sh/hook": test
spec:
  containers:
    - name: {{ .Release.Name }}-credentials-test
      image: {{ template "wordpress.image" . }}
      imagePullPolicy: {{ .Values.image.pullPolicy | quote }}
      {{- if .Values.securityContext.enabled }}
      securityContext:
        runAsUser: {{ .Values.securityContext.runAsUser }}
      {{- end }}
      env:
        - name: MARIADB_HOST
          value: {{ template "mariadb.fullname" . }}
        - name: MARIADB_PORT
          value: "3306"
        - name: WORDPRESS_DATABASE_NAME
          value: {{ default "" .Values.mariadb.db.name | quote }}
        - name: WORDPRESS_DATABASE_USER
          value: {{ default "" .Values.mariadb.db.user | quote }}
        - name: WORDPRESS_DATABASE_PASSWORD
          valueFrom:
            secretKeyRef:
              name: {{ template "mariadb.fullname" . }}
              key: mariadb-password
      command:
        - /bin/bash
        - -ec
        - |
          mysql --host=$MARIADB_HOST --port=$MARIADB_PORT --user=$WORDPRESS_DATABASE_USER --password=$WORDPRESS_DATABASE_PASSWORD
  restartPolicy: Never
{{- end }}
```

<br>

运行测试:

```sh
helm install quirky-walrus wordpress --namespace default

helm test quirky-walrus
```

<br>

**注意：**

- 你可以在`templates/`目录下定义许多测试
- 你可以嵌套你的测试`<chart-name>/templates/tests/`
- 一个测试就是一个helm hook



<br>
<br>



## Library

Library Charts: <https://helm.sh/docs/topics/library_charts/>

A library chart is a type of Helm chart，定义chart可通过helm模板在其它charts中共享。




<br>
<br>




## 完整性校验

Helm Provenance and Integrity: <https://helm.sh/docs/topics/provenance/>

helm有来源工具，帮助chart user验证包的来源和完整性。使用基于行业标准的PIK, GnuPG等备受推崇的包管理器，Helm 可以生成和验证签名文件。

<br>

```
# 生成
helm package --sign ...
helm package --sign --key 'John Smith' --keyring path/to/keyring.secret mychart

# 校验
helm install --verify
helm verify mychart-0.1.0.tgz
helm install --generate-name --verify mychart-0.1.0.tgz
```




<br>
<br>




## 仓库

Chart Repository: <https://helm.sh/docs/topics/chart_repository/>

官方的chart repo由[Kubernetes Charts](https://github.com/helm/charts)项目维护。欢迎各位参与。Helm也使得创建和运行自己的chart repo变得很容易。


<br>


### 创建仓库

Create a chart repository: <https://helm.sh/docs/topics/chart_repository/>

一个chart repo是一个HTTP服务器，它容纳了一个`index.yaml`文件和一些包。当你准备好分享你的charts，方法是将它们上传到一个chart repository。你可以使用GCS, S3, GitHub Pages等来创建你自己的web服务器。



<br>
<br>




## 注册中心

Registries: <https://helm.sh/docs/topics/registries/>

Helm 3 支持OCI用于包分发。 Chart包可以通过基于OCI的注册中心存储和分发。

```
# 激活对OCI的支持
export HELM_EXPERIMENTAL_OCI=1


# 运行一个注册中心
docker run -dp 5000:5000 --restart=always --name registry registry


# 认证
htpasswd -cB -b auth.htpasswd myuser mypass
docker run -dp 5000:5000 --restart=always --name registry \
  -v $(pwd)/auth.htpasswd:/etc/docker/registry/auth.htpasswd \
  -e REGISTRY_AUTH="{htpasswd: {realm: localhost, path: /etc/docker/registry/auth.htpasswd}}" \
  registry


# 登录
helm registry login -u myuser localhost:5000


# 注销
helm registry logout localhost:5000


# 保存
helm chart save mychart/ localhost:5000/myrepo/mychart:2.7.0

# 查看
helm chart list

# 导出
helm chart export localhost:5000/myrepo/mychart:2.7.0

# 推送到远程
helm chart push localhost:5000/myrepo/mychart:2.7.0

# 从缓存中移除
helm chart remove localhost:5000/myrepo/mychart:2.7.0

# 从远程拉取
helm chart pull localhost:5000/myrepo/mychart:2.7.0
```

<br>

使用上述命令存储的chart会被缓存到文件系统中。OCI 镜像设计规范 严格遵守文件系统布局的。如：

```
tree ~/Library/Caches/helm/
└── registry
    ├── cache
    │   ├── blobs
    │   │   └── sha256
    │   │       ├── 1b251d38cfe948dfc0a5745b7af5ca574ecb61e52aed10b19039db39af6e1617
    │   │       ├── 31fb454efb3c69fafe53672598006790122269a1b3b458607dbe106aba7059ef
    │   │       └── 8ec7c0f2f6860037c19b54c3cfbab48d9b4b21b485a93d87b64690fdb68c2111
    │   ├── index.json
    │   ├── ingest
    │   └── oci-layout
    └── config.json
```



<br>
<br>



## 架构

Helm Architecture: <https://helm.sh/docs/topics/architecture/>

介绍Helm在高级别的架构。


<br>


### 目的

The Purpose of Helm

Helm是管理称为chart的k8s包的工具。Helm可以做以下事情：

- 从头开始创建一个新的charts
- packages charts为归档(tgz) chart文件
- 与chart repo交互，并存储在那
- 安装和卸载charts到k8s集群
- 管理已安装的charts的发行版

对于Helm，有三个重要的概念：

- chart是创建一个k8s应用实例所需的信息束
- config包含配置信息，可以合并到package chart来创建一个可发行的对象
- release是一个运行的chart实例，包含特定的配置


<br>
<br>


### 组件

Components

Helm被实现为两个不同部分来执行：

Helm CLI客户端，负责以下事情：

- 本地chart开发
- 管理repo
- 管理release
- 与Helm Library接口
  - 发送chart安装
  - 请求升级或卸载releases

Helm Library提供了执行所有helm操作的逻辑。与k8s API接口交互，并提供以下功能：

- 组合chart和配置来构建一个release
- 安装chart到k8s，并提供release对象
- 通过与k8s交互，升级和卸载chart


<br>
<br>


### 实现

Implementation

Helm client和library由go编写。library使用k8s client与k8s集群通信。目前，library使用`REST+JSON`。它存储信息在k8s内的secrets里，不需要自己的数据库。配置文件以YAML编写。




<br>
<br>




## 高级技术

Advanced Helm Techniques: <https://helm.sh/docs/topics/advanced/>


<br>


### 后置渲染

Post Rendering


<br>
<br>


### GO SDK


<br>
<br>


### 后端存储

Storage backends




<br>
<br>




## RBAC

Role-based Access Control: <https://helm.sh/docs/topics/rbac/>

k8s rbac: <https://kubernetes.io/docs/reference/access-authn-authz/rbac/>

介绍Helm如何与k8s RBAC进行交互。

在k8s中，授权角色给特定用户或应用的服务账号(service account)，以确保应用程序的操作在特定范围内。从k8s v1.6开始，RBAC默认启用。

使用RBAC，你可以：

- 授权特权操作给管理员
- 限制用户在特定命名空间/集群范围创建资源的能力
- 限制用户在特定命名空间/集群范围内查看资源的能力


<br>


### 管理用户账户

Managing user accounts

所有的k8s集群有两种类型的用户：

- service accounts managed by Kubernetes
- normal users

普通用户假定由外部进行管理，独立的服务。管理员分发私钥，用户存储密码，甚至是用户名密码列表这样的文件。在这方面，k8s不具有代表普通用户账户的对象。普通用户无法通过API调用被添加到集群。

相比之下，服务账号是由k8s API管理的用户。它们被绑定到特定的命名空间，通过API server自动创建，或通过API调用手动创建。服务账号绑定在一组凭据里，存储为secret，它被挂载到pod，允许集群内进程与k8s API进行交谈。

API请求被绑定到任何一个用户（普通用户、服务账号），或者被视为匿名请求。这意味着集群内或集群外的每一个进程，从工作站上输入`kubectl`的人类用户，到节点上的kubelets，到控制面板的成员，在进行请求API server时必须进行认证，或被视为匿名用户。


<br>
<br>


### 角色、集群角色、角色绑定、集群角色绑定

Roles, ClusterRoles, RoleBindings, and ClusterRoleBindings

在k8s中，用户账户和服务账户只能够根据授权访问来查看和修改资源。这种授权是通过使用**角色(Roles)**和**角色绑定(RoleBindings)**。角色和角色绑定被绑定到特定的命名空间，它通过角色提供授权，授予用户在此命名空间内查看或修改资源的能力。

在集群范围内，这些被称为**集群角色(ClusterRoles)**和**集群角色绑定(ClusterRoleBindings)**。授权用户集群角色，允许它们访问和修改整个集群的资源。这也需要查看和修改集群范围(命名空间，资源配额，节点)的资源。

集群角色可通过角色绑定的引用来绑定到特定的命名空间。`admin`, `edit`, `view`是最常使用的默认集群角色。

k8s有一些默认的集群角色可用，它们的本意是面向用户的角色。它们包含超级角色(`cluster-admin`)，和细粒度访问的角色(`admin`, `edit`, `view`)。

<br>

| Default ClusterRole | Default ClusterRoleBinding | 描述 |
|  -  |  -  |  -  |
| cluster-admin | system:masters group  | 允许超级用户访问对任意资源执行任意动作。 |
| admin | None | 允许管理员访问，在命名空间内使用角色绑定来授权。如读写命名空间内的大部分资源，包括在命名空间内创建角色和角色绑定的能力。但不允许对资源配额或命名空间进行写操作。 |
| edit | None | 允许在命名空间内读取大多数对象的权限，不允许查看或修改角色和角色绑定 |
| view | None | 允许在命名空间内查看大多数对象的权限。不允许查看角色和角色绑定。不允许查看secrets。 |


<br>
<br>

### 限制用户账户使用RBAC访问

Restricting a user account access using RBAC

现在让我们了解基于角色的访问控制的基础知识，让我们讨论管理员如何限制用户的访问范围。

<br>

**示例：授予用户命名空间范围的读写权限**

Grant a user read/write access to a particular namespace

要限制用户对特定命名空间的读写权限，可以使用`edit`或`admin`角色。

此外，你还可以使用`cluster-admin`来创建一个角色绑定。授予在命名空间范围内的`cluster-admin`来提供在此命名空间内完整控制资源的权限，包含命名空间自身。

```
# 创建ns
kubectl create namespace foo

#创建RoleBinding
kubectl create rolebinding sam-edit
    --clusterrole edit \
    --user sam \
    --namespace foo
```

<br>

**示例：授予用户集群范围的读写权限**

Example: Grant a user read/write access at the cluster scope

如果用户希望安装chart，在集群范围内安装集群资源（ns, roles, crd...），它们将需要集群范围的写权限。要这样做，授予用户`admin`或`cluster-admin`角色权限。

```
kubectl create clusterrolebinding sam-view
    --clusterrole view \
    --user sam


kubectl create clusterrolebinding sam-secret-reader
    --clusterrole secret-reader \
    --user sam
```

<br>

**示例：授予用户命名空间范围的只读权限**

Example: Grant a user read-only access to a particular namespace

你可能注意到了，没有查看secret的集群角色。`view`集群角色没有授予用户访问secret的权限。然而，Helm默认将release metadata存储为secret。

为了使用户运行`helm list`，它需要读取这些secrets。为此，我们将创建一个特殊的`secret-reader`集群角色。

```yaml
# cluster-role-secret-reader.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

```
kubectl create -f clusterrole-secret-reader.yaml

kubectl create namespace foo

kubectl create rolebinding sam-view
    --clusterrole view \
    --user sam \
    --namespace foo


kubectl create rolebinding sam-secret-reader
    --clusterrole secret-reader \
    --user sam \
    --namespace foo
```

<br>

**示例：授予用户集群范围的只读权限**

Example: Grant a user read-only access at the cluster scope

如果用户想运行`helm l ist --all-namespaces`命令，API需要用户拥有集群范围内的读权限。

```
kubectl create clusterrolebinding sam-view
    --clusterrole view \
    --user sam

kubectl create clusterrolebinding sam-secret-reader
    --clusterrole secret-reader \
    --user sam
```



<br>
<br>



## 插件

The Helm Plugins Guide: <https://helm.sh/docs/topics/plugins/>

Helm plugin是一个可通过helm CLI访问的工具，但不是内置的helm基础代码的一部分。




<br>
<br>



## V2迁移到V3

Migrating Helm v2 to v3: <https://helm.sh/docs/topics/v2_v3_migration/>




<br>
<br>



## 弃用的k8s api

Deprecated Kubernetes APIs: <https://helm.sh/docs/topics/kubernetes_apis/>



<br>
<br>



## 版本支持

Helm Version Support Policy: <https://helm.sh/docs/topics/version_skew/>



<br>
<br>


## SQL存储后端的权限管理

Permissions management for SQL storage backend: <https://helm.sh/docs/topics/permissions_sql_storage_backend/>















<br>
<br>

---

<br>
<br>



















# 最佳实践

The Chart Best Practices Guide: <https://helm.sh/docs/chart_best_practices/>

涵盖了Helm团队对创建chart的最佳做法。它侧重于chart应该如何构造。主要关注那些可能会公开部署的charts的最佳实践。


<br>


## 一般约定

General Conventions: <https://helm.sh/docs/chart_best_practices/conventions/>


<br>


### chart名称

Chart Names

chart名称必须是小写字母和数字，可用`-`隔开。

chart目录必须与chart名称相同。

```
# 示例
drupal
nginx-lego
aws-cluster-autoscaler

nginx-lego
nginx-lego/
```


<br>
<br>


### 版本号

Version Numbers

只要有可能，Helm使用SemVer2来表示版本号。请注意，Docker image tag并不一定遵循SemVer，注意。


<br>
<br>


### 格式化YAML

Formatting YAML

YAML应该使用两个空格（别使用tab）。


<br>
<br>


### 词

Usage of the Words Helm and Chart

Helm词的一些约定：

- Helm指作为一个整体的项目
- `helm`客户端CLI
- `chart`不需要大写，它不是专有名词
- `Chart.yaml`需要大写，因为该文件名是大小写敏感的



<br>
<br>



## 值

Values: <https://helm.sh/docs/chart_best_practices/values/>

提供给你如何组织和设计chart的`values.yaml`文件，并使用你的值。


<br>


### 命名约定

Naming Conventions

变量名必须小写字母开头，使用驼峰分开：

```
chicken: true
chickenNoodleSoup: true
```

请注意，所有Helm内置变量以大写字母开头，用户可以轻松区分开:

```
.Release.Name
.Capabilities.KubeVersion
```


<br>
<br>




### 嵌套值

YAML是一种灵活的格式，值可以被深度嵌套。

```
server:
  name: nginx
  port: 80
```

```
{{ if .Values.server }}
  {{ default "none" .Values.server.name }}
{{ end }}
```


<br>
<br>






### 使类型清晰

Make Types Clear

```yaml
# YAM的类型强制规则有时是反直觉的。例如一下两者是不同的
foo: false
foo: "false"

# 避免类型转化错误的最简单的方法是要明确字符串和隐含的一切。使用引号引用字符串
# 要避免整数转换错误，将整数存储为字符串，使用以下方法来获取整数值
{{ int $value }}

# 在大多数情况下，显式类型标签被尊重。如以下1234被当作字符串
foo: !!string 1234
```




<br>
<br>


### 考虑用户如何使用你的值

Consider How Users Will Use Your Values


值有三个来源：

- `values.yaml`文件
- `helm install -f`或`helm upgrade -f`时指定的文件里
- `--set`或`--set-string`选项指定

当设计值的组织结构时，用户是希望可通过`-f`或`--set`选项来覆盖它们。YAML建议写成映射(mapping)，便于替换`--set servers.foo.port=80`。

```
servers:
  foo:
    port: 80
  bar:
    port: 81
```


<br>
<br>


### values.yaml

每个在`values.yaml`中定义的属性应该被记录(documented)。文档字符串应该用它描述的属性的名称开始，然后给出至少一个单句描述。

```yaml
# serverHost is the host name for the webserver
serverHost: example
# serverPort is the HTTP listener port for the webserver
serverPort: 9191
```



<br>
<br>





## 模板

Templates: <https://helm.sh/docs/chart_best_practices/templates/>


<br>


### templates目录架构

Structure of `templates/`

`templates/`目录应该是如下结构：

- 模板文件是`.yaml`扩展的YAML输出。`.tpl`扩展可用于未经格式化的模板文件
- 模板文件名应使用虚线(example-configmap.yaml)，而不是驼峰
- 每个资源定义应该有自己的模板文件
- 模板文件应放映资源类型（如`foo-pod.yaml`, `bar-svc.yaml`）


<br>
<br>


### 定义的模板的名称

Names of Defined Templates

定义的模板(模板文件内的`{{ define }}`)是全局访问的。这意味着，chart和它的subchart可以访问所有`{{ define }}`创建的模板。这样我想起了Pythond的模板语言（Django模板语言，Jinja2等等）。

出于此原因，所有定义的模板名称都应该命名空间。

```
{{- define "nginx.fullname" }}
{{/* ... */}}
{{ end -}}
```

It is highly recommended that new charts are created via helm create command as the template names are automatically defined as per this best practice.


<br>
<br>




### 格式化模板

Formatting Templates

模板应该使用两个空格，而不是tab。花括号前后应该有空格。有适当的空格和缩进。

```
{{ .foo }}
{{ print "foo" }}
{{- print "bar" -}}

{{ if $foo -}}
  {{- with .Bar }}Hello{{ end -}}
{{- end -}}
```


<br>
<br>






### 生成模板中的空格

Whitespace in Generated Templates

优选的是，保持在生成的模板中的空格数量降到最低。特别是，许多空行不应出现彼此相邻。但偶尔空行还是可以的。

```yaml
# This is best
apiVersion: batch/v1
kind: Job
metadata:
  name: example
  labels:
    first: first
    second: second


# This is okay
apiVersion: batch/v1
kind: Job

metadata:
  name: example

  labels:
    first: first
    second: second
```


<br>
<br>


### 注释

Comments (YAML Comments vs Template Comments)

YAML文件注释和模板注释。当一个模板记录功能时，应该使用模板注释。当Helm用户通过查看注释调试时，在模板内应该使用YANML注释。

```
# yaml 注释


{{- /*
模板注释
*/ -}}


{{- /*
mychart.shortname provides a 6 char truncated version of the release name.
*/ -}}
{{ define "mychart.shortname" -}}
{{ .Release.Name | trunc 6 }}
{{- end -}}


# This may cause problems if the value is more than 100Gi
memory: {{ .Values.maxMem | quote }}
```


<br>
<br>


### 在模板和模板输出中使用JSON

Use of JSON in Templates and Template Output

YAML是JSON的超集(superset)。在一些情况下，使用JSON语法可比其它YAML表示更具有可读性。

```
# 列表

# yaml
arguments:
  - "--dirname"
  - "/foo"

# json
arguments: ["--dirname", "/foo"]
```



<br>
<br>




## 依赖

Dependencies: <https://helm.sh/docs/chart_best_practices/dependencies/>

介绍`Chart.yaml`内声明的`dependencies`的最佳实践。


<br>


### 版本

Versions

如果可能的化，使用版本范围，而不是某个确切的版本。建议使用补丁级别(patch-level)版本匹配:

```
# >= 1.2.3, < 1.3.0
version: ~1.2.3
```

repo ruls，如果可能，使用HTTPS。文件URL(`file://...`)被认为是一个特殊，对由一个固定部署的流水线charts。


<br>
<br>


### 条件和标记

Conditions and Tags

条件或标记应被添加到任何依赖（可选的）。

```
# 条件的推荐格式
condition: somechart.enabled


# 标记
tags:
  - webaccelerator
```



<br>
<br>



## 标签和注释

Labels and Annotations: <https://helm.sh/docs/chart_best_practices/labels/>

讨论chart中使用标签和注释的最佳实践。


<br>


### 标签还是注释

Is it a Label or an Annotation?

以下条件的元数据项应该为标签(label)：

- 它利用k8s来标识此资源
- 暴露给查询系统的目的是有用的

如果元数据的条目不用于查询，它应该设置为注释。Helm hooks总是注释。


<br>
<br>


### 标准的标签

Standard Labels

下表定义了Helm chart常用的标签。Helm自身从未要求特定的标签存在。REC的标签是建议的，并应该放置到全局一致性的chart。OPT的标签是可选的。

```
名称    状态   描述
app.kubernetes.io/name  REC    这应该是app名称。通常使用{{ template "name" . }} 这由许多k8s manifests使用，不是Helm特定的
helm.sh/chart           REC  chart名称和版本: {{ .Chart.Name }}-{{ .Chart.Version }}
app.kubernetes.io/managed-by    REC    这应该始终设置为{{ .Release.Service }}
app.kubernetes.io/instance    REC    这应该为{{ .Release.Name }}，有助于在同意应用不同实例之间进行区分
app.kubernetes.io/version    OPT   应用的版本设置为{{ .Chart.AppVersion }}
app.kubernetes.io/component    OPT   This is a common label for marking the different roles that pieces may play in an application
|app.kubernetes.io/part-of    OPT   当多个charts或软件片一起使用来做一个应用
```

可在k8s 文档中，带有`app.kubernetes.io`前缀的文档中查看更多信息。



<br>
<br>






## Pods和PodTemplates

Pods and PodTemplates: <https://helm.sh/docs/chart_best_practices/pods/>

以下资源列表使用PodTemplate：

- Deployment
- ReplicationController
- ReplicaSet
- DaemonSet
- StatefulSet


<br>


### 镜像

Images

容器镜像应该使用确定的标记或镜像的SHA。但不应该使用`latest`, `head`, `canary`这样的标记。

镜像可以在`values.yaml`文件中定义，使其很容易替换镜像。

```
image: {{ .Values.redisImage | quote }}
```

镜像和标记可在`values.yaml`中被定义为分开的两个字段：

```
image: "{{ .Values.redisImage }}:{{ .Values.redisTag }}"
```


<br>
<br>


### 镜像拉取策略

ImagePullPolicy

`helm create`默认在`deployment.yaml`中设置`imagePullPolicy`为`IfNotPresent`。

```yaml
imagePullPolicy: {{ .Values.image.pullPolicy }}
```

```yaml
# values.yaml
image:
  pullPolicy: IfNotPresent
```

同样，如果未设置`impagePullPolicy`，k8s默认会将其设置为`IfNotPresent`。如果想要修改此值，只需在`values.yaml`文件中更新此值。


<br>
<br>


### PodTemplate应该声明选择器

PodTemplates Should Declare Selectors

所有的PodTemplate部分应该指定一个选择器。示例：

```yaml
selector:
  matchLabels:
      app.kubernetes.io/name: MyName
template:
  metadata:
    labels:
      app.kubernetes.io/name: MyName
```

这是一个很好的做法，因为它使set和pod相关联。

但是，这对于像Deployment这样的集更为重要。没有这一点，整个标签集(set of labels)用于选择匹配pod，如果你使用的标签发生改变（如版本或日期），这将打破匹配。



<br>
<br>



## 自定义资源的定义

Custom Resource Definitions

当使用自定义资源定义(CRDs)，区分两种不同的片是很重要的：

- 声明一个CRD(`kind: CustomResourceDefinition`)
- 然后资源使用CRD


<br>


### 使用资源前安装CRD声明

Install a CRD Declaration Before Using the Resource

Helm是尽可能优化地载入更多的资源到k8s中。按照设计，k8s可以采取一整套清单(manifests)，并带它们所有上线（这就是所谓的和解循环(reconciliation loop))）。

但是，CRDs有一些不同。对于CRD，在任意CRDs类型资源被使用之前，必须先注册声明。注册过程有时需要几秒。

<br>

**方法1：让helm为你做此事**

Method 1: Let helm Do It For You

随着Helm3的到来，出于更简单的方法，Helm移除了旧的`crd-install` hooks。这在是一个称为`crds`的新目录，在你创建的chart的此目录下保存你的CRDs。这些CRDs没有模板，但会在chart运行`helm install`时默认安装。如果CRD已存在，它会被跳过。你也可以通过传递`--skip-crds`选项来跳过CRD的安装。

**一些注意事项:**

目前不支持使用Helm更新或删除CRDs。这是一个经过反复讨论的明确的决定，由于存在非故意丢失数据的危险。此外，目前社区如何处理CRDs和它的生命周期没有共识，由于这种演变，Helm将添加对这些用例的支持。

`helm install`和`helm upgrade`的`--dry-run`选项暂不支持CRDs。Dry Run的目的是去验证chart的输出将实际地工作，如果发送到服务器。但CRDs可通过服务器行为的修改。Helm无法在dry run上安装CRD，因此发现客户端将不知道自定义资源(CR)，并验证将失败。你可以可选地移动CRDs到它们自己的chart，或使用`helm template`来代替。

围绕CRD支持的另一个重要的考虑点是如何处理模板的渲染(rendering of templates)。一个在Helm2中使用`crd-install`方法的明显的缺点是不能正确验证chart，由于改变API可用性（一个CRD被实际添加到另一个可用API到k8s集群）。如果一个chart安装了CRD，`helm`不再有一组API版本的有效集。这也是在移除从CRDs的模板支持的原因。随着安装CRD的新的`crds`方法，我们现在确保`helm`有关于当前集群状态的完整信息。

<br>

**方法2：独立chart**

Separate Charts

另一种方法是，把CRD定义在一个chart中，然后把所有资源使用的该CRD放在另一个chart。

在此方法中，每个char都必须单独安装。然而，这个工作流程可能是集群操作器(cluster operators)（对集群拥有admin权限）使用。




<br>
<br>




## RBAC

Role-Based Access Control: <https://helm.sh/docs/chart_best_practices/rbac/>

RBAC资源有：

- ServiceAccount (namespaced)
- Role (namespaced)
- ClusterRole
- RoleBinding (namespaced)
- ClusterRoleBinding


<br>


### YAML配置

RBAC和ServiceAccount配置因该在单独的密钥里。它们是不同的东西。拆分这两个概念在YAML歧义消除它们，使之更清楚。

```yaml
rbac:
  # Specifies whether RBAC resources should be created
  create: true

serviceAccount:
  # Specifies whether a ServiceAccount should be created
  create: true
  # The name of the ServiceAccount to use.
  # If not set and create is true, a name is generated using the fullname template
  name:
```

多个服务账号可以扩展为更复杂的charts。

```
someComponent:
  serviceAccount:
    create: true
    name:
anotherComponent:
  serviceAccount:
    create: true
    name:
```


<br>
<br>


### RBAC资源应该被默认创建

RBAC Resources Should be Created by Default


`rbac.create`应该是一个布尔值，由RBAC资源来控制创建。默认应该为true。希望管理RBAC访问控制的用户可以将此设置为false。


<br>
<br>


### 使用RBAC资源

Using RBAC Resources

`serviceAccount.name`应该被设置为由chart创建的访问控制资源使用的服务账号名称。如果`serviceAccount.create`为true，那么此名称的服务名称应该被创建。如果此名称未设置，则使用模板`fullname`来生成。如果为false，则它不应该被创建，但它应该与同样的资源相关联，以便创建后引用该手动创建RBAC资源正常工作。如果为false且没有指定名称，则使用默认的服务账号。

下面的助手模板应该用于服务账号：

```yaml
{{/*
Create the name of the service account to use
*/}}
{{- define "mychart.serviceAccountName" -}}
{{- if .Values.serviceAccount.create -}}
    {{ default (include "mychart.fullname" .) .Values.serviceAccount.name }}
{{- else -}}
    {{ default "default" .Values.serviceAccount.name }}
{{- end -}}
{{- end -}}
```









<br>
<br>

---

<br>
<br>












# 模板

Chart Template: <https://helm.sh/docs/chart_template_guide/>

Helm‘s chart templates，重点介绍模板语言。让我想起的Django模板语言、Jinja2模板语言。

模板生成清单文件，这是k8s可理解的YAML格式的资源描述。本章重点介绍以下概念：

- Helm模板语言
- Values使用
- 使用模板的技术


<br>


## 入门

Getting Started: <https://helm.sh/docs/chart_template_guide/getting_started/>

创建一个chart并添加一个模板。


<br>


### Charts

```
mychart/
  Chart.yaml
  values.yaml
  charts/
  templates/
  ...
```

`templates/`目录存放模板文件。当Helm评估一个chart，它会发送所有模板目录中的文件到模板渲染引擎。然后，它收集模板的结果，并将它们发送到k8s。

`values.yaml`文件对模板也很重要。此文件包含了一个chart的默认值。默认值可通过命令行选项进行覆盖。

`Chart.yaml`文件包含对chart包的描述信息。你可在模板中访问它。`charts/`目录可能包含其它chats(称为subcharts)。


<br>
<br>


### 示例

A Starter Chart

```
# 创建一个名为mychart的chart包
helm create mychart
Creating mychart


# 目录结构
tree ./mychart -L 2
./mychart
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl    #模板助手，你可以重新使用整个chart
│   ├── hpa.yaml    #
│   ├── ingress.yaml
│   ├── NOTES.txt    #chart包的帮助文本(help text)，会在运行helm install显示
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
└── values.yaml


# 创建自己的模板
rm -rf mychart/templates/*
```

当编写生产环境的chart包时，有这些charts包的基础版本可能很有用。


<br>
<br>


### 第一个模板

A First Template

创建一个`ConfigMap`资源的模板。由于它是一个基本的资源，因此它为我们提供了一个很好的起点。

```yaml
# mychart/templates/configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configpmap
data:
  myvalue: "Hello World"
```

小技巧：模板名称不遵循严格的命名模式。然而，我们建议为YAML文件使用`.yaml`后缀，为模板助手使用`.tpl`后缀。

上述YAML文件是一个最基本的ConfigMap，最有最小的必要的字段。它会通过模板引擎进行发送。

一个普通的平YAML文件是蛮好的。当Helm读取此模板，它会简单地将文件原样发送给k8s。

在这个简单的例子中，我们现在有了一个可安装的chart包。安装一下：

```
helm install full-coral mychart
NAME: full-coral
LAST DEPLOYED: Sun Sep 27 10:38:03 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None


helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
full-coral      default         1               2020-09-27 10:38:03.546664865 +0800 CST deployed        mychart-0.1.0   1.16.0


helm get manifest full-coral
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Hello World"


kubectl get configmap
NAME                DATA   AGE
mychart-configmap   1      9m47s


# 卸载
helm uninstall full-coral
```

<br>

**添加一个简单的模板调用**

Adding a Simple Template Call

硬编码的`name`，通常被认为是不好的做法。每个发行版的名称应该是唯一的。因此，我们可能将生成一个名称字段来写入发行版名称。

注意，由于DNS系统的限制，`name`字段被限制为63字符。出于这个原因，发行版名称被限制为53字符。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
```

```
# 模板指令放置于{{ xxx }} 块内
helm install clunky-serval mychart/
NAME: clunky-serval
LAST DEPLOYED: Sun Sep 27 11:16:20 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None


helm get manifest clunky-serval
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: clunky-serval-configmap
data:
  myvalue: "Hello World"


# 可使用--debug查看详情
# 下面将渲染模板，返回渲染输出，不会真正安装
helm install --debug --dry-run goodly-guppy ./mychart
```

使用`--dry-run`将更容易对代码进行测试，但它不会保证k8s会接受你生成的模板。


<br>
<br>



## 内置对象

Built-in Objects: <https://helm.sh/docs/chart_template_guide/builtin_objects/>

对象动模板引擎传递到模板。你的代码可以传递对象范围（如`with`和`range`）。有一些方法可在模板中创建新的对象，如`tuple`函数。

对象可以很简单，它只有一个值。它们也可以包含其它对象或函数。如，`Realease`对象可包含几个对象（如`Release.Name`），`Files`对象有一些函数。

```
- `Release`对象
  - `Release.Name`
  - `Release.Namespace`
  - `Release.IsUpgrade`
  - `Release.IsInstall`
  - `Release.Revision`
  - `Release.Service`：在Helm中，总是Helm
- `Values`: `values.yaml`中传递给模板的值
- `Chart`: `Chart.yaml`文件内容
- `Files`: 访问chart包中非模板的文件
  - `Files.Get`: 通过名称生成文件的函数
  - `Files.GetBytes`
  - `Files.Glob`: 返回文件为列表的函数
  - `Files.Lines`: 一行行读取文件的函数
  - `Files.AsSecrets`: 返回文件内容为base64编码字符串的函数
  - `Files.AsConfig`: 返回文件内容为YAML map的函数
- `Capabilities`
  - `Capabilities.APIVersions`
  - `Capabilities.APIVersions.Has $version`
  - `Capabilities.KubeVersion`, `Capabilities.KubeVersion.Version`
  - `Capabilities.KubeVersion.Major`
  - `Capabilities.KubeVersion.Minor`
- `Template`
  - `Template.Name`
  - `Template.BasePath`
```

内置的值总以大写字母开头。这与go命名方式保持一致。


<br>
<br>


### 值文件

Values Files: <https://helm.sh/docs/chart_template_guide/values_files/>

`Values`是一个内置的对象。它提供了访问值并传递到chart包。值文件是平YAML文件。其内容来源于多个源：

- chart包中的`values.yaml`文件
- 如果是一个subchart包，则为parent chart包的`values.yaml`文件
- 通过`helm install/upgrade`的`-f myvals.yaml`传递值
- 通过`helm install/upgrade`的`--set foo=bar`选项传递值


```yaml
# values.yaml
favoriteDrink: coffee


# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favoriteDrink }}
```

```
# 渲染
helm install geared-marsupi ./mychart --dry-run --debug
HOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: geared-marsupi-configmap
data:
  myvalue: "Hello World"
  drink: coffee


# 通过命令行选项覆盖值
helm install solid-vulture ./mychart --dry-run --debug --set favoriteDrink=slurm
HOOKS:
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: solid-vulture-configmap
data:
  myvalue: "Hello World"
  drink: slurm
```

值文件也可以包含更多结构化的内容。

```yaml
# values.yaml
favorite:
  drink: coffee
  food: pizza


# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink }}
  food: {{ .Values.favorite.food }}
```

虽然结构化数据这种方式是可行的，但建议你保持值的浅度(shallow)，有利于平整。当看到subcharts包的值时，我们将看到值是如何使用树状结构命名的。


<br>
<br>


### 删除一个默认键

Deleting a default key

如果你需要从默认值删除一个键，你可以覆盖这个键的值为`null`，在这种情况下，Helm将从覆盖值得合并中移除这个键。



<br>
<br>



## 模板函数和管道

Template Functions and Pipelines: <https://helm.sh/docs/chart_template_guide/functions_and_pipelines/>

到目前为止，我们以将看到如何将信息转换为模板。但这些信息放入未修改的模板。有时候，我们希望以一种更可用的方式来转换提供的数据。

在模板指令中调用`quote`函数：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ quote .Values.favorite.drink }}
  food: {{ quote .Values.favorite.food }}
```

模板函数使用`funcationName arg1 arg2...`语法。上面的`quote .Values.favorite.drink`调用`quote`函数并传递一个参数。

Helm有超过60个可用的函数。一些通过go模板语言定义。大多数是Sprig template library的一部分。


<br>


### 管道

pipelines(`|`)

模板语言的一个强大功能就是它的管道(`|`)。管道是让几件事情依序进行的有效方式。让我们使用管道重写上面的示例：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | quote }}
  food: {{ .Values.favorite.food | quote }}
```

使用管道，我们可以将多个函数链接在一起：

```yaml
drink: {{ .Values.favorite.drink | repeat 5 | quote }}
food: {{ .Values.favorite.food | upper | quote }}

#drink: "coffeecoffeecoffeecoffeecoffee"
#food: "PIZZA"
```


<br>
<br>


### default函数

`default`函数经常在模板中使用(`default DEFAULT_VALUE GIVEN_VALUE`)。此函数允许你指定一个默认值。有则替换它，无则使用默认值。

```yaml
drink: {{ .Values.favorite.drink | default "tea" | quote }}
```

在实际的chart包中，所有静态默认值都应该位于`values.yaml`中，而不应该使用`default`重复。然而，`default`命令对于不能在`values.yaml`中声明的值，是完美的计算值的方法。例如：

```yaml
drink: {{ .Values.favorite.drink | default (printf "%s-tea" (include "fullname" .)) }}
```


<br>
<br>


### lookup函数

`lookup`函数可用于在正在运行的集群中查找资源。它查找`apiVersion`, `kind`, `namespace`, `name`到resource或resource list。

```
# apiVersion, kind, namespace, name都是string
# name, namespace两个是可选的，可以为空进行传递


# 下列将会返回mynamespace对象的注释
(lookup "v1" "Namespace" "" "mynamespace").metadata.annotations


# 当lookup返回一个列表(list)对象时，可以通过items字段访问列表对象
{{ range $index, $service := (lookup "v1" "Service" "mynamespace" "").items }}
    {{/* do something with each service */}}
{{ end }}
```

当没有找到对象时，则返回一个空值。这可以用于检查对象是否存在。

`lookup`函数使用Helm现有的k8s连接配置来查询k8s。如果调用API server进行交互时返回错误，则Helm的模板处理将失败。

请记住，Helm是不应该在`helm template`或`helm install|update|delete|rollback --dry-run`期间连接到k8s API server，因此，`lookup`在此情况下将会获得一个空列表。


<br>
<br>


### 操作符

Operators are functions

对于模板，操作符(`eq`, `ne`, `lt`, `gt`, `and`, `or`等)都被实现为函数。在管道中，操作符可使用括号`()`进行分组。



<br>
<br>



## 函数列表

Template Function List: <https://helm.sh/docs/chart_template_guide/function_list/>

Helm包含很多模板函数，你可以在模板中使用它们。下面按照功能列出：

- Cryptographic and Security
- Date
- Dictionaries
- Encoding
- File Path
- Kubernetes and Chart
- Logic and Flow Control
- Lists
- Math
- Network
- Reflection
- Regular Expressions
- Semantic Versions
- String
- Type Conversion
- URL
- UUID



<br>
<br>



## 流程控制

Flow Control: <https://helm.sh/docs/chart_template_guide/control_structures/>

控制结构（在模板原语中称为行动）提供给模板作者，模板生成的控制流程的能力。Helm的模板语言提供了如下控制结构：

- `if`, `else`：创建条件块
- `with`：指定一个范围
- `range`： 提供一个类似的for循环

除此之外，它为声明和使用命名模板段提供了一些动作：

- `define`：在模板内声明一个新的命名模板
- `template`：导入一个命名的模板
- `block`：声明一个特殊的可填写的模板区域

这些都让我想起之前用Django模板语言写前端的时候，基本上一样的原理。


<br>


### if和else

`if`, `else`块示例：

```yaml
{{ if PIPELINE }}
  # Do something
{{ else if OTHER PIPELINE }}
  # Do something else
{{ else }}
  # Default case
{{ end }}
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{ if eq .Values.favorite.drink "coffee" }}mug: true{{ end }}
```


<br>
<br>


### 控制空格

Controlling Whitespace

虽然我们看到了条件语句，我们也应该了解模板中的空格的控制方式。这主要是确保对于生成的YAML文件的缩进的正确性。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{ if eq .Values.favorite.drink "coffee" }}
    mug: true
  {{ end }}
```

生成的不正确的YAML格式：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: eyewitness-elk-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
    mug: true
```

mug被不正确地缩进。让我们修改模板：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{ if eq .Values.favorite.drink "coffee" }}
  mug: true
  {{ end }}
```

这样生成的YAML是有效的，但显得很滑稽：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: telling-chimp-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"

  mug: true

```

请注意，在YAML文件中生成了几个空行。为什么？当模板引擎运行时，它将删除花括号里的内容，但它留下的剩余空格完全一样。

<br>

YAML对空白很在意，所以管理空白变得非常重要。幸运的是，Helm有一些工具来帮助我们。

```
# 首先，模板声明的花括号 {{ 可以使用特殊字符进行修改，来告诉模板引擎排列空白
{{- 表示空白应靠左(chomped left)
-}} 表示空白应在右边消耗(right should be consumed)
# 注意，换行也是空白（Newlines are whitespace)
```

<br>

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{- if eq .Values.favorite.drink "coffee" }}
  mug: true
  {{- end }}
```

```
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: clunky-cat-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
  mug: true
```

小心使用排列修改器(chomping modifier)。很容易不小心做了下面的事情：

```yaml
  food: {{ .Values.favorite.food | upper | quote }}
  {{- if eq .Values.favorite.drink "coffee" -}}
  mug: true
  {{- end -}}
```

这会生成`food: "PIZZA"mug:true`这样，因为它消耗了两侧的换行。

```
# 最后，有时很容易告诉模板系统如何缩进，而不是试图掌握模板指令的空格。
# 出于此原因，你有时可能会发现使用 indent函数 处理缩进是很有用的
{{ indent 2 "mug: true" }}
```

<br>
<br>


### 使用with修改范围

Modifying scope using with

另一个控制结构是`with`动作。这可以控制变量的范围，`.`是指的当前范围。因此，`.Values`告诉模板到当前范围下去寻找`Values`对象。

```yaml
# with语法和if语句类似
{{ with PIPELINE }}
  # restricted scope
{{ end }}
```

范围可以被更改。`with`可以允许你将当前范围(`.`)设置为特定对象。例如，我们使用`.Values.favorite`工作。让我们在`.Values.favorite`范围来重写ConfigMap：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  {{- end }}
```

```
# 注意，由于我们使用 with 将范围设置在了 .Values.favorite
# 所以我们使用 .drink, .food。范围在 {{ end }} 后被还原
```

但这里有一个值得注意的问题！在限制的范围内，你将无法从父对象范围(`.`)访问其它对象。以下示例会失败：

```yaml
{{- with .Values.favorite }}
drink: {{ .drink | default "tea" | quote }}
food: {{ .food | upper | quote }}
release: {{ .Release.Name }}
{{- end }}
release-2: {{ .Release.Name }}
```

由于`Release.Name`没有在限制的范围(`.`)内，会报错。但在限制的之外就没有问题。

或者，我们可以使用`$`符号从父范围访问`Release.Name`对象。`$`符号在开始执行时会映射到根范围内，在模板执行时也不会改变。示例如下：

```yaml
{{- with .Values.favorite }}
drink: {{ .drink | default "tea" | quote }}
food: {{ .food | upper | quote }}
release: {{ $.Release.Name }}
{{- end }}
```

在了解`range`后，我们会看到模板变量，它提供了一个解决上述作用域问题的方法。


<br>
<br>


### range循环

Looping with the range action

许多编程语言都是用`for`循环，在Helm模板语言中，它使用`range`操作符来实现迭代。

首先，让我们在`values.yaml`文件里添加一个列表。

```yaml
favorite:
  drink: coffee
  food: pizza
pizzaToppings:
  - mushrooms
  - cheese
  - peppers
  - onions
```

在我们的ConfigMap中获取值里面的列表：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  {{- end }}
  toppings: |-
    {{- ranage .Values.pizzaToppings }}
    - {{ . | title | quote }}
    {{- end }}
```

我们可以使用`$`来访问父范围内的`Values.pizzaToppings`。`$`符号映射到根目录下，并在函数执行时不会改变。示例如下:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- with $.Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  toppings: |-
    {{- range $.Values.pizzaToppings }}
    - {{ .| title | quote }}
    {{- end }}
  {{- end }}
```

渲染示例：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: edgy-dragonfly-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
  toppings: |-
    - "Mushrooms"
    - "Cheese"
    - "Peppers"
    - "Onions"
```

符号`|-`声明一个多行字符串。因此，实际上我们的toppings不是一个YAML list，而是一个big string。我们为什么要这样做？因为在ConfigMaps `data`里的数据是由键值对(k/v)组成，其中键和值都是简单的字符串。要理解为什么这样的化，请查看k8s configmap文档。

YAML里的`|-`符号表示一个多行字符串(multi-line string)。这可以在文件中嵌入一大块数据。

Helm模板具有一个`tuple`函数，来使得操作更简单。让我想起了Python中的tuple数据类型。示例如下:

```yaml
sizes: |-
  {{- range ruple "small" "medium" "large"}}
  - {{ . }}
  {{- end }}
```

结果如下：

```yaml
sizes: |-
  - small
  - medium
  - large
```

除了list和tuple，`range`还可以迭代具有有键值对的map和dict。我们将在后面的章节中了解它们。



<br>
<br>
<br>




## 变量

Variables: <https://helm.sh/docs/chart_template_guide/variables/>

我们可以在模板中使用变量。在Helm模板中，变量是其它对象的命名引用。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Releae.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- $relname := .Release.Name -}}
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  release: {{ $relname }}
  {{- end }}
```

在`with`块之前，我们赋值了一个变量。在`with`块内，`$relname`变量仍然指向版本名称。

在`range`循环中使用变量：

```yaml
  toppings: |-
    {{- range $index, $topping := .Values.pizzaToppings }}
      {{ $index }}: {{ $topping }}
    {{- end }
```
渲染效果：

```yaml
  toppings: |-
      0: mushrooms
      1: cheese
      2: peppers
      3: onions
```

有一个变量(`$`)它永远是全局的，此变量将永远指向根上下文(root context)。放你使用`range`循环，并且需要知道chart的版本名称时，这非常有用。示例如下：

```yaml
{{- range .Values.tlsSecrets }}
apiVersion: v1
kind: Secret
metadata:
  name: {{ .name }}
  labels:
    # Many helm templates would use `.` below, but that will not work,
    # however `$` will work here
    app.kubernetes.io/name: {{ template "fullname" $ }}
    # I cannot reference .Chart.Name, but I can do $.Chart.Name
    helm.sh/chart: "{{ $.Chart.Name }}-{{ $.Chart.Version }}"
    app.kubernetes.io/instance: "{{ $.Release.Name }}"
    # Value from appVersion in Chart.yaml
    app.kubernetes.io/version: "{{ $.Chart.AppVersion }}"
    app.kubernetes.io/managed-by: "{{ $.Release.Service }}"
type: kubernetes.io/tls
data:
  tls.crt: {{ .certificate }}
  tls.key: {{ .key }}
---
{{- end }}
```

到目前为止，我们已看到了只在一个文件中声明的模板。但是Helm模板语言的一个强大功能是声明多个模板和使用它们。我们将在后面的章节了解到。




<br>
<br>
<br>



## 命名模板

Named Templates: <https://helm.sh/docs/chart_template_guide/named_templates/>

是时候使用多个模板了。本章中，我们将在一个文件中命名模板，然后在其它地方使用它们。这让我想起了Python写Web是的模板。命名模板（有时称为子模板）是在文件中定义的一个简单的模板。有两种方法来创建它，有几种不同的方法来使用它。

在流程控制(flow control)章节，我们介绍了`define`, `template`, `block`这三个声明和管理模板的动作。在本章中，我们将讨论这三种动作，并引入一种特殊目的的`include`函数。

命名模板的一个重要细节：模板名称是全局的。如果声明了两个相同名称的模板，whichever one is loaded last will be the one used. 由于subcharts中的模板与顶级模板一起编译，你应该小心命名。

```
# 一种流行的命名约定是使用chart名作为前缀：
{{ define "mychart.labels" }}
# 通过使用特定的chart名称作为前缀，我们可以避免相同模板名称所带来的冲突
```

<br>


### 下划线文件

Partials and `_` files

目前为止，我们使用的一个文件中包含一个模板。但是Helm模板语言允许你创建命名嵌套模板，可通过名称在其它任何地方进行访问。

在我们开始编写这些模板之前，我们需要注意一下命名规范：

- `templates/`下的大多数文件被视为包含k8s manifests
- `NOTES.txt`是一个例外
- 以下划线(`_`)开头的文件被假定为不包含k8s manifests。这些文件不会被渲染为k8s对象定义，但可在任意chart templates中使用。

这些文件用来存储特定(partials)和助手(helpers)。实际上，当我们第一次创建`mychart`，我们会看到`_helpers.tpl`文件，此文件是默认的template partials。


<br>
<br>


### 声明和使用模板

Declaring and using templates with `define` and `template`。

`define`动作允许我们在一个模板文件中创建命名模板(named template)。语法如下:

```tpl
{{ define "MY.NAME "}}
  # body of template here
{{ end }}
```

栗子：

```yaml
{{- define "mychart.labels" }}
  labels:
    generator: helm
    data: {{ now | htmlDate }}
{{- end }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  {{- template "mychart.labels" }}
  data:
    myvalue: "Hello World"
    {{- range $key, $va1 := .Values.favorite }}
    {{ $key }}: {{ $va1 | quote }}
    {{- end }}
```

渲染之后的效果：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: running-panda-configmap
  labels:
    generator: helm
    date: 2016-11-02
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
```

`define`仅定义，只有在模板中调用时才会产生输出。

按照惯例，Helm charts将这些模板放在partials文件中（通常是`_helpers.tpl`），如：

```tpl
{{/* Generate basic labels */}}
{{- define "mychart.labels" }}
  labels:
    generator: helm
    date: {{ now | htmlDate }}
{{- end }}
```

<br>

```
# 按照管理，define函数 应该有一个简单的文档块 {{/*...*/}}
# 如上。然后在其它模板文件中访问它。
```


<br>
<br>


### 设置模板范围

Setting the scope of a template

在上面定义的模板中，我们没有使用任何对象。让我们做些修改：

```tpl
{{/* Generate basic labels */}}
{{- define "mychart.labels" }}
  labels:
    generator: helm
    data: {{ now | htmlData }}
    chart: {{ .Chart.Name }}
    version: {{ .Chart.Version }}
{{- end }}
```

上面定义的名称和版本是动态的，会根据不同的模板生成不同的值。

之前的引用并没有床底范围，因此在模板内我们不能使用`.`来访问任何事物。现在我们对模板加上范围:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  {{- template "mychart.labels" . }}
```

注意上面在模板调用处使用的点(`.`)。我们可以非常容易地传递`.Values`或`.Values.favorite`或任何我们需要的范围。但是，我们需要的是顶级范围。

现在运行渲染(`helm install --dry-run --debug plinking-anaco ./mychart`)来预览下：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: plinking-anaco-configmap
  labels:
    generator: helm
    date: 2016-11-02
    chart: mychart
    version: 0.1.0
```


<br>
<br>


### include

The `include` function

假设我们定义了如下一个简单模板：

```tpl
{{- define "mychart.app" -}}
app_name: {{ .Chart.Name }}
app_version: "{{ .Chart.Version }}"
{{- end -}}
```

<br>

一个错误的栗子：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  labels:
    {{ template "mychart.app" . }}
data:
  myvalue: "Hello World"
  {{- range $key, $val := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
  {{- end }}
{{ template "mychart.app" . }}
```

渲染的结果并不正确：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: measly-whippet-configmap
  labels:
    app_name: mychart
app_version: "0.1.0+1478129847"
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
  app_name: mychart
app_version: "0.1.0+1478129847"
```

Because the template that is substituted in has the text aligned to the right. Because template is an action, and not a function, there is no way to pass the output of a template call to other functions; the data is simply inserted inline.

To work around this case, Helm provides an alternative to template that will import the contents of a template into the present pipeline where it can be passed along to other functions in the pipeline.

因为模板是靠右对齐的文本，因为`template`是一个动作，不是一个函数，因此无法传递调用其它函数的`template`的输出，数据被简单的插入内联。

<br>

现在我们需要使用`ident`来告诉模板正确的缩进，栗子：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  labels:
{{ include "mychart.app" . | indent 4 }}
data:
  myvalue: "Hello World"
  {{- range $key, $va1 := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
  {{- end }}
  {{ include "mychart.app" . | indent 2 }}
```

正确的渲染结果：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: edgy-mole-configmap
  labels:
    app_name: mychart
    app_version: "0.1.0+1478129987"
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
  app_name: mychart
  app_version: "0.1.0+1478129987"
```

在Helm template中使用`include`对`template`被认为更好，这使得输出格式可以为YAML文档更好地处理。

有时候，我们想要导入内容，但不作为模板。也就是逐字导入文件。我们可以通过`.Files`对象访问文件来实现这一目标。



<br>
<br>



## 在模板内访问文件

Accessing Files Inside Templates: <https://helm.sh/docs/chart_template_guide/accessing_files/>

Helm提供了`.Files`对象来访问文件。在开始模板示例之前，有些事需要注意下：

- 可以添加额外的文件到Helm chart。这些文件将被捆绑。要注意，charts必须小于1MB，因为k8s对象的存储限制。
- 某些文件无法通过`.Files`对象访问，通常出于安全原因
  - `templates/`目录内的文件无法访问
  - `.helmignore`中包含的文件无法访问
- Charts不保留UNIX mode信息，当设计到`.Files`对象时，文件级别的权限对一个文件的可用性没有影响。


<br>


### 示例

Basic example

添加三个位于`mychart/`目录下的文件。

```
# config1.toml
message = Hello from config 1

# config1.tom2
message = Hello from config 2

# config1.tom3
message = Goodbye from config 3
```

在模板中访问文件：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  {{- $files := .Files }}
  {{- range tuple "config1.tom1" "config2.toml" "config3.toml" }}
  {{ .}}: |-
    {{ $files.Get .}}
  {{- end }}
```

<br>

```
# 首先，创建了一个 $files变量 来保存.Files对象的引用
# 我们同样使用 tuple函数来创建循环的文件列表
# 接着打印每个文件名 {{ . }}: |- 后面接着文件内容 {{ $files.Get . }}
```

渲染效果示例：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: quieting-giraf-configmap
data:
  config1.toml: |-
    message = Hello from config 1

  config2.toml: |-
    message = This is config 2

  config3.toml: |-
    message = Goodbye from config 3
```


<br>
<br>


### 路径助手

Path helpers

使用文件时，对文件路径执行一些标准的操作是有用的。为了帮助处理，Helm从go path包中引入了许多函数供你使用:

- `Base`
- `Dir`
- `Ext`
- `IsAbs`
- `Clean`


<br>
<br>


### Glob模式

Glob patterns

随着你的chart包的增长，你会发现你有一个更大的需来组织你的文件，因此我们提供了`Files.Glob(pattern string)`方法，以帮助提取某些文件与glob patterns的所有灵活性。

`.Glob`返回一个`Files`类型，因此你可以在返回的对象上调用任意`Files`方法。

```
# 示例的目录结构
foo/:
  foo.txt foo.yaml

bar/:
  bar.go bar.conf baz.yaml
```

使用Globs的多种选项：

```
{{ $currentScope := .}}
{{ range $path, $_ :=  .Files.Glob  "**.yaml" }}
    {{- with $currentScope}}
        {{ .Files.Get $path }}
    {{- end }}
{{ end }}
```

或者：

```
{{ range $path, $_ :=  .Files.Glob  "**.yaml" }}
      {{ $.Files.Get $path }}
{{ end }}
```


<br>
<br>


### ConfigMap和Secrets的实用功能

ConfigMap and Secrets utility functions

将文件内容放置到K8s ConfigMap或Secrets中非常常见，然后在运行的时候挂载到容器。为了帮助实现此功能，我们在`Files`类型上提供了几种实用的方法：

- `AsCoinfig`
- `AsSecrets`

栗子：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: conf
data:
{{ (.Files.Glob "foo/*").AsConfig | indent 2 }}
---
apiVersion: v1
kind: Secret
metadata:
  name: very-secret
type: Opaque
data:
{{ (.Files.Glob "bar/*").AsSecrets | indent 2 }}
```


<br>
<br>


### 编码

Encoding

你可以导入一个文件，并实用base64编码来确保成功传输：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ .Release.Name }}-secret
type: Opaque
data:
  token: |-
    {{ .Files.Get "config1.toml" | b64enc }}
```

渲染后的效果：

```yaml
# Source: mychart/templates/secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: lucky-turkey-secret
type: Opaque
data:
  token: |-
    bWVzc2FnZSA9IEhlbGxvIGZyb20gY29uZmlnIDEK
```


<br>
<br>


### 行

Lines

有时候需要在模板中访问一个文件中的每行内容。我们为此提供了`Lines`方法。

示例：

```yaml
data:
  some-file.txt: {{ range .Files.Lines "foo/bar.txt" }}
    {{ . }}{{ end }}
```



<br>
<br>



## NOTES.txt

Creating a NOTES.txt File: <https://helm.sh/docs/chart_template_guide/notes_files/>

在`helm install`或`helm upgrade`结束，Helm可以为用户打印一块有用的信息。此信息使用模板且高度可定制。

要为你的chart包添加安装说明，简单地创建一个`templates/NOTES.txt`文件。此文件是纯文本文件，但它像作为模板一样处理，并可访问所有正常模板函数和对象。

`NOTES.txt`文件示例：

```
Thank you for installing {{ .Chart.Name }}

Your release is named {{ .Release.Name }}

To learn more about the release, try:
  $ helm status {{ .Release.Name }}
  $ helm get all {{ .Release.Name }}
```

接下来运行：

```
helm install rude-cardinal ./mychart

RESOURCES:
==> v1/Secret
NAME                   TYPE      DATA      AGE
rude-cardinal-secret   Opaque    1         0s

==> v1/ConfigMap
NAME                      DATA      AGE
rude-cardinal-configmap   3         0s


NOTES:
Thank you for installing mychart.

Your release is named rude-cardinal.

To learn more about the release, try:

  $ helm status rude-cardinal
  $ helm get all rude-cardinal
```

强烈建议创建`NOTES.txt`文件，以帮助用户获得chart包的有用信息。



<br>
<br>



## Subcharts

Subcharts and Global Values: <https://helm.sh/docs/chart_template_guide/subcharts_and_globals/>

之前我们只有一个chart，但charts可能会有依赖(dependencies)，称为subcharts。subcharts也有自己的值和模板。本章我们将会创建subchart，并看看我们可以从模板访问值的不同的方式。

subcharts的一些重要详情：

- 一个subchart被认为是独立的(stand-alone)，这意味着一个subchart不能明确依赖它的parent chart
- 出于此原因，subchart不能访问parent chart的值
- parent chart可以覆盖subcharts的值
- Helm有一个全局值(global values)的概念，这些全局值可被所有charts访问


<br>


### 创建一个subchart

Creating a Subchart

```sh
cd mychart/charts

helm create mysubchart

rm -rf mysubchart/templates/*.*
```


<br>
<br>


### 对subchart添加值和模板

Adding Values and a Template to the Subchart

为subchart添加一个简单的值和模板：

```yaml
# values.yaml
dessert: cake
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-cfgmap2
data:
  dessert: {{ .Values.dessert }}
```

因为每个subchart都是独立的chart，我们可以测试mysubchart：

```sh
helm install --generate-name --dry-run --debug mychart/charts/mysubchart
SERVER: "localhost:44134"
CHART PATH: /Users/mattbutcher/Code/Go/src/helm.sh/helm/_scratch/mychart/charts/mysubchart
NAME:   newbie-elk
TARGET NAMESPACE:   default
CHART:  mysubchart 0.1.0
MANIFEST:
---
# Source: mysubchart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: newbie-elk-cfgmap2
data:
  dessert: cake
```


<br>
<br>


### 从parent chart覆盖值

现在，mychart是mysubchart的parent chart。因为mychart是parent chart，我们可以在mychart中指定配置，并将配置推送到mysubchart中。

```yaml
# mychart/values.yaml
favorite:
  drink: coffee
  food: pizza
pizzaToppings:
  - mushrooms
  - cheese
  - peppers
  - onions

mysubchart:
  dessert: ice cream
```

我们在parent chart(mychart)的值文件里添加了mysubchart的值，mysubchart这部分值会发送到mysubchart包，这回覆盖mysubchart的值。

```sh
helm install --dry-run --debug mychart

# Source: mychart/charts/mysubchart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: unhinged-bee-cfgmap2
data:
  dessert: ice cream
```


<br>
<br>


### 全局值

Global Chart Values

有时候你需要将值提供给所有模板，这可以使用全局值(global chart values)。全局值可被任意chart或subchart通过相同的名称来访问。全局需要明确地声明。

值数据类型保留在称为`Values.global`的区域，此区域可以设置全局值。

```yaml
# mychart/values.yaml

favorite:
  drink: coffee
  food: pizza
pizzaToppings:
  - mushrooms
  - cheese
  - peppers
  - onions

mysubchart:
  dessert: ice cream

global:
  salad: caesar
```

<br>

```yaml
# 任意chart和subchart都可以使用 {{ .Values.global.salad }} 来访问这个值
# mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  salad: {{ .Values.global.salad }}
```

```yaml
# mysubchart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-cfgmap2
data:
  dessert: {{ .Values.dessert }}
  salad: {{ .Values.global.salad }}
```

渲染输出效果：

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: silly-snake-configmap
data:
  salad: caesar

---
# Source: mychart/charts/mysubchart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: silly-snake-cfgmap2
data:
  dessert: ice cream
  salad: caesar
```


<br>
<br>


### 共享模板

Sharing Templates with Subcharts

parent charts和subcharts可以共享模板。任意在chart中定义的block(块)都可以被其它charts所使用。

定义一个简单的模板栗子：

```yaml
{{- define "labels" }}from: mychart {{ end }}
```

尽管chart开发者可以在`include`和`template`之间选择，但使用`include`的优点是它可以动态引用模板：

```
{{ include $mytemplate }}
```

上面间接引用`$mytemplate`。`template`函数，相反它只接受一个字符串。


<br>
<br>


### 避免使用块

Avoid Using Blocks

go模板语言提供了一个`block`关键字，来允许开发者提供一个覆盖的默认实现。在Helm chart中，块(block)并不是覆盖的最佳工具，因为如果提供了相同块的多个实现，选择的那个是不可预测的。

建议使用`include`来代替。



<br>
<br>



## .helmignore

The .helmignore file: <https://helm.sh/docs/chart_template_guide/helm_ignore_file/>

`.helmignore`也就类似于`.gitignore`, `.dockerignore`，指定不需要包含在chart包中的文件。

如果此文件存在，`helm package`命令将忽略`.helmignore`里面匹配到的文件打包到应用的包里。

一个`.helmignore`文件的栗子：

```
# comment

# Match any file or path named .git
.git

# Match any text file
*.txt

# Match only directories named mydir
mydir/

# Match only text files in the top-level directory
/*.txt

# Match only the file foo.txt in the top-level directory
/foo.txt

# Match any file named ab.txt, ac.txt, or ad.txt
a[b-d].txt

# Match any file under subdir matching temp*
*/temp*

*/*/temp*
temp?
```



<br>
<br>



## 模板调试

Debugging Templates: <https://helm.sh/docs/chart_template_guide/debugging/>

有几个命令可帮助调试模板：

- `helm lint`: 验证chart最佳实践的工具
- `helm install --dry-run --debug`或`helm template --debug`：渲染模板并返回k8s manifest文件
- `helm get manifest`：查看安装了哪些模板



<br>
<br>



## YAML技巧

YAML Techniques: <https://helm.sh/docs/chart_template_guide/yaml_techniques/>





















<br>
<br>

---

<br>
<br>















# Helm命令

Helm Commands: <https://helm.sh/docs/helm/>
























<br>
<br>

---

<br>
<br>






# 社区指南

Community Guides: <https://helm.sh/docs/community/>




























<br>
<br>

---

<br>
<br>

















# FAQ

Frequently Asked Questions: <https://helm.sh/docs/faq/>









