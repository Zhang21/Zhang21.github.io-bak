---
title: Apollo
date: 2019-01-09 19:11:22
categories: DevOps
tags:
  - Apollo
  - 分布式
  - 配置中心
---

参考:

- Apollo官方文档: <https://github.com/ctripcorp/apollo/wiki>

<br>

环境:

- Apollo v1.2
- Docker v1.18
- K8s v1.11



<br/>
<br/>

<!--more-->

---

<br/>
<br/>



# 概述



基本上按照官方文档都没什么问题，说几点我在配置过程中容易出错的地方。

总的来说就是一个`portal`，多个`config+admin`，而`Eruea`注册的`Meta Server`是和`config`在一起的，每个环境的`admin`注册到对应环境的`Meta Server(config)`。

我是将其放入k8s集群中运行，所以针对官方给出的`Dockerfile`和`k8s.yaml`文件做了对应的修改。

<br/>


看一下我画的架构图和官方架构图:

![](/images/Apollo/Apollo.png)

<br>

![](/images/Apollo/apollo-deployment.png)


<br/>

**我自己画的一个Apollo项目架构图：**

![](/images/Apollo/apollo_arch.jpg)



<br/>
<br/>
<br/>



# 注意事项


- **为不同环境创建不同数据库**

官方已经给出了创建数据库的sql语句，一个`portadb`, 多个`configdb-project-env`。我们需要修改数据库名，为不同的环境建立不同的数据库，所以需要在使用官方sql的时候把数据库名修改为自定义的即可，这样创建的各个环境数据库的表结构都是一样的。


<br/>


- **配置了一个环境变量，它也就是部署服务的集群内访问地址(不在同一VPC可能需要外部访问地址)**

官方是写入了Dockerfile里面作为环境变量，我是将其写入k8s yaml中的环境变量。当然，也可以写入启动脚本中。
以下配置，随便用哪一个。

```yaml
#Dockerfile中
ENV APOLLO_CONFIG_SERVICE_NAME="{service-name}.{namespace}.svc.cluster.local"


#k8s yaml container中
          env:
            - name: APOLLO_CONFIG_SERVICE_NAME
              value: {service_name}.{namespace}.svc.cluster.local


#启动脚本
#scripts/startup-kubernetes.sh
#SERVER_URL="http://${APOLLO_ADMIN_SERVICE_NAME}:${SERVER_PORT}"
SERVER_URL="http://{service_name}.{namespace}.svc.cluster.local:${SERVER_PORT}"

```


<br/>


- **将数据库和注册地址写入`config/application-github.properties`配置文件**

官方是写入`Dockerfile`中作为环境变量，然后通过`entrypoint.sh`进行相应的替换。我直接将其写入此配置文件，并删除`entrypoint.sh`。

```yaml
spring.datasource.url = jdbc:mysql://{mysql-ip}:{mysql-port}/{mysql-db}?characterEncoding=utf8
spring.datasource.username = user
spring.datasource.password = passwd
eureka.service.url = http://{service-name}.{namespace}.svc.cluster.local:8080/eureka/
# 如果环境跨VPC，还需要指定公网地址的 HomePageUrl
# eureka.instance.homePageUrl = http://ELB:PORT
# 或 在启动命名指定: -Dapollo.configService=http://config-service的公网IP:端口来跳过meta service的服务发现
# 如果不指定的话，则默认使用获取的内部地址，这无法正常访问
```


<br/>


- **portal服务的默认环境是DEV，请注意**

如果配置的第一个环境并不是DEV，请记得先修改数据库中的这个值，不然portal读取config, admin会失败。
`portaldb.serverconfig`的`apollo.portal.envs`这个key，多个环境使用`,`分割，后面可以在UI上配置。其它环境请修改为其它环境名。

```sql
UPDATE serverconfig SET Value='uat' WHERE Key='apollo.portal.envs';

```


```yaml
config/apollo-env.properties


#dev.meta=http://DEV_META_SERVICE_NAME:8080
#fat.meta=http://TEST_ALPHA_META_SERVICE_NAME:8080
#uat.meta=http://TEST_BETA_META_SERVICE_NAME:8080
#pro.meta=http://PROD_META_SERVICE_NAME:8080

#某个环境的config
uat.meta=http://{service-name}.{namespace}.svc.cluster.local:8080

```


<br/>


- **将日志输出到标准输出`/dev/stdout`**

由于我是运行在容器中，所以需要将日志输出到标准输出。
这里很奇怪，直接在主机(centos7)上运行`./apollo-xxx.jar start`，日志正常输出到console；在openjdk容器里运行，`./apollo-portal.jar  start`它放到后台运行，日志不输出到console，使用`java ${JAVA_OPTS} -jar ./"${SERVICE_NAME}.jar"`日志能够正常输出到console。
所以这里修改`startup-kubernetes.sh`启动命令:

