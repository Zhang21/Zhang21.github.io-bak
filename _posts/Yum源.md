---

title: Yum源
date: 2018-01-09 11:46:21
tags: Yum
categories: Linux

---

参考：

- [CentOS 7下配置本地yum源及yum客户端](http://blog.csdn.net/leshami/article/details/78133716)
- [Centos7 配置本地源+阿里yum源/epel-yum+修改优先级](http://blog.csdn.net/conling_/article/details/70399694)
- [调整CentOS 7中yum仓库的优先级](http://blog.csdn.net/kingfox/article/details/51233153)
- [国内开源站点](https://segmentfault.com/a/1190000000375848)

<!--more-->

<br>

---

<br/>




# 国内开源镜像站点

- 网易开源镜像站：<http://mirrors.163.com/>
- 阿里云开源镜像站：<http://mirrors.aliyun.com>
- 清华大学开源镜像站：<https://mirrors.tuna.tsinghua.edu.cn/>
- 浙江大学开源镜像站： <http://mirrors.zju.edu.cn/>
- 中国科技大学开源镜像站：<http://mirrors.ustc.edu.cn/>



<br/>

---

<br>


# CentOS自带源

rpm包管理方式，对于安装、升级、卸载却难以处理包之间的依赖关系。而yum作为一个rpm包前端管理工具，可以自动处理依赖性，并支持在线现在、安装、升级、卸载rpm软件包。


CentOS默认自带`CentOS-Base.repo`源，但官方源在国外，连接速度令人心痛。并且有很多软件在默认源里面是找不到的。


## 配置网络yun源

**配置aliyun.repo：**

```
#先备份默认源
mv CentOS-Base.repo{,.bak}

#下载阿里云源替换默认源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

yum clean all
yum makecache    #重构yum缓存

yum repolist    #查看yum仓库
```



<br/>

## 配置本地yum源

**配置本地yum源，考虑到优先使用本地安装包，所以会涉及到一个优先级的概念。**

安装完毕后，就可以在yum源中添加一个优先级`priority`。

**安装yum优先级插件：**

```
yum install -y yum-plugin-priorities

#检查安装完成后配置
vim /etc/yum/pluginconf.d/priorities.conf

enable=1
#enable=0
```


<br>

**创建本地yum源：**

```
mv /etc/yum.repos.d/CentOS-Base.repo{,.bak}

vim /etc/yum.repos.d/CentOS-Local.repo

[base-Local]
name=Centos- Local
baseurl=file:///mnt/xxx
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
priority=1    #优先级为1

[updates-Local]
name=CentOS- Local
gpgcheck=0
baseurl=file:///dir/path/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
priority=1

······
#具体可参考CentOS-Base.repo
#可将aliyun源优先级写成2

yum clean all
yum makecache
```



<br>

## 配置ftp方式源
```
vim /etc/yum.repos.d/ftp.repo

[ftp-media]
name=name=CentOS-$releasever - media
baseurl=ftp://ip
gpgcheck=0
enable=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

yum clean all
yum makecache
```




<br>

---

<br/>

# 其他常见YUM源

官方的默认`yum源`提供的软件包往往是很滞后的，(可能为了服务器版本的稳定性和安全性)。并且官方默认源提供的RPM包也不够丰富。



<br>

## EPEL源

EPEL的全称叫 `Extra Packages for Enterprise Linux` 。EPEL是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上了 EPEL之后，就相当于添加了一个第三方源。

`EPEL源`为服务器提供了大量的rpm包(这些包可能有很多在默认源中没有)，并且绝大多数rpm包比官方默认源版本要新。

**添加epel源：**
epel下载地址：<http://download.fedora.redhat.com/pub/epel/>
```sh
rpm -vih http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-9.noarch.rpm

#yum install -y epel-release
```



<br>

## remi源

Remi源大家或许很少听说，不过Remi源GoFace强烈推荐，尤其对于不想编译最新版的linux使用者，因为Remi源中的软件几乎都是最新稳定版。
或许您会怀疑稳定不？
放心，这些都是Linux骨灰级的玩家编译好放进源里的，他们对于系统环境和软件编译参数的熟悉程度毋庸置疑。

**添加remi源：**
Remi下载地址：<http://rpms.famillecollet.com>
```sh
rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

#yum install -y  http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```



<br>

## RPMForge源

`RPMForge`是`CentOS`系统下的软件仓库，拥有4000多种的软件包, 被CentOS社区认为是最安全也是最稳定的一个软件仓库。

**添加RPMForge源：**
RPMForge下载地址：<http://repository.it4i.cz/mirrors/repoforge/redhat/el7/en/x86_64/rpmforge/RPMS/>
GitHub:<https://github.com/repoforge>

```
rpm -ivh http://repository.it4i.cz/mirrors/repoforge/redhat/el7/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm

#yum localinstall --nogpgcheckhttp://repository.it4i.cz/mirrors/repoforge/redhat/el7/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm
```






