```sh
# 原
# export JAVA_OPTS="$JAVA_OPTS -Dserver.port=$SERVER_PORT -Dlogging.file=$LOG_DIR/$SERVICE_NAME.log -Xloggc:$LOG_DIR/gc.log -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=5M -XX:HeapDumpPath=$LOG_DIR/HeapDumpOnOutOfMemoryError/"

# 修改
export JAVA_OPTS="$JAVA_OPTS -Dserver.port=$SERVER_PORT -Xloggc:$LOG_DIR/gc.log -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=5M -XX:HeapDumpPath=$LOG_DIR/HeapDumpOnOutOfMemoryError/"


# 原
# ./$SERVICE_NAME".jar" start

# 修改
# 因为 xxx.jar start也是调用 java $opt_java -jar xxx.jar
java ${JAVA_OPT} -jar ./"${SERVICE_NAME}.jar"
```



<br/>


- **登录Web UI后可修改配置**

如组织里面的部门名，管理员等等参数，在系统参数里面更新`Key`对应的`Value`。
具体这个Key可参考文档 —— [调整服务端配置](https://github.com/ctripcorp/apollo/wiki/%E5%88%86%E5%B8%83%E5%BC%8F%E9%83%A8%E7%BD%B2%E6%8C%87%E5%8D%97#213-%E8%B0%83%E6%95%B4%E6%9C%8D%E5%8A%A1%E7%AB%AF%E9%85%8D%E7%BD%AE)


<br/>

- **每个环境下有多个config/admin**

请注意，每个环境只有一个数据库，也就是这几个服务都要连接同一个configdb，这点请注意。
后面还需要修改ConfigDB.ServerConfig表，每个环境的都需要单独配置。这里的`eureka.service.url`  - Eureka服务Url要来修改。

> ps: 官方文档
> 不管是apollo-configservice还是apollo-adminservice都需要向eureka服务注册，所以需要配置eureka服务地址。 按照目前的实现，apollo-configservice本身就是一个eureka服务，所以只需要填入apollo-configservice的地址即可，如有多个，用逗号分隔（注意不要忘了/eureka/后缀）。
> 需要注意的是每个环境只填入自己环境的eureka服务地址，比如FAT的apollo-configservice是1.1.1.1:8080和2.2.2.2:8080，UAT的apollo-configservice是3.3.3.3:8080和4.4.4.4:8080，PRO的apollo-configservice是5.5.5.5:8080和6.6.6.6:8080，那么：
> 在FAT环境的ApolloConfigDB.ServerConfig表中设置eureka.service.url为：
> http://1.1.1.1:8080/eureka/,http://2.2.2.2:8080/eureka/
> 在UAT环境的ApolloConfigDB.ServerConfig表中设置eureka.service.url为：
> http://3.3.3.3:8080/eureka/,http://4.4.4.4:8080/eureka/
> 在PRO环境的ApolloConfigDB.ServerConfig表中设置eureka.service.url为：
> http://5.5.5.5:8080/eureka/,http://6.6.6.6:8080/eureka/

更过详情请查看官方文档！

<br>


假如UAT环境下有:

- config-cd, config-bj, config-gz
- admin-cd, admin-bj, admin-gz

他们需要向这三个Eureka(cd, bj, gz)注册。

之后查看config:8080地址，便可以看到这几个地址的信息；在Portal集群信息里面，也可看到有三个CONFIG，三个ADMIN。



<br/>
<br/>

---

<br/>
<br/>



# 栗子


具体信息可以查看官方文档，然后根据官方文档给出的栗子做一些修改。


<br>


## apollo-config


```sh
tree .

.
├── apollo-configservice.conf
├── apollo-configservice.jar
├── config
│   ├── application-github.properties
│   └── app.properties
├── Dockerfile
├── k8s-apollo-config-dev.yaml
├── README.md
└── scripts
    └── startup-kubernetes.sh

```

<br>

**Dockerfile**

```
FROM openjdk:8-jre-alpine3.8

RUN \
    echo "http://mirrors.aliyun.com/alpine/v3.8/main" > /etc/apk/repositories && \
    echo "http://mirrors.aliyun.com/alpine/v3.8/community" >> /etc/apk/repositories && \
    apk update upgrade && \
    apk add --no-cache procps curl bash tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone && \
    mkdir -p /apollo-config-server

COPY . /apollo-config-server/

EXPOSE 8080

CMD ["/apollo-config-server/scripts/startup-kubernetes.sh"]
```

<br>

**config/**

```
# app.properties不用修改
#  application-github.properties
# DataSource
spring.datasource.url = jdbc:mysql://host:port/configdbtest?characterEncoding=utf8
spring.datasource.username = user
spring.datasource.password = passwd
eureka.service.url = http://xxx.svc.cluster.local:8080/eureka/
# 扩公网的话请修改 HomePageUrl
# config-web-test service homepage ELB
# eureka.instance.homePageUrl = http://ELB:PORT
```

<br/>

**scripts/**

`startup-kubernetes.sh` 这个文件，文件名你也可以随便修改。
有些值既可以写在这里面，也可以配置成环境变量(Dockerfile， 或k8s yaml)
这里把 `APOLLO_CONFIG_SERVICE_NAME` 配置成 k8s yaml 里的环境变量 `xxx.apollo-test.svc.cluster.local`(即k8s service)。

```bash
#!/bin/bash
SERVICE_NAME=apollo-configservice
## Adjust log dir if necessary
LOG_DIR=/opt/logs/apollo-config-server
## Adjust server port if necessary
SERVER_PORT=8080

SERVER_URL="http://${APOLLO_CONFIG_SERVICE_NAME}:${SERVER_PORT}"

## Adjust memory settings if necessary
#export JAVA_OPTS="-Xms6144m -Xmx6144m -Xss256k -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=384m -XX:NewSize=4096m -XX:MaxNewSize=4096m -XX:SurvivorRatio=8"

## Only uncomment the following when you are using server jvm
#export JAVA_OPTS="$JAVA_OPTS -server -XX:-ReduceInitialCardMarks"

########### The following is the same for configservice, adminservice, portal ###########
export JAVA_OPTS="$JAVA_OPTS -XX:+UseParNewGC -XX:ParallelGCThreads=4 -XX:MaxTenuringThreshold=9 -XX:+UseConcMarkSweepGC -XX:+DisableExplicitGC -XX:+UseCMSInitiatingOccupancyOnly -XX:+ScavengeBeforeFullGC -XX:+UseCMSCompactAtFullCollection -XX:+CMSParallelRemarkEnabled -XX:CMSFullGCsBeforeCompaction=9 -XX:CMSInitiatingOccupancyFraction=60 -XX:+CMSClassUnloadingEnabled -XX:SoftRefLRUPolicyMSPerMB=0 -XX:+CMSPermGenSweepingEnabled -XX:CMSInitiatingPermOccupancyFraction=70 -XX:+ExplicitGCInvokesConcurrent -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCApplicationConcurrentTime -XX:+PrintHeapAtGC -XX:+UseGCLogFileRotation -XX:+HeapDumpOnOutOfMemoryError -XX:-OmitStackTraceInFastThrow -Duser.timezone=Asia/Shanghai -Dclient.encoding.override=UTF-8 -Dfile.encoding=UTF-8 -Djava.security.egd=file:/dev/./urandom"

#官方默认是这个: export JAVA_OPTS="$JAVA_OPTS -Dserver.port=$SERVER_PORT -Dlogging.file=$LOG_DIR/$SERVICE_NAME.log -Xloggc:$LOG_DIR/gc.log -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=5M -XX:HeapDumpPath=$LOG_DIR/HeapDumpOnOutOfMemoryError/"

# 这里我修改，不将GC信息输出到console
export JAVA_OPTS="$JAVA_OPTS -Dserver.port=$SERVER_PORT -Xloggc:$LOG_DIR/gc.log -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=5M -XX:HeapDumpPath=$LOG_DIR/HeapDumpOnOutOfMemoryError/"


printf "$(date) ==== Starting ==== \n"

cd `dirname $0`/..
chmod 755 $SERVICE_NAME".jar"

# 官方默认这样启动
# ./$SERVICE_NAME".jar" start
# 这样启动在主机上可以输出日志到console，但openjdk容器里不行，所以修改如下
java ${JAVA_OPTS} -jar ./$SERVICE_NAME".jar"


rc=$?;

if [[ $rc != 0 ]];
then
    echo "$(date) Failed to start $SERVICE_NAME.jar, return code: $rc"
    exit $rc;
fi

tail -f /dev/null
```

<br/>

**apollo-configservice.conf**

这个也不需要做什么修改，看个人情况。

```
MODE=service
PID_FOLDER=.
LOG_FOLDER=/opt/logs/apollo-config-server
```



<br/>
<br/>
<br/>



## apollo-admin


这个其实和 `apollo-config` 差不多配置，只是修改一些配置项。




<br/>
<br/>
<br/>



## apollo-portal


这个也和上面差不多，只是修改一些配置项。







