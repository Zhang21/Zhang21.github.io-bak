---
title: GitLab
date: 2018-12-06 13:11:22
categories: DevOps
tags:
  - GitLab
  - CI
  - CD
  - Git
---



参考:

- 维基百科
- GitLab文档: <https://docs.gitlab.com/>

<br>

版本:

- GitLib-CE: `v11.6.0`
- GitLab-Runner: `v11.6.0`



<br/>
<br/>

<!--more-->

---

<br/>
<br/>


GitLab是由GitLab Inc.开发，使用MIT许可证的基于网络的Git仓库管理工具。包括Git仓库管理、代码审查、问题跟踪、动态订阅、wiki等功能.以及GitLab内部集成的GitLab CI 更是一个持续集成和交付的好工具。


它有两个版本：

- CE
- EE





<br/>
<br/>

---

<br/>
<br/>




# User

User docs



<br/>
<br/>

---

<br/>
<br/>




# Admin

Administrator documentation



<br>


## 安装和维护

Installing and maintaining GitLab


<br/>


### 安装

Installation


GitLab有多种方式进行安装。


<br>

**依赖(requirements)**

在安装之前，请先查看相关依赖文档。

依赖: <https://docs.gitlab.com/ce/install/requirements.html>

- 操作系统

- Ruby版本

- 硬件
	+ CPU
	+ Memory
	+ Storage

- 数据库

- Unicorn Workers
Unicorn是多进程的Server容器。
可以增加unicorn worker的数量，这通常有助于减少应用程序的响应时间并提高处理并行请求的能力。
对于大多数情况，我们建议使用:`CPU cores + 1 = unicorn workers`

```sh
vi /etc/gitlab/gitlab.rb


#CPU Cores=2
unicorn['worker_processes'] = 3
unicorn['worker_timeout'] = 60
```

- Redis and Sidekiq
Redis存储所有用户会话和后台任务队列。Redis的存储要求很低，每个用户约25kB。
Sidekiq是多线程的异步处理程序，使用多线程进程处理后台作业。 此过程从整个Rails Stack(`200MB+`)开始，如果存在内存泄漏，它可能会随着时间的推移而增长。 在非常活跃的服务器上（10,000+活动用户），Sidekiq进程可能使用`1GB +`内存。

- GitLab Runner
我们强烈不要在计划安装GitLab的同一台机器上安装GitLab Runner。根据您决定配置GitLab Runner的方式以及用于在CI环境中运行应用程序的工具，GitLab Runner可能会占用大量可用内存。
如果您决定在同一台机器上运行GitLab Runner和GitLab Rails应用程序，则上面提供的内存消耗计算将无效。
由于安全原因，将所有内容安装在一台计算机上也是不安全的——特别是当您计划将shell执行程序与GitLab Runner一起使用时。
如果您打算使用CI功能，我们建议为每个GitLab Runner使用单独的计算机。


- Prometheus and its exporters
从Omnibus GitLab 9.0开始，Prometheus及其相关的exporter默认启用，一遍轻松、深入地监控GitLab。这些进程大概消耗200MB内存。

- 支持的浏览器

<br>

**安装方式(Installation methods)**

-  Omnibus包: <https://about.gitlab.com/install/>
-  源码
-  Docker

<br>

**数据库(Database)**

- PostgreSQL (highly recommended)
- MySQL/MariaDB (strongly discouraged, not all GitLab features are supported, no support for MySQL/MariaDB GTID)

As of GitLab 10.0, PostgreSQL 9.6 or newer is required, and earlier versions are not supported.

Users using PostgreSQL must ensure the `pg_trgm` extension is loaded into every GitLab database. This extension can be enabled (using a PostgreSQL super user) by running the following query for every database:

```sql
CREATE EXTENSION pg_trgm;
```

在其它系统上，你可能需要安装附加包(e.g. postgresql-contrib)才能使得扩展可用。

如果你需要使用**GitLab Geo**，则需要`postgres_fdw`扩展：

```sql
CREATE EXTENSION postgres_fdw;
```



<br/>
<br/>



#### 包安装

**Centos7为栗子**

```sh
#安装依赖Ruby
#因为需要v2.3版本，而yum查找出来的为v2.0，所以不使用yum安装
#yum info ruby.x86_64

#这里使用Ruby管理工具RVM（“Ruby Version Manager”）进行安装
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB


#开发版
\curl -sSL https://get.rvm.io | bash
#安装稳定版
\curl -sSL https://get.rvm.io | bash -s stable --ruby

#To start using RVM you need to run source ~/.rvm/scripts/rvm
source ~/.rvm/scripts/rvm
#可把它写入profile
vim /etc/profile
source ~/.rvm/scripts/rvm
source /etc/profile

#查看
rvm list known

#安装ruby2.3
rvm install 2.3
ruby --version
ruby 2.3.7p456 (2018-03-28 revision 63024) [x86_64-linux]

#使用2.3
rvm use 2.3

#设为默认
rvm use 2.3 --default



#Install and configure the necessary dependencies
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd

sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld



#install Postfix to send notification emails
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix



#Add the GitLab package repository and install the package
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ce
#镜像如无法下载，可使用国内清华，阿里镜像


#重配置GitLab
sudo gitlab-ctl reconfigure
#这里GitLab会安装许多软件，如Nginx，Prometheus，Redis...
#首次启动会有很多信息，请稍等
#首次访问GitLab,系统会让你重新设置管理员的密码,设置成功后会返回登录界面.默认的管理员账号是root
#在Web界面修改密码


#Browse to the hostname and login
#浏览器访问前面定义的URL
##在Web界面修改密码，并登陆

#Set up your communication preferences
```

![GitLab](/images/GitLab/GitLab_login.jpg)



<br/>
<br/>



#### 配置域名或URL


```sh
#Configuring the external URL for GitLab
vi /etc/gitlab/gitlab.rb
external_url "http://gitlab.example.com"

#重载配置
sudo gitlab-ctl reconfigure



#Configuring a relative URL for Gitlab
#从v8.17以后便不需要再重新编译
#要求：4GB RAM, and 4 or 8 CPU cores
#栗子: https://example.com/gitlab

#Enable relative URL in GitLab
#如果资源不够，可临时关闭 Unicorn and Sidekiq以节省资源
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq

vi /etc/gitlab/gitlab.rb
external_url "https://example.com/gitlab"

#重载配置
sudo gitlab-ctl reconfigure

#重新启动服务，以便Unicorn和Sidekiq获取更改
sudo gitlab-ctl restart



#Disable relative URL in GitLab
external_url后面不包含相对路劲即可
#之后重载配置
sudo gitlab-ctl restart unicorn

```


<br/>
<br/>


#### 从non-root用户载入配置

Loading external configuration file from non-root user

Omnibus-gitlab package 从 `/etc/gitlab/gitlab.rb file`载入所有配置。它属于root用户，有严格的权限配置。它通过root用户由ruby代码执行`gitlab-ctl reconfigure`。

指定其它配置:

```sh
vim /etc/gitlab/gitlab.rb

from_file "/home/admin/external_gitlab.rb"
```



<br/>
<br/>



#### 将Git数据存储在备用目录中

Storing Git data in an alternative directory

默认情况下，`omnibus-gitlab`将repository数据存放于`/var/opt/gitlab/git-data`目录下。repository存储在此目录下的repositories子目录中。
可在`/etc/gitlab/gitlab.rb`中修改`git-data`来添加备用数据目录。
请注意，目录和子目录的路径必须不是链接。

如果还运行了Gitaly，请为每个git数据目录包含`gitaly_address`。

```sh
vim /etc/gitlab/gitlab.rb

#
git_data_dirs({ "default" => { "path" => "/mnt/nas/git-data" } })


#你可以添加不止一个数据目录
git_data_dirs({
  "default" => { "path" => "/var/opt/gitlab/git-data" },
  "alternative" => { "path" => "/mnt/nas/git-data" }
})


#重载配置，使得更改生效
sudo gitlab-ctl reconfigure


#查看
sudo ls /var/opt/gitlab/git-data
repositories



#如果你的/var/opt/gitlab/git-data已有Git repositories，则
# Prevent users from writing to the repositories while you move them.
sudo gitlab-ctl stop

# Note there is _no_ slash behind 'repositories', but there _is_ a
# slash behind 'git-data'.
sudo rsync -av /var/opt/gitlab/git-data/repositories /mnt/nas/git-data/

# Start the necessary processes and run reconfigure to fix permissions
# if necessary
sudo gitlab-ctl upgrade

# Double-check directory layout in /mnt/nas/git-data. Expected output:
# repositories
sudo ls /mnt/nas/git-data/

# Done! Start GitLab and verify that you can browse through the repositories in
# the web interface.
sudo gitlab-ctl start
```



<br/>
<br/>



####　修改Git用户/组

Changing the name of the Git user / group

默认情况下，`omnibus-gitLab`使用`git`用户登录`gitlab-shell`和远程Web接口。不推荐改变已安装的User/Group。

```sh
vim /etc/gitlab/gitlab.rb

#用户名/组名
user['username'] = "git"
user['group'] = "git"

##! The shell for the git user
# user['shell'] = "/bin/sh"

##! The home directory for the git user
# user['home'] = "/var/opt/gitlab"


#uid/gid
#omnibus-gitlab creates users for GitLab, PostgreSQL, Redis and NGINX.你可以指定他们的ID
user['uid'] = 1234
user['gid'] = 1234
postgresql['uid'] = 1235
postgresql['gid'] = 1235
redis['uid'] = 1236
redis['gid'] = 1236
web_server['uid'] = 1237
web_server['gid'] = 1237
```



<br/>
<br/>



#### 禁用用户和组的账号管理

Disable user and group account management

默认情况下，omnibus-gitlab会创建系统用户/组账户，这些系统账户运行包的各种组件。大多数用户都不需要去改变这些行为。然而，如果你的系统账户由其它软件管理，你或许需要禁用此功能。

```sh
vim /etc/gitlab/gitlab.rb

manage_accounts['enable'] = false



#omnibus-gitlab依然保留之前创建的账户
#默认创建以下用户
# GitLab user (required)
git

# Web server user (required)
gitlab-www

# Redis user for GitLab (only when using packaged Redis)
gitlab-redis

# Postgresql user (only when using packaged Postgresql)
gitlab-psql

# Prometheus user for prometheus monitoring and various exporters
gitlab-prometheus

# GitLab Mattermost user (only when using GitLab Mattermost)
mattermost

# GitLab Registry user (only when using GitLab Registry)
registry

# GitLab Consul user (only when using GitLab Consul)
gitlab-consul


#查看用户
sudo awk -F':' '{print $1}' /etc/passwd
```

<br>

你也可以在GitLab配置文件里面更改:

```sh
vi /etc/gitlab/gitlab.rb


# Do not manage user/group accounts
manage_accounts['enable'] = false

# GitLab
user['username'] = "custom-gitlab"
user['group'] = "custom-gitlab"
user['shell'] = "/bin/sh"
user['home'] = "/var/opt/custom-gitlab"

# Web server
web_server['username'] = 'webserver-gitlab'
web_server['group'] = 'webserver-gitlab'
web_server['shell'] = '/bin/false'
web_server['home'] = '/var/opt/gitlab/webserver'

# Postgresql (not needed when using external Postgresql)
postgresql['username'] = "postgres-gitlab"
postgresql['group'] = "postgres-gitlab"
postgresql['shell'] = "/bin/sh"
postgresql['home'] = "/var/opt/postgres-gitlab"

# Redis (not needed when using external Redis)
redis['username'] = "redis-gitlab"
redis['group'] = "redis-gitlab"
redis['shell'] = "/bin/false"
redis['home'] = "/var/opt/redis-gitlab"

# And so on for users/groups for GitLab Mattermost1
```



<br/>
<br/>



#### 禁用存储目录管理

Disable storage directories management

omnibus-gitlab负责使用正确的所有权与权限创建所必须的目录，并保持更新。一种一些目录在配置时可能会包含大量数据，也可能会挂载到NFS。

```sh
vi /etc/gitlab/gitlab.rb


#如果已挂在/etc/gitlab，则可关闭此目录的管理
manage_storage_directories['manage_etc'] = false


#如果要挂载GitLab的所有存储目录，并且每个目录都是单独地挂载，则应完全禁用存储目录的管理
manage_storage_directories['enable'] = false
```

<br>

**GitLab所有数据目录：**

| Default location | Permissions | Ownership | Purpose |
| - | - | - | - |
| /var/opt/gitlab/git-data | 0700 | git:root | Holds repositories directory |
| /var/opt/gitlab/git-data/repositories | 2770 | git:git | Holds git repositories |
| /var/opt/gitlab/gitlab-rails/shared | 0751 | git:gitlab-www | Holds large object directories |
| /var/opt/gitlab/gitlab-rails/shared/artifacts | 0700 | git:root | Holds CI artifacts |
| /var/opt/gitlab/gitlab-rails/shared/lfs-objects | 0700 | git:root | Holds LFS objects |
| /var/opt/gitlab/gitlab-rails/uploads | 0700 | git:root | Holds user attachments |
| /var/opt/gitlab/gitlab-rails/shared/pages | 0750 | git:gitlab-www | Holds user pages |
| /var/opt/gitlab/gitlab-ci/builds | 0700 | git:root | Holds CI build logs |
| /var/opt/gitlab/.ssh | 0700 | git:git | Holds authorized keys |



<br/>
<br/>



#### 仅在挂载给定文件系统后启动Omnibus-GitLab服务

Only start Omnibus-GitLab services after a given filesystem is mounted


如果你想防止Omnibus-GitLab服务(Nginx，Redis，Unicorn...)在挂载给定文件系统之前启动，则:

```sh
vi /etc/gitlab/gitlab.rb


# wait for /var/opt/gitlab to be mounted
high_availability['mountpoint'] = '/var/opt/gitlab'
```



<br/>
<br/>



#### 配置运行时目录

Configuring runtime directory

启用Prometheus监控后，GitLab-monitor将对每个Unicorn进程(Rails metrics)进行监控。每个Unicorn进行都需要将度量文件(metrics file)写入每个控制器临时的位置，然后，Prometheus收集这些文件并处理他们的值。

为了避免创建磁盘I/O，Omnibus-GitLab包将会使用一个运行时目录。

```sh
#During reconfigure, package will check if `/run` is a `tmpfs` mount. If it is not, warning will be printed, and Rails metrics will be disabled.
Runtime directory '/run' is not a tmpfs mount.



#To enable Rails metrics again, create a tmpfs mount and specify it
vi /etc/gitlab/gitlab.rb


# runtime_dir '/run'
runtime_dir '/path/to/tmpfs'
```



<br/>
<br/>



#### 在安装期间禁用自动缓存清理

Disabling automatic cache cleaning during installation

如果安装了大型的GitLab，则你可能不希望运行`rake cache:clean`，因为它将会耗费很长时间。

```sh
vi /etc/gitlab/gitlab.rb


# This is advanced feature used by large gitlab deployments where loading
# whole RAILS env takes a lot of time.
gitlab_rails['rake_cache_clear'] = false



#Enabling/Disabling Rack Attack and setting up basic auth throttling
gitlab_rails['rack_attack_git_basic_auth'] = {
  'enabled' => true, # Enable/Disable Rack Attack
  'ip_whitelist' => ["127.0.0.1"], # Whitelisted urls
  'maxretry' => 10, # Limit the number of Git HTTP authentication attempts per IP
  'findtime' => 60, # Reset the auth attempt counter per IP after 60 seconds
  'bantime' => 3600 # Ban an IP for one hour (3600s) after too many auth attempts
}



#Setting up paths to be protected by Rack Attack
#如果你想改变默认保护路径
#警告：此动作将会被Omnibus-GitLab提供的列表所覆盖
gitlab_rails['rack_attack_protected_paths'] = [
  '/users/password',
  '/users/sign_in',
  '/api/#{API::API.version}/session.json',
  '/api/#{API::API.version}/session',
  '/users',
  '/users/confirmation',
  '/unsubscribes/',
  '/import/github/personal_access_token'
]



#Setting up throttling for ‘paths to be protected’
gitlab_rails['rate_limit_requests_per_period'] = 10
gitlab_rails['rate_limit_period'] = 60
```


<br/>
<br/>



#### 其它

- Nginx
- HTTPS
- Database
- Redis
- SMTP
- ...

具体参考官方文档。




<br/>
<br/>
<br/>




### 更新

Update


更新方式取决你你使用的安装方法。

<br>

**不停机升级(Upgrading without downtime)**

从GitLab 9.1.0开始便可以非脱机更新，但要遵循一下依赖：

- You can only upgrade 1 minor release at a time. So from 9.1 to 9.2, not to 9.3.
- You have to use [post-deployment migrations](https://docs.gitlab.com/ce/development/post_deployment_migrations.html)
- You are using PostgreSQL. If you are using MySQL please look at the release post to see if downtime is required.

<br>

**更新版本(Upgrading between editions)**

- CE->EE
- EE->CE

<br>

**杂项(Miscellaneous)**

- [MySQL to PostgreSQL](https://docs.gitlab.com/ce/update/mysql_to_postgresql.html)
- [Restoring from backup after a failed upgrade](https://docs.gitlab.com/ce/update/restore_after_failure.html)
- [Upgrading PostgreSQL Using Slony](https://docs.gitlab.com/ce/update/upgrading_postgresql_using_slony.html)




<br/>
<br/>
<br/>



### 高可用

**High Availability**: Configure multiple servers for scaling or high availability.


GitLab支持多种不同类型的集群和高可用。方案取决于你所依赖的伸缩和可用的级别。最简单的方式是可伸缩，但并不一定是高可用的。
由于Git的分布式特性，即使GitLab不可用，开发人员仍然可以在本地提交代码。 但是，当GitLab关闭时，某些GitLab功能（如issue tracker and Continuous Integration...）不可用。

请记住，所有高可用性解决方案都需要在成本/复杂性和正常运行时间之间进行权衡。想要正常运行的时间越久，则解决方案就越复杂，则设置和维护它的工作就越多。高可用不是免费的，每个高可用方案都应该考虑成本和收益。


<br/>
<br/>


#### 架构

Architecture


有两种配置：

- active/active
- active/passive

<br>

**Active/Active**

此体系结构可轻松扩展，因为所有应用程序Server可同时处理用户请求。Database、Redis、GitLab都部署在不同的Server上，如果他们配置也是如此，则高度可用。

配置`active/active`所遵循的步骤：

- [配置Database](https://docs.gitlab.com/ce/administration/high_availability/database.html)
- [配置Redis](https://docs.gitlab.com/ce/administration/high_availability/redis.html)
- [配置NFS](https://docs.gitlab.com/ce/administration/high_availability/nfs.html)
- [配置GitLab](https://docs.gitlab.com/ce/administration/high_availability/gitlab.html)
- [配置LoadBlancer](https://docs.gitlab.com/ce/administration/high_availability/load_balancer.html)

![](/images/GitLab/active-active-diagram.png)


<br>

**Active/Passive**

对于没有扩展的高可用/故障转移，你可使用Active/Passive。这利用DRBD（Distributed Replicated Block Device）来保持所有数据同步。DRBD要求低延迟链接保持同步。 不建议尝试在数据中心之间或不同的云可用区域中运行DRBD。

至少需要两台机器(one active/one passive)。

![](/images/GitLab/active-passive-diagram.png)



<br/>
<br/>
<br/>



### 配置

Configuring GitLab


<br/>


#### 配置时区

Adjust your instance’s timezone: Customize the default time zone of GitLab.


GitLab默认时区为`UTC`，

```sh
vi /etc/gitlab/gitlab.rb

# gitlab_rails['time_zone'] = 'UTC'
gitlab_rails['time_zone'] = 'Asia/Shanghai'


#重载重启
gitlab-ctl reconfigure gitlab-ctl restart


#查看时区
gitlab-rake time:zones:all
```



<br/>
<br/>



#### 系统钩子

System hook，Notifications when users, projects and keys are changed.


GitLab实例可对以下事件执行HTTP POST请求：

- project_create
- project_destroy
- project_rename
- project_transfer
- project_update
- user_add_to_team
- user_remove_from_team
- user_create
- user_destroy
- user_failed_login
- user_rename
- key_create
- key_destroy
- group_create
- group_destroy
- group_rename
- user_add_to_group
- user_remove_from_group

可以使用系统钩子，如用于记录或更改 LDAP Server 中的信息。

> 注意：
> 我们遵循`Webhook`中对Push和Tag事件的相同结构，但不会显示commit的信息。Webhook的相同弃用在此有效。


<br/>
<br/>


##### Hook请求

Request Header:
```
X-Gitlab-Event: System Hook

```

项目创建栗子，还有删除、重名、更新、用户、组等其它事件。

```json
{
          "created_at": "2012-07-21T07:30:54Z",
          "updated_at": "2012-07-21T07:38:22Z",
          "event_name": "project_create",
                "name": "StoreCloud",
         "owner_email": "johnsmith@gmail.com",
          "owner_name": "John Smith",
                "path": "storecloud",
 "path_with_namespace": "jsmith/storecloud",
          "project_id": 74,
  "project_visibility": "private"
}
```


<br/>
<br/>


##### Tag事件

当向仓库(Repository)创建或删除标记(tag)时触发，它为每个修改过的标记生成一个事件。。

Request header:

```
X-Gitlab-Event: System Hook
```

Request body:

```json
{
  "event_name": "tag_push",
  "before": "0000000000000000000000000000000000000000",
  "after": "82b3d5ae55f7080f1e6022629cdb57bfae7cccc7",
  "ref": "refs/tags/v1.0.0",
  "checkout_sha": "5937ac0a7beb003549fc5fd26fc247adbce4a52e",
  "user_id": 1,
  "user_name": "John Smith",
  "user_avatar": "https://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=8://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=80",
  "project_id": 1,
  "project":{
    "name":"Example",
    "description":"",
    "web_url":"http://example.com/jsmith/example",
    "avatar_url":null,
    "git_ssh_url":"git@example.com:jsmith/example.git",
    "git_http_url":"http://example.com/jsmith/example.git",
    "namespace":"Jsmith",
    "visibility_level":0,
    "path_with_namespace":"jsmith/example",
    "default_branch":"master",
    "homepage":"http://example.com/jsmith/example",
    "url":"git@example.com:jsmith/example.git",
    "ssh_url":"git@example.com:jsmith/example.git",
    "http_url":"http://example.com/jsmith/example.git"
  },
  "repository":{
    "name": "Example",
    "url": "ssh://git@example.com/jsmith/example.git",
    "description": "",
    "homepage": "http://example.com/jsmith/example",
    "git_http_url":"http://example.com/jsmith/example.git",
    "git_ssh_url":"git@example.com:jsmith/example.git",
    "visibility_level":0
  },
  "commits": [],
  "total_commits_count": 0
}
```


<br/>
<br/>


##### Merge请求事件

在创建一个新的合并(merge)请求时触发，更新、合并、关闭现有合并请求，或在源分支中添加commit。

Request Header:

```
X-Gitlab-Event: System Hook
```

```json
{
  "object_kind": "merge_request",
  "user": {
    "name": "Administrator",
    "username": "root",
    "avatar_url": "http://www.gravatar.com/avatar/e64c7d89f26bd1972efa854d13d7dd61?s=80&d=identicon"
  },
  "project": {
    "name": "Example",
    "description": "",
    "web_url": "http://example.com/jsmith/example",
    "avatar_url": null,
    "git_ssh_url": "git@example.com:jsmith/example.git",
    "git_http_url": "http://example.com/jsmith/example.git",
    "namespace": "Jsmith",
    "visibility_level": 0,
    "path_with_namespace": "jsmith/example",
    "default_branch": "master",
    "ci_config_path": "",
    "homepage": "http://example.com/jsmith/example",
    "url": "git@example.com:jsmith/example.git",
    "ssh_url": "git@example.com:jsmith/example.git",
    "http_url": "http://example.com/jsmith/example.git"
  },
  "object_attributes": {
    "id": 90,
    "target_branch": "master",
    "source_branch": "ms-viewport",
    "source_project_id": 14,
    "author_id": 51,
    "assignee_id": 6,
    "title": "MS-Viewport",
    "created_at": "2017-09-20T08:31:45.944Z",
    "updated_at": "2017-09-28T12:23:42.365Z",
    "milestone_id": null,
    "state": "opened",
    "merge_status": "unchecked",
    "target_project_id": 14,
    "iid": 1,
    "description": "",
    "updated_by_id": 1,
    "merge_error": null,
    "merge_params": {
      "force_remove_source_branch": "0"
    },
    "merge_when_pipeline_succeeds": false,
    "merge_user_id": null,
    "merge_commit_sha": null,
    "deleted_at": null,
    "in_progress_merge_commit_sha": null,
    "lock_version": 5,
    "time_estimate": 0,
    "last_edited_at": "2017-09-27T12:43:37.558Z",
    "last_edited_by_id": 1,
    "head_pipeline_id": 61,
    "ref_fetched": true,
    "merge_jid": null,
    "source": {
      "name": "Awesome Project",
      "description": "",
      "web_url": "http://example.com/awesome_space/awesome_project",
      "avatar_url": null,
      "git_ssh_url": "git@example.com:awesome_space/awesome_project.git",
      "git_http_url": "http://example.com/awesome_space/awesome_project.git",
      "namespace": "root",
      "visibility_level": 0,
      "path_with_namespace": "awesome_space/awesome_project",
      "default_branch": "master",
      "ci_config_path": "",
      "homepage": "http://example.com/awesome_space/awesome_project",
      "url": "http://example.com/awesome_space/awesome_project.git",
      "ssh_url": "git@example.com:awesome_space/awesome_project.git",
      "http_url": "http://example.com/awesome_space/awesome_project.git"
    },
    "target": {
      "name": "Awesome Project",
      "description": "Aut reprehenderit ut est.",
      "web_url": "http://example.com/awesome_space/awesome_project",
      "avatar_url": null,
      "git_ssh_url": "git@example.com:awesome_space/awesome_project.git",
      "git_http_url": "http://example.com/awesome_space/awesome_project.git",
      "namespace": "Awesome Space",
      "visibility_level": 0,
      "path_with_namespace": "awesome_space/awesome_project",
      "default_branch": "master",
      "ci_config_path": "",
      "homepage": "http://example.com/awesome_space/awesome_project",
      "url": "http://example.com/awesome_space/awesome_project.git",
      "ssh_url": "git@example.com:awesome_space/awesome_project.git",
      "http_url": "http://example.com/awesome_space/awesome_project.git"
    },
    "last_commit": {
      "id": "ba3e0d8ff79c80d5b0bbb4f3e2e343e0aaa662b7",
      "message": "fixed readme",
      "timestamp": "2017-09-26T16:12:57Z",
      "url": "http://example.com/awesome_space/awesome_project/commits/da1560886d4f094c3e6c9ef40349f7d38b5d27d7",
      "author": {
        "name": "GitLab dev user",
        "email": "gitlabdev@dv6700.(none)"
      }
    },
    "work_in_progress": false,
    "total_time_spent": 0,
    "human_total_time_spent": null,
    "human_time_estimate": null
  },
  "labels": null,
  "repository": {
    "name": "git-gpg-test",
    "url": "git@example.com:awesome_space/awesome_project.git",
    "description": "",
    "homepage": "http://example.com/awesome_space/awesome_project"
  }
}
```


<br/>
<br/>


##### 库更新事件

Repository Update events

当你push到Repository(包括tag)的时候仅触发一次。

Request Header：

```
X-Gitlab-Event: System Hook
```

Request body:

```json
{
  "event_name": "repository_update",
  "user_id": 1,
  "user_name": "John Smith",
  "user_email": "admin@example.com",
  "user_avatar": "https://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=8://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=80",
  "project_id": 1,
  "project": {
    "name":"Example",
    "description":"",
    "web_url":"http://example.com/jsmith/example",
    "avatar_url":null,
    "git_ssh_url":"git@example.com:jsmith/example.git",
    "git_http_url":"http://example.com/jsmith/example.git",
    "namespace":"Jsmith",
    "visibility_level":0,
    "path_with_namespace":"jsmith/example",
    "default_branch":"master",
    "homepage":"http://example.com/jsmith/example",
    "url":"git@example.com:jsmith/example.git",
    "ssh_url":"git@example.com:jsmith/example.git",
    "http_url":"http://example.com/jsmith/example.git",
  },
  "changes": [
    {
      "before":"8205ea8d81ce0c6b90fbe8280d118cc9fdad6130",
      "after":"4045ea7a3df38697b3730a20fb73c8bed8a3e69e",
      "ref":"refs/heads/master"
    }
  ],
  "refs":["refs/heads/master"]
}
```


<br/>
<br/>



#### 安全

Security: Learn what you can do to further secure your GitLab instance.


<br/>


##### 密码长度

Password length limits

如果要强制使用更长的用户密码，可使用`Devise initializer`来设置。

如果未使用`devise_password_length.rb`初始化程序，则在`config/initializers/devise.rb`中设置密码长度。

```
cd /home/git/gitlab
sudo -u git -H cp config/initializers/devise_password_length.rb.example config/initializers/devise_password_length.rb
sudo -u git -H editor config/initializers/devise_password_length.rb   # inspect and edit the new password length limits
```


<br/>
<br/>


##### 限制SSH秘钥和长度

Restrict SSH key technologies and minimum length

`ssh-keygen`允许用户创建少至768位的RSA密钥，这远低于某些标准组的建议。

这个功能在Web界面的设置里去设置。


<br/>
<br/>


##### 机架攻击

Rack attack

`Rack Attack`, 也称为`Rack::Attack`.旨在通过自定义限制和阻止用户IP来保护GitLab。从`v 11.2`开始，默认禁用此功能。
您可以通过限制来自发出大量请求的IP地址的请求来防止暴力密码攻击，抓取程序或任何其他违规者。 如果您发现限制不足以保护您免受滥用客户端的攻击，Rack Attack提供IP白名单，黑名单，Fail2ban样式过滤和跟踪。
如果你的实例并未对外有任何传入连接，则建议你禁用此功能。


<br/>


##### 设置

```sh
vi /etc/gitlab/gitlab.rb


#启用
gitlab_rails['rack_attack_git_basic_auth'] = {
  'enabled' => true,
  'ip_whitelist' => ["127.0.0.1"],
  'maxretry' => 10, # Limit the number of Git HTTP authentication attempts per IP
  'findtime' => 60, # Reset the auth attempt counter per IP after 60 seconds
  'bantime' => 3600 # Ban an IP for one hour (3600s) after too many auth attempts
}


#重载
sudo gitlab-ctl reconfigure
```


<br/>
<br/>


##### 通过Redis冲机架攻击中移除阻止的IP

Remove blocked IPs from Rack Attack via Redis

如果想移除阻止的IPs，参考下：

```
#在日志中找出被阻止的IPs
grep "Rack_Attack" /var/log/gitlab/gitlab-rails/production.log


#由于黑名单存在Redis中，所以需要连接Redis
/opt/gitlab/embedded/bin/redis-cli -s /var/opt/gitlab/redis/redis.socket


#删除此IP
DEL cache:gitlab:rack::attack:allow2ban:ban:<ip>


#查看
KEYS *rack::attack*



#或者，将其加入白名单
```

<br>

如果所有流量来自于负载均衡器，请记得把负载均衡器加入白名单。


<br/>
<br/>


##### Webhooks和不安全的内部Web服务

Webhooks and insecure internal web services

如果您的GitLab Server或其本地网络中运行non-GitLab Web服务，则这些服务可能很容易被Webhooks利用。
使用Webhook，你便可以设置项目在发生特定事件时触发的URL。通常，这些请求被发送到专门为此目的设置的外部Web服务，以适当的方式处理请求及其附加数据。

然而，当Webhook设置的URL不是指向外部服务而是指向内部服务时，可能会在触发webhook并发送POST请求时完全无意中执行操作。
因为Webhook请求是由GitLab Server本身发出的，所以它们可以完全访问服务器上运行的所有内容或服务器的本地网络，即使这些服务受到其他方面的保护，无法与外界联系。

如果一个Web服务不需要身份认证，Webhooks可以通过让GitLab Server向端点(endpoint)发出POST请求来触发破坏性命令，例如`http://localhost:123/some-resource/delete`

为了防止这种类型的利用，从GitLab v10.6开始，默认禁止对当前GitLab Instance Server Address或private network的所有Webhook请求。

```
#这意味着这些地址都被禁止
127.0.0.1
::1
0.0.0.0
10.0.0.0/8
172.16.0.0/12
173.192.168.0.0/16
```

可在Web界面的设置里面的**Outbound requests**里启用**Allow requests to the local network from hooks and services**.


<br/>
<br/>


##### 信息独占性

Information exclusivity


Git是一个分布式版本控制系统，这意味着使用源代码的每一个人都拥有完整Repository的本地副本。GitLab有Guest、 Reporter、Developer、Maintainer这些项目用户权限。在获取此Repository后，用户可在任何位置上传此Repository。
您无法构建访问控制来阻止有权访问源代码的用户有意共享源代码。这是DVCS的固有特性，所有git管理系统都有此限制。显然你可以采取措施防止无意的共享和信息破坏，这就是为什么只有一些人被允许邀请其他人，没有人可以强制推动受保护的分支。


<br/>
<br/>


##### 重置root密码

reset your root password


使用root权限登录Ruby Rail控制台:

```sh
sudo su -


#控制台
gitlab-rails console production


#等待终端的载入

#查找用户
user = User.where(id: 1).first

#or
user = User.find_by(email: 'admin@local.host')


#修改密码
user.password = 'secret_pass'
user.password_confirmation = 'secret_pass'


#保存更改和退出
user.save!
```


<br/>
<br/>


##### 解锁锁定的用户

How to unlock a locked user


使用root权限登录Server，启动Ruby Rail Console:

```sh
sudo su -

gitlab-rails console production


user = User.where(id: 1).first
#or
user = User.find_by(email: 'admin@local.host')


#解锁
user.unlock_access!
```


<br/>
<br/>


##### 用户文件上传

User File Uploads


如果有人知道直接URL，则附加图像到问题，合并请求或评论不需要查看身份验证。此直接URL包含一个随机的32个字符的ID，可防止未经授权的人员将URL猜到包含敏感信息的图像。我们不启用身份验证，因为这些图像需要在通知电子邮件正文中可见，通常从未通过GitLab验证的电子邮件客户端读取，例如Outlook、Gmail..

请注意，非图像附件确实需要查看身份验证。


<br/>
<br/>


##### 管理CRIME漏洞

How we manage the CRIME vulnerability

> CRIME(“Compression Ratio Info-leak Made Easy”)是一种针对使用HTTPS和SPDY协议进行连接的秘密Web cookie的安全漏洞，这些协议也使用数据压缩。当用于恢复秘密身份验证cookie的内容时，它允许攻击者在经过身份验证的Web会话上执行会话劫持，从而允许发起进一步的攻击。

TLS协议CRIME漏洞影响HTTPS上的压缩，因此它警告不要使用SSL压缩（例如gzip）或SPDY，它也可以选择使用压缩。
虽然在Omnibus安装中启用了SPDY，但CRIME依赖于压缩（'C'），并且NGINX的SPDY模块中的默认压缩级别为0（无压缩）。

GitLab支持gzip和SPDY，并在启用HTTPS时通过停用gzip来缓解CRIME漏洞。
你可以看到问题的来源：

- Source installation NGINX file
- Omnibus installation NGINX file


<br/>
<br/>


##### 强制双重认证

Enforce Two-factor Authentication (2FA)


双因素身份验证（2FA）为GitLab帐户提供了额外的安全级别。启用后，除了提供用户名和密码登录外，还要求在输入应用程序生成的代码。

<br>

**为所有账户启用两步认证：**

有两种方式：

- Enforce on next login.
- Suggest on next login, but allow a grace period before enforcing.

在Web界面里Admin区域里的设置里面的“Sign-in Restrictions”选项。

<br>

**为组中的所有用户启用：**

如果你只想对某些特定组启用两步认证，则你需要则群组设置中启用它。

<br>

**为所有用户禁用：**

```sh
# Omnibus installations
sudo gitlab-rake gitlab:two_factor:disable_for_all_users


# Installations from source
sudo -u git -H bundle exec rake gitlab:two_factor:disable_for_all_users RAILS_ENV=production
```


<br/>
<br/>


##### 注册时用户邮件确认

User email confirmation at sign-up


如果您想在所有用户电子邮件登录之前确认，GitLab管理员可以在注册时启用电子邮件确认。

在Web界面的Admin区域的设置的“ Sign-up Restrictions”里启用“Send confirmation email on sign-up”。




<br/>
<br/>
<br/>



#### 统计、检查和ping

Usage statistics, version check, and usage ping: Enable or disable information about your instance to be sent to GitLab, Inc.


GitLab定期从实例收集各种信息。你可在`Admin area > Settings`去设置他们。

<br>

**停止使用ping：**

在设置面板里面取消，并修改配置。

```
#omnibus
vi /etc/gitlab/gitlab.rb

gitlab_rails['usage_ping_enabled'] = false


#source
vi ./gitlab.yml

production: &base
  # ...
  gitlab:
    # ...
    usage_ping_enabled: false
```



<br/>
<br/>
<br/>



#### 轮询配置

Polling: Configure how often the GitLab UI polls for updates.


GitLab UI按照适合资源的计划轮询不同资源的更新(issue notes, issue titles, pipeline statuses, etc.)
在Web UI的应用程序使用功能中设置它：

- 1(默认值，推荐用于大多数安装)（Issue notes poll every 2 seconds, and issue titles poll every 5 seconds.)
- 0(禁用UI轮询)
- 大于1(将减慢轮询速度)
- 0-1之间(轮询更频繁，不推荐)



<br/>
<br/>
<br/>



#### GitLab Page

GitLab Pages configuration: Enable and configure GitLab Pages.


GitLab Pages使用GitLab Pages Daemon，这是一个用Go编写的简单HTTP Server，可以侦听外部IP地址并提供对自定义域和自定义证书的支持。它通过SNI支持动态证书，默认情况下使用HTTP2公开页面。

对于自定义域（但不是通配符域），Pages Daemon需要侦听端口80/443。因此，您可以灵活设置它：

- 在与GitLab相同的Server中运行Pages Daemon，监听 Secondary IP
- 在与GitLab相同的Server上运行Pages Daemon，监听同一IP的不同Port
- 在单独的Server中运行Pages Daemon


<br/>
<br/>


##### 依赖

在配置Pages之前，你需要：

- 拥有用于提供GitLab Pages的独占根域。请注意，您不能使用GitLab实例域的子域。
- 配置wildcard DNS record.
- HTTPS(可选)
- 启用shared runner(可选，但推荐)

<br>

**将域添加到公共后缀列表**
Add the domain to the Public Suffix List

<br>

**DNS配置**

您需要添加指向GitLab运行的主机的通配符DNS A记录。


<br/>
<br/>


#####　配置

根据您的需要，您可以通过4种不同的方式设置GitLab页面。

- Wildcard domains
- Wildcard domains with TLS support
- Custom domains
- Custom domains with TLS support
- Custom domain verification
- Access control

```
vi /etc/gitlab/gitlab.rb


gitlab_pages
...
```

<br>

**其它一些配置：**

```
#日志记录
gitlab_pages['log_verbose'] = true


#存储路径
gitlab_rails['pages_path'] = "/mnt/storage/pages"


#监听和代理请求
gitlab_pages['listen_proxy'] = "localhost:10080"
#禁用
gitlab_pages['listen_proxy'] = nil

#安全
#备份
#page size
...
```



<br/>
<br/>
<br/>



#### 环境变量

Environment variables: Supported environment variables that can be used to override their defaults values in order to configure GitLab.


GitLab公开了某些环境变量，这些变量可用于覆盖其默认值。

**支持的环境变量：**

| Variable | Type | Description |
| - | - | - |
| `GITLAB_CDN_HOST` | string | Sets the base URL for a CDN to serve static assets (e.g. //mycdnsubdomain.fictional-cdn.com) |
| `GITLAB_ROOT_PASSWORD` | string | Sets the password for the root user on installation |
| `GITLAB_HOST` | string | The full URL of the GitLab server (including http:// or https://) |
| `RAILS_ENV` | string | The Rails environment; can be one of production, development, staging or test |
| `DATABASE_URL` | string | The database URL; is of the form: postgresql://localhost/blog_development |
| `GITLAB_EMAIL_FROM` | string | The e-mail address used in the “From” field in e-mails sent by GitLab |
| `GITLAB_EMAIL_DISPLAY_NAME` | string | The name used in the “From” field in e-mails sent by GitLab |
| `GITLAB_EMAIL_REPLY_TO` | string | The e-mail address used in the “Reply-To” field in e-mails sent by GitLab |
| `GITLAB_EMAIL_SUBJECT_SUFFIX` | string | The e-mail subject suffix used in e-mails sent by GitLab |
| `GITLAB_UNICORN_MEMORY_MIN` | integer | The minimum memory threshold (in bytes) for the Unicorn worker killer |
| `GITLAB_UNICORN_MEMORY_MAX` | integer | The maximum memory threshold (in bytes) for the Unicorn worker killer |
| `GITLAB_SHARED_RUNNERS_REGISTRATION_TOKEN` | string | Sets the initial registration token used for GitLab Runners |

<br>

**完整的数据库变量：**

指定数据库连接信息的推荐方法是设置`DATABASE_URL`环境变量。此变量仅保存连接信息(adapter, database, username, password, host, port)，没有行为信息(encoding, pool)。

如果你不想使用`DATABASE_URL`环境变量或想要使用数据库行为信息，则：

- 复制模板文件: `cp config/database.yml.env config/database.yml`
- 或，为`GITLAB_DATABASE_XXX`变量设置值

你可以设置的`GITLAB_DATABASE_XXX`变量列表：

| Variable | Default value | Overridden by `DATABASE_URL`?         |
| - | - | - |
| `GITLAB_DATABASE_ADAPTER` | postgresql (for MySQL use mysql2) | Yes      |
| `GITLAB_DATABASE_DATABASE` | gitlab_#{ENV['RAILS_ENV'] | Yes     |
| `GITLAB_DATABASE_USERNAME` | root | Yes  |
| `GITLAB_DATABASE_PASSWORD` | None | Yes  |
| `GITLAB_DATABASE_HOST` | localhost | Yes         |
| `GITLAB_DATABASE_PORT` | 5432 | Yes      |
| `GITLAB_DATABASE_ENCODING` | unicode | No        |
| `GITLAB_DATABASE_POOL` | 10 | No         |

<br>

**添加更多变量：**

我们欢迎合并请求，并通过变量进行更多配置。
请在`config/initializers/1_settings.rb`文件中进行更改，并使用`GITLAB_#{name in 1_settings.rb in upper case}`这样的命名方案。


<br>

**Omnibus设置自定义环境变量：**

如有必要，您可以通过`/etc/gitlab/gitlab.rb`设置Unicorn，Sidekiq，Rails和Rake使用的自定义环境变量。这在您需要使用代理来访问Internet并且您希望将外部托管的存储库直接克隆到gitlab的情况下非常有用。

```
gitlab_rails['env'] = {
    "http_proxy" => "my_proxy",
    "https_proxy" => "my_proxy"
}


#你还可以覆盖GitLab组件中的其它环境变量
# Needed for proxying Git clones
gitaly['env'] = {
    "http_proxy" => "my_proxy",
    "https_proxy" => "my_proxy"
}

gitlab_workhorse['env'] = {
    "http_proxy" => "my_proxy",
    "https_proxy" => "my_proxy"
}

# If you use the docker registry
registry['env'] = {
    "http_proxy" => "my_proxy",
    "https_proxy" => "my_proxy"
}


#应用更改
#对环境变量所做的任何更改都需要在重新配置后进行硬重启才能使其生效
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```



<br/>
<br/>
<br/>



#### 插件

GitLab Plugin System
Plugins: With custom plugins, GitLab administrators can introduce custom integrations without modifying GitLab’s source code.

使用自定义插件，GitLab管理员可以在不修改GitLab源代码的情况下引入自定义集成。
你也可以之间修改GitLab源代码而不用编写插件。
必须在GitLab Server上配置插件。

插件将在每个事件上运行，因此您可以在插件代码中过滤事件或项目。你可以拥有任意数量的插件。如果发生事件，每个插件都将由GitLab异步触发。

<br>

**配置：**

插件必须直接放在plugin目录中，按照以下步骤自定义hook:

- 在GitLab Server上，定位到plugin目录
	+ source: `/home/git/gitlab/plugins/`
	+ omnibus: `/opt/gitlab/embedded/service/gitlab-rails/plugins`
- 在plugins目录内，创建一个你需要的文件(文件名不要使用特殊字符)
- 使hook文件可执行，并有git用户所拥有
- 编写代码以使插件功能符合预期。这可以是任何语言
- 插件的数据将在STDIN上以JSON的形式提供

<br>

**验证：**

编写自己的插件可能会非常棘手，如果您可以在不改变系统的情况下进行检查，则会更容易。

```sh
# Omnibus installations
sudo gitlab-rake plugins:validate

# Installations from source
cd /home/git/gitlab
bundle exec rake plugins:validate RAILS_ENV=production
```



<br/>
<br/>
<br/>



#### 规范性

Compliance: A collection of features from across the application that you may configure to help ensure that your 


您可以配置以下GitLab功能，以帮助确保您的GitLab实例符合通用的规范性标准。

| 功能 | GitLab tier |
| - | - |
| **Restrict SSH Keys** <br> 控制用于访问GitLab的SSH密钥的技术和密钥长度 | Core+ | 
| **Granular user roles and flexible permissions** <br> 使用五种不同的用户角色和外部用户设置管理访问权限和权限。根据人员的角色设置权限，而不是对存储库的读取或写入访问权限。不要与只需要访问问题跟踪器的人共享源代码。 | Core+ |
| **Enforce TOS acceptance** <br> 通过阻止GitLab流量强制您的用户接受新的服务条款。| Core+ |
| **Email all users of a project, group, or entire server** <br> 管理员可以根据项目或组成员身份向用户组发送电子邮件，或使用GitLab实例向每个人发送电子邮件。| Starter+ |
| **Omnibus package supports log forwarding** <br> 将日志转发到中央系统。 | Starter+ |
| **Lock project membership to group** <br> 组所有者可以阻止将新成员添加到组中的项目。| Starter+ |
| **LDAP group sync** <br> GitLab企业版使管理员能够自动同步组并管理SSH密钥，权限和身份验证，因此您可以专注于构建产品，而不是配置工具。| Starter+ |
| **LDAP group sync filters** <br> GitLab企业版Premium可以更灵活地基于过滤器与LDAP同步，这意味着您可以利用LDAP属性来映射GitLab权限。 | Premium+ |
| **Audit logs** <br> 为了保持代码的完整性，GitLab Enterprise Edition Premium使管理员能够在高级审计日志系统中查看GitLab服务器内的任何修改，以便您可以控制，分析和跟踪每个更改。 | Premium+ |
| **Auditor users** <br> 审核员用户是对GitLab实例上的所有项目，组和其他资源具有只读访问权限的用户。| Premium+ |



<br/>
<br/>
<br/>



#### 自定义GitLab外观

Customizing GitLab’s appearance


这些外观配置请在Web UI里面进行设置：

- Header logo
- Favicon
- Branded login page
- Welcome message
- “New Project” page



<br/>
<br/>
<br/>
<br/>




### 维护GitLab

Maintaining GitLab


<br/>


#### 靶任务

Raketasks: Perform various tasks for maintenance, backups, automatic webhooks setup, etc.


<br/>


##### 备份

Backing up and restoring GitLab



应用程序数据备份会创建一个归档文件，其中包含数据库、所有Repository和所有附件。
您只能将备份恢复到与其创建的GitLab完全相同的版本和类型（CE / EE）。将Repository从一个服务器迁移到另一个服务器的最佳方法是通过备份还原。

![](/images/GitLab/backup_hrz.png)

<br>

**依赖(requirements)**
为了实现备份和还原，需要在系统上安装两个工具。

- `rsync`
- `tar v1.3+`

<br>

**备份时间戳(Backup timestamp)**

> Note: In GitLab 9.2 the timestamp format was changed from `EPOCH_YYYY_MM_DD` to `EPOCH_YYYY_MM_DD_GitLab_version`

备份存档将保存在`backup_path`中，它在`config/gitlab.yml`文件中指定。文件名为`[TIMESTAMP] _gitlab_backup.tar`，其中TIMESTAMP标识每个备份的创建时间以及GitLab版本。如果需要还原GitLab并且有多个备份可用，则需要时间戳。

<br>

**创建备份(Creating a backup of the GitLab system)**

GitLab提供了一个简单的命令行接口来备份整个实例。包括：

- Database
- Attachments
- Git repositories data
- CI/CD job output logs
- CI/CD job artifacts
- LFS objects
- Container Registry images
- GitLab Pages content

> 注意：GitLab不会备份配置文件、SSL证书、系统文件。

```sh
#omnibus
sudo gitlab-rake gitlab:backup:create

#source
sudo -u git -H bundle exec rake gitlab:backup:create RAILS_ENV=production

#docker
docker exec -t <container name> gitlab-rake gitlab:backup:create

#k8s cluster
kubectl exec -it <gitlab task-runner pod> backup-utility



#输出栗子
sudo gitlab-rake gitlab:backup:create

Dumping database ...
Dumping PostgreSQL database gitlabhq_production ... [DONE]
done
Dumping repositories ...
 * root/zhangbin-test ... [DONE]
[SKIPPED] Wiki
 * root/test02 ... [SKIPPED]
[SKIPPED] Wiki
done
Dumping uploads ...
done
Dumping builds ...
done
Dumping artifacts ...
done
Dumping pages ...
done
Dumping lfs objects ...
done
Dumping container registry images ...
[DISABLED]
Creating backup archive: 1544578010_2018_12_12_11.5.1_gitlab_backup.tar ... done
Uploading backup archive to remote storage  ... skipped
Deleting tmp directories ... done
done
done
done
done
done
done
done
Deleting old backups ... skipping


#查看
sudo ls /var/opt/gitlab/backups/
1544578010_2018_12_12_11.5.1_gitlab_backup.tar


sudo tar -tvf  backups/1544578010_2018_12_12_11.5.1_gitlab_backup.tar
drwx------ git/git           0 2018-12-12 09:26 repositories/
drwxr-xr-x git/git           0 2018-12-12 09:26 repositories/root/
-rw-r--r-- git/git         476 2018-12-12 09:26 repositories/root/zhangbin-test.bundle
drwxr-xr-x git/git           0 2018-12-12 09:26 repositories/root/zhangbin-test/
drwxr-xr-x git/git           0 2018-12-12 09:26 db/
-rw------- git/git       84875 2018-12-12 09:26 db/database.sql.gz
-rw------- git/git         152 2018-12-12 09:26 uploads.tar.gz
-rw------- git/git         151 2018-12-12 09:26 builds.tar.gz
-rw------- git/git         152 2018-12-12 09:26 artifacts.tar.gz
-rw------- git/git         155 2018-12-12 09:26 pages.tar.gz
-rw------- git/git         152 2018-12-12 09:26 lfs.tar.gz
-rw-r--r-- git/git         190 2018-12-12 09:26 backup_information.yml
```

<br>

**保存配置文件(Storing configuration files)**

- Omnibus
	+ `/etc/gitlab/gitlab-secrets.json`
	+ `/etc/gitlab/gitlab.rb`
- Source
	+ `/home/git/gitlab/config/secrets.yml`
	+ `/home/git/gitlab/config/gitlab.yml`
- TLS keys and certificates
- SSH key
- ...

<br>

**备份选项(Backup options)**
备份策略提供了许多可用选项。

- 备份策略(Backup strategy option)
默认备份策略是使用Linux命令`tar`和`gzip`将数据从相应的数据位置流式传输到备份。这在大多数情况下都可以正常工作，但在数据快速变化时会导致问题。
当`tar`读取数据时数据发生变化，读取文件会发生错误，并导致备份过程失败。为了解决这个问题，`v8.17`引入了一种名为`copy`的新备份策略。该策略在调用`tar`和`gzip`之前将数据文件复制到临时位置，以避免错误。
副作用(side-effect)是备份过程中占用额外的磁盘空间，该过程尽最大努力在每个阶段清理临时文件，因此问题不会复杂化，但对于大型安装而言，这可能是一个相当大的变化。

```sh
#使用
sudo gitlab-rake gitlab:backup:create STRATEGY=copy
```

<br>

- 从备份中排出特定目录(Excluding specific directories from the backup)
	+ db (database)
	+ uploads (attachments)
	+ repositories (Git repositories data)
	+ builds (CI job output logs)
	+ artifacts (CI job artifacts)
	+ lfs (LFS objects)
	+ registry (Container Registry images)
	+ pages (Pages content)

你可以使用`SKIP`环境变量来跳过不需要备份的内容，使用逗号来分隔多个

```sh
#栗子
sudo gitlab-rake gitlab:backup:create SKIP=db,uploads
```

<br>

- 上传到本地挂载来共享(Uploading to locally mounted shares)
你也可以使用`Fog Local`存储提供程序将备份发送到已挂载的共享(NFS/CIFS/SMB...)。`local_root` key 指向的目录在挂载时必须由git用户拥有。
除`local_root` key 外，还必须设置`backup_upload_remote_directory`，这是已挂载目录中将要复制备份的子目录，如果不存在则将创建。

```
#Omnibus
#vi /etc/gitlab/gitlab.rb

gitlab_rails['backup_upload_connection'] = {
  :provider => 'Local',
  :local_root => '/mnt/backups'
}

# The directory inside the mounted folder to copy backups to
# Use '.' to store them in the root directory
gitlab_rails['backup_upload_remote_directory'] = 'gitlab_backups'



#source
#vi home/git/gitlab/config/gitlab.yml

backup:
  upload:
    # Fog storage connection settings, see http://fog.io/storage/ .
    connection:
      provider: Local
      local_root: '/mnt/backups'
    # The directory inside the mounted folder to copy backups to
    # Use '.' to store them in the root directory
    remote_directory: 'gitlab_backups'
```

<br>

- 备份归档权限(Backup archive permissions)
GitLab创建的备份归档文件的默认所属用户和组为(`git:git`)，这是为了避免其它系统用户读取GitLab数据。如果你需要备份文件具有其它权限，请在配置文件中修改它：

```sh
#Omnibus
#/etc/gitlab/gitlab.rb

gitlab_rails['backup_archive_permissions'] = 0644 # Makes the backup archives world-readable



#source
#/home/git/gitlab/config/gitlab.yml:
backup:
  archive_permissions: 0644 # Makes the backup archives world-readable

```

<br>

- 配置定时备份(Configuring cron to make daily backups)

请注意，`backup_keep_time`配置选项仅管理本地文件。 GitLab不会自动清理存储在第三方对象存储（例如，AWS S3）中的旧文件，因为用户可能没有列出和删除文件的权限。建议您为对象存储配置适当的保留策略。

```sh
#Omnibus
#/etc/gitlab/gitlab.rb

#默认保留7天
## Limit backup lifetime to 7 days - 604800 seconds
gitlab_rails['backup_keep_time'] = 604800


#cron
sudo su -
crontab -e

#每天2AM
0 2 * * * /opt/gitlab/bin/gitlab-rake gitlab:backup:create CRON=1
```


<br/>
<br/>


##### 恢复

Restore


GitLab提供了一个简单的命令行界面来恢复整个安装，并且足够灵活，可以满足您的需求。
您只能将备份恢复到与其创建的GitLab完全相同的版本和类型（CE / EE）。

<br>

**先决条件(prerequisites)**
在执行还原之前，您需要安装有效的GitLab。这主要是因为通常不允许执行恢复操作（`git`）的系统用户创建或删除将数据导入（`gitlabhq_production`）所需的SQL数据库。所有现有数据将被删除或移动到单独的目录。
要恢复备份，您还需要恢复`/etc/gitlab/gitlab-secrets.json`（Omnibus）或 `/home/git/gitlab/.secret`（Source），它包含了 database encryption key, CI/CD 变量 和 two-factor authentication的变量。如果您无法将此加密密钥文件与应用程序数据备份一起恢复，则启用了双因素身份验证的用户和GitLab Runners将无法访问您的GitLab服务器。
你可能还需要还原TLS keys, certificates, or SSH host keys...

根据你的情况，你可能需要使用如下选项：

- `BACKUP=timestamp_of_backup`: 如果存在多个备份，则必需
- `force=yes`: 不询问`authorized_keys`文件是否应该重新生成，并假设“yes”表示将删除数据库表，启用“写入`authorized_keys`文件”设置，并更新LDAP提供程序。

> 注意: 如果要还原到作为挂载点的目录，则需要在尝试还原之前确保这些目录为空。否则GitLab将在恢复新数据之前尝试移动这些目录，这将导致错误。

<br>

**还原源码安装(Source)**

```sh
# Stop processes that are connected to the database
sudo service gitlab stop

bundle exec rake gitlab:backup:restore RAILS_ENV=production
```

<br>

**还原包安装(Omnibus)**

此过程假定：

- 你已使用包安装相同版本的GitLab
- 你至少已经运行了一次`sudo gitlab-ctl reconfigure`
- GitLab已经运行

首先确保你的备份文件已经放置到了备份目录中(默认为`/var/opt/gitlab/backups`)，并将其所属用户和组修改为`git:git`。

```sh
#复制备份文件
sudo cp 11493107454_2018_04_25_10.6.4-ce_gitlab_backup.tar /var/opt/gitlab/backups/
sudo chown git.git /var/opt/gitlab/backups/11493107454_2018_04_25_10.6.4-ce_gitlab_backup.tar


#暂停程序与数据库的连接
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
# Verify
sudo gitlab-ctl status


#还原
# This command will overwrite the contents of your GitLab database!
sudo gitlab-rake gitlab:backup:restore BACKUP=1493107454_2018_04_25_10.6.4-ce


#重启和检查
sudo gitlab-ctl restart
sudo gitlab-rake gitlab:check SANITIZE=true
```

<br>

**还原Docker**
对于使用Docker或可k8s安装的GitLab，还原期望还原的目录为空。然而，使用Docker和k8s volume 挂载时，可能会在Volume根下创建一些系统级的目录(如: `lost+found`)。这些目录通常由root拥有，可能会导致访问权限错误。因为还原操作是以`git`用户运行。因此，要还原GitLab，请确保还原的目标目录为空。

```sh
#Docker
docker exec -it <name of container> gitlab-rake gitlab:backup:restore
```


<br/>
<br/>


##### 其它备份策略

Alternative backup strategies


如果您的GitLab服务器包含大量Git Repository数据，您可能会发现GitLab备份脚本太慢。在这种情况下，您可以考虑使用文件系统快照作为备份策略的一部分。

- LVM snapshots + rsync
建立一个临时的LVM快照，将它作为只读文件系统挂载到`/mnt/gitlab_backup`。现在我们可以有一个更长的rsync作业，它将在远程Server创建一致的副本。


<br/>
<br/>



##### 完整性检查

Integrity Check

<br>

**仓库完整性(Repository Integrity)**

即使Git非常有弹性并试图防止数据完整性问题，但有时候仍会出现问题。以下Rake task 旨在帮助GitLab管理员诊断问题 Repo，以便修复它们。

- Git repository file system check
- Check for `config.lock` in the repository directory
- Check for any `branch/references` lock files in `refs/heads`

以下症状可能表示Repo完整性存在问题：

- Receiving an error when trying to push code - `remote: error: cannot lock ref`
- A 500 error when viewing the GitLab dashboard or when accessing a specific project

<br>

**检查所有GitLab Repo完整性**

此任务循环遍历GitLab服务器上的所有存储库，并运行前面描述的完整性检查。

```sh
#Omnibus
sudo gitlab-rake gitlab:git:fsck


#source
sudo -u git -H bundle exec rake gitlab:git:fsck RAILS_ENV=production

```

<br>

**上传的文件完整性(Uploaded Files Integrity)**

用户可以将各种类型的文件上传到GitLab上。此外，这些完整性检查可以检测丢失的文件。对于本地存储的文件，在上传时生成校验和(checksum)并将其存储在数据库中，并且这些检查将针对当前文件验证它们。
目前，支持一下类型文件的完整性检查：

- CI artifacts
- LFS objects
- User uploads

```sh
#Omnibus
sudo gitlab-rake gitlab:artifacts:check
sudo gitlab-rake gitlab:lfs:check
sudo gitlab-rake gitlab:uploads:check


#Source
sudo -u git -H bundle exec rake gitlab:artifacts:check RAILS_ENV=production
sudo -u git -H bundle exec rake gitlab:lfs:check RAILS_ENV=production
sudo -u git -H bundle exec rake gitlab:uploads:check RAILS_ENV=production
```

这些任务还接受一些环境变量，您可以使用这些变量来覆盖某些值：

| Variable | Type | Description |
| - | - | - |
| BATCH | integer | Specifies the size of the batch. Defaults to 200. |
| ID_FROM | integer | Specifies the ID to start from, inclusive of the value. |
| ID_TO | integer | Specifies the ID value to end at, inclusive of the value. |
| VERBOSE | boolean | Causes failures to be listed individually, rather than being summarized. |

```sh
#栗子
sudo gitlab-rake gitlab:artifacts:check BATCH=100 ID_FROM=50 ID_TO=250
sudo gitlab-rake gitlab:lfs:check BATCH=100 ID_FROM=50 ID_TO=250
sudo gitlab-rake gitlab:uploads:check BATCH=100 ID_FROM=50 ID_TO=250

```

<br>

**LDAP检查**



<br/>
<br/>


##### 清理

Cleanup

<br>

**从文件系统移除垃圾(Remove garbage from filesystem. Important! Data loss!)**

如果GitLab数据库中不存在 namespace（dirs），则从所有Repo存储路径中删除它们。

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:cleanup:dirs


# installation from source
bundle exec rake gitlab:cleanup:dirs RAILS_ENV=production
```

<br>

如果GitLab数据库中不存在Repo，则从所有Repo存储路径重命名存储库。Repo获得一个`+orphaned+TIMESTAMP`后缀，以便他们无法阻止新Repo的创建。

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:cleanup:repos


# installation from source
bundle exec rake gitlab:cleanup:repos RAILS_ENV=production
```

<br>

如果GitLab数据库中不存在本地项目上传文件，请将其清除。该任务尝试修复文件，如果它可以找到它的项目，否则它将文件移动到丢失和找到的目录。

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:cleanup:project_uploads


# installation from source
bundle exec rake gitlab:cleanup:project_uploads RAILS_ENV=production
```

<br>

如果GitLab数据库中不存在对象存储上载文件，请将其删除。

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:cleanup:remote_upload_files


# installation from source
bundle exec rake gitlab:cleanup:remote_upload_files RAILS_ENV=production
```


<br/>
<br/>


##### 命名空间

Namespaces

**为用户项目启用用户名和命名空间(Enable usernames and namespaces for user projects)**

此命令启动命名空间，它将移动其命名空间文件夹中的每个项目。
注意：

- 由于Repo Location发生改变，因此你需要更新git URL以指向新地址
- 用户名可在Profile中修改

```
#栗子

#Old path
git@example.org:myrepo.git


#new path
git@example.org:username/myrepo.git
#or
git@example.org:groupname/myrepo.git
```


<br/>
<br/>


##### LDAP

LDAP Rake Tasks

**检查(Check)**

LDAP检查Rake task 将测试`bind_dn`和`password`凭据（如果已配置），并将列出LDAP用户的示例。此任务作为`gitlab:check`任务的一部分执行，但可以使用以下命令单独运行。

```sh
#Omnibush
sudo gitlab-rake gitlab:ldap:check


#Source
sudo -u git -H bundle exec rake gitlab:ldap:check RAILS_ENV=production
```

<br>

**重命名提供商(Rename a provider)**

如果更改了配置文件中的LDAP Server ID，则需要更新所有用户标识，否则将无法登录。输入旧的和新的提供商，此任务将更新数据库中的所有匹配标识。

```yaml
#栗子
#main是LDAP Server ID
main:
  label: 'LDAP'
  host: '_your_ldap_server'
  port: 389
  uid: 'sAMAccountName'
  ...
```

```sh
#Omnibus
sudo gitlab-rake gitlab:ldap:rename_provider[old_provider,new_provider]


#Source
bundle exec rake gitlab:ldap:rename_provider[old_provider,new_provider] RAILS_ENV=production
```


<br/>
<br/>


##### 一般维护和自检

**收集有关GitLab及其运行的系统的信息**

```sh
#Omnibus
sudo gitlab-rake gitlab:env:info


#Source
bundle exec rake gitlab:env:info RAILS_ENV=production
```

<br>

**检查GitLab配置**

运行以下`rake tasks`：

- `gitlab:gitlab_shell:check`
- `gitlab:gitaly:check`
- `gitlab:sidekiq:check`
- `gitlab:app:check`

它将检查每个组件是否已根据安装指南进行设置，并针对发现的问题提出修复建议。

```sh
#Omnibus
sudo gitlab-rake gitlab:check


#Source
bundle exec rake gitlab:check RAILS_ENV=production
```

<br>

**重建authorized_keys文件**

在某些情况下，有必要重建authorized_keys文件。

```sh
#Omnibus
sudo gitlab-rake gitlab:shell:setup


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake gitlab:shell:setup RAILS_ENV=production
```

<br>

**清理Redis缓存**

如果由于某种原因，仪表板显示错误信息，您可能希望清除Redis的缓存。

```sh
#Omnibus
sudo gitlab-rake cache:clear


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake cache:clear RAILS_ENV=production
```

<br>

**跟踪部署(Tracking Deployments)**

GitLab提供了一个Rake task，可以让您跟踪GitLab性能监控中的部署。

```sh
#Omnibus
sudo gitlab-rake gitlab:track_deployment


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake gitlab:track_deployment RAILS_ENV=production
```

<br>

**创建或修复Repo hook符号链接(Create or repair repository hooks symlink)**

如果GitLab shell hooks 目录位置更改或其他情况导致hooks符号链接丢失或无效，请运行此Rake task以创建或修复符号链接。

```sh
#Omnibus
sudo gitlab-rake gitlab:shell:create_hooks


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake gitlab:shell:create_hooks RAILS_ENV=production
```

<br>

**检查TCP连接(Check TCP connectivity to a remote site)**

```sh
#Omnibus
sudo gitlab-rake gitlab:tcp_check[example.com,80]


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake gitlab:tcp_check[example.com,80] RAILS_ENV=production
```


<br/>
<br/>


##### 用户管理

User management

**将用户作为开发人员添加到所有项目中**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:import:user_to_projects[username@domain.tld]

# installation from source
bundle exec rake gitlab:import:user_to_projects[username@domain.tld] RAILS_ENV=production
```

<br>

**将所有用户添加到所有项目**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:import:all_users_to_all_projects

# installation from source
bundle exec rake gitlab:import:all_users_to_all_projects RAILS_ENV=production
```

<br>

**将用户作为开发人员添加到所有组**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:import:user_to_groups[username@domain.tld]

# installation from source
bundle exec rake gitlab:import:user_to_groups[username@domain.tld] RAILS_ENV=production
```

<br>

**将所有用户添加到所有组**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:import:all_users_to_all_groups

# installation from source
bundle exec rake gitlab:import:all_users_to_all_groups RAILS_ENV=production
```

<br>

**保持对GitLab上活跃用户数量的严格控制**

```sh
#启用此设置可以阻止新用户被管理员清除(默认：false)
block_auto_created_users: false
```

<br>

**禁用所有用户的双重验证（2FA）**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:two_factor:disable_for_all_users


# installation from source
bundle exec rake gitlab:two_factor:disable_for_all_users RAILS_ENV=production
```

<br>

**轮询双重认证的加密秘钥(Rotate Two-factor Authentication (2FA) encryption key)**

GitLab存储secret data，使双重认证(2FA)能够在加密的数据库列中工作。此数据的加密密钥称为`otp_key_base`，存储在`config/secrets.yml`中。
如果该文件被泄露，但个别2FA secret 没有泄露，则可以使用新的加密密钥重新加密这些机密。这允许您更改泄漏的密钥，而不强制所有用户更改其`2FA`详细信息。

- 首先，查找old key。这是在`config/secrets.yml`文件中，但请确保您正在使用生产部分：

```yaml
production:
  otp_key_base: ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```

- 生成new secret

```sh
# omnibus-gitlab
sudo gitlab-rake secret

# installation from source
bundle exec rake secret RAILS_ENV=production
```

- 现在您需要停止GitLab服务器，备份现有的secrets file并更新数据库：

```sh
# omnibus-gitlab
sudo gitlab-ctl stop
sudo cp config/secrets.yml config/secrets.yml.bak
sudo gitlab-rake gitlab:two_factor:rotate_key:apply filename=backup.csv old_key=<old key> new_key=<new key>


# installation from source
sudo /etc/init.d/gitlab stop
cp config/secrets.yml config/secrets.yml.bak
bundle exec rake gitlab:two_factor:rotate_key:apply filename=backup.csv old_key=<old key> new_key=<new key> RAILS_ENV=production
```

- 最后，将`config/secrets.yml`中的`otp_key_base`更改为`<new key>`并重新启动。再次，确保您在生产部分中运行：

```sh
#首先更改key
#之后重启


# omnibus-gitlab
sudo gitlab-ctl start


# installation from source
sudo /etc/init.d/gitlab start
```

如果出现问题，你也可以进行回滚。


<br/>
<br/>


##### Webhooks

**为所有项目添加webhook**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:add URL="http://example.com/hook"


# source installations
bundle exec rake gitlab:web_hook:add URL="http://example.com/hook" RAILS_ENV=production
```

**为给定NAMESPACE中的项目添加webhook**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:add URL="http://example.com/hook" NAMESPACE=acme


# source installations
bundle exec rake gitlab:web_hook:add URL="http://example.com/hook" NAMESPACE=acme RAILS_ENV=production
```

<br>

**从所有项目中删除webhook**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:rm URL="http://example.com/hook"


# source installations
bundle exec rake gitlab:web_hook:rm URL="http://example.com/hook" RAILS_ENV=production
```

<br>

**从给定NAMESPACE中的项目中删除webhook：**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:rm URL="http://example.com/hook" NAMESPACE=acme


# source installations
bundle exec rake gitlab:web_hook:rm URL="http://example.com/hook" NAMESPACE=acme RAILS_ENV=production
```

<br>

**列出所有webhooks：**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:list


# source installations
bundle exec rake gitlab:web_hook:list RAILS_ENV=production
```

<br>

**列出给定NAMESPACE中项目的webhooks**

```sh
# omnibus-gitlab
sudo gitlab-rake gitlab:web_hook:list NAMESPACE=acme
# source installations
bundle exec rake gitlab:web_hook:list NAMESPACE=acme RAILS_ENV=production
```


<br/>
<br/>


##### 批量导入git库

Import of git repositories in bulk

**注意：**

- The owner of the project will be the first admin
- The groups will be created as needed, including subgroups
- The owner of the group will be the first admin
- Existing projects will be skipped
- Projects in hashed storage may be skipped
- The existing Git repos will be moved from disk

<br>

**如何使用：**

- 创建一个新文件夹以从中导入您的Git Repo：

```sh
#注意owner, group, permission
sudo -u git mkdir /var/opt/gitlab/git-data/repository-import-<date>/new_group
```

- 将Repo复制到新创建的文件夹中
在任何子文件夹中找到的任何`.git` Repo 都将作为项目导入。
将根据需要创建group

```sh
sudo cp -r /old/git/foo.git /var/opt/gitlab/git-data/repository-import-<date>/new_group/


# Do this once when you are done copying git repositories
sudo chown -R git:git /var/opt/gitlab/git-data/repository-import-<date>
```

- 运行命令

```sh
#Omnibus
sudo gitlab-rake gitlab:import:repos['/var/opt/gitlab/git-data/repository-import-<date>']


#Source
cd /home/git/gitlab
sudo -u git -H bundle exec rake gitlab:import:repos['/var/opt/gitlab/git-data/repository-import-<date>'] RAILS_ENV=production
```

<br>

**从`hashed storage`导入Repo**


背景： 传统存储中的项目具有一个目录结构，该结构反映了GitLab中的完整项目路径，包括其命名空间结构。Repo导入程序利用此信息将项目导入其适当的位置。每个项目及其父命名空间都有名称。
但是，散列存储中的项目的目录结构不包含此信息。这有利于各种原因，尤其是改进的性能和数据完整性。

- GitLab v10.3 or earlier: 不支持导入`hashed storage`
- GitLab v10.4 and later: 为了支持从散列存储导入裸存储库，GitLab将每个存储库的完整项目路径存储在git Repo 配置文件的特殊部分中。

如果Repo发生以下事件，则可导入：

- Created
- Migrated to hashed storage
- Renamed
- Transferred to another namespace
- Ancestor renamed
- Ancestor transferred to another namespace

满足以下内容，Repo无法导入：

- It was created in GitLab 10.3 or earlier.
- It was not renamed, transferred, or migrated to hashed storage in GitLab 10.4 and later.
- Its ancestor namespaces were not renamed or transferred in GitLab 10.4 and later.

你也可以手动使用 Rails console 执行此操作：

```
# start a Rails console for GitLab
sudo gitlab-rails console


project = Project.find_by_full_path('gitlab-org/gitlab-ce')
project.write_repository_config


#在Rails控制台会话中，运行以下命令以迁移所有命名空间的项目
namespace = Namespace.find_by_full_path('gitlab-org')
namespace.send(:write_projects_repository_config)
```


<br/>
<br/>


##### 上传

Uploads

> **注意：** 上传表示可以作为单个文件(single file)发送到GitLab的所有用户数据。例如，头像和附注的附件是上传的。上传是GitLab功能的组成部分，因此无法禁用。

**使用本地存储(Local Storage)**
这是默认选项。

```sh
#Omnibus
#上传默认存放位置： /var/opt/gitlab/gitlab-rails/uploads/-/system


#修改
vi /etc/gitlab/gitlab.rb


gitlab_rails['uploads_storage_path'] = "/mnt/storage/"
gitlab_rails['uploads_base_dir'] = "uploads"



#Source
#默认存放位置：/home/git/gitlab/public/uploads/-/system

#修改
vi /home/git/gitlab/config/gitlab.yml

uploads:
  storage_path: /mnt/storage
  base_dir: uploads
```

<br>

**使用对象存储(Object Storage)**

如果不想使用本地存储，可使用对象存储(华为云、阿里云、腾讯云、aws...)。
GitLab有几个选项，其它云服务商的配置请参考他们的文档。

| `uploads_object_store_` | Description | Default|
| - | - | - |
| `enabled` | Enable/disable object storage | false|
| `remote_directory` | The bucket name where Uploads will be stored |  |
| `direct_upload` | Set to true to enable direct upload of Uploads without the need of local shared storage. Option may be removed once we decide to support only single storage for all files. | false|
| `background_upload` | Set to false to disable automatic upload. Option may be removed once upload is direct to S3 | true|
| `proxy_download` | Set to true to enable proxying all files served. Option allows to reduce egress traffic as this allows clients to download directly from remote storage instead of proxying all data | false|
| `connection` | Various connection options described below |  |


<br/>
<br/>


#####　迁移上传文件

Migrate Uploads

**迁移到对象存储(Migrate to Object Storage)**

为GitLab的上传配置对象存储之后，您可以使用此任务将现有的上传文件从本地存储迁移到远程存储。

- All-in-one rake task
GitLab提供了一个 wrapper rake task，可以将所有上传的文件（头像，徽标，附件，图标等）一次性迁移到对象存储。在此之下，它会调用各个rake task来逐个迁移属于这个类别的文件。

```sh
#Omnibus
gitlab-rake "gitlab:uploads:migrate:all"


#Source
sudo RAILS_ENV=production -u git -H bundle exec rake gitlab:uploads:migrate:all
```

- Individual rake tasks
如果您已经运行了前面提到的rake task，则无需像自动完成那样运行这些单独的rake task。

rake task使用3个参数来查找要迁移的上传：

| Parameter | Type | Description |
| - | - | - |
| `uploader_class` | string | Type of the uploader to migrate from |
| `model_class` | string | Type of the model to migrate from |
| `mount_point` | string/symbol | Name of the model’s column on which the uploader is mounted on. |

注意：这些参数主要是GitLab结构的内部参数，您可能需要在下面引用任务列表。
此任务还接受一些可用于覆盖某些值的环境变量：

| Variable | Type	| Description |
| - | - |
| BATCH | integer	| Specifies the size of the batch. Defaults to 200. |


```sh
#Omnibus

# gitlab-rake gitlab:uploads:migrate[uploader_class, model_class, mount_point]

# Avatars
gitlab-rake "gitlab:uploads:migrate[AvatarUploader, Project, :avatar]"
gitlab-rake "gitlab:uploads:migrate[AvatarUploader, Group, :avatar]"
gitlab-rake "gitlab:uploads:migrate[AvatarUploader, User, :avatar]"

# Attachments
gitlab-rake "gitlab:uploads:migrate[AttachmentUploader, Note, :attachment]"
gitlab-rake "gitlab:uploads:migrate[AttachmentUploader, Appearance, :logo]"
gitlab-rake "gitlab:uploads:migrate[AttachmentUploader, Appearance, :header_logo]"

# Favicon
gitlab-rake "gitlab:uploads:migrate[FaviconUploader, Appearance, :favicon]"

# Markdown
gitlab-rake "gitlab:uploads:migrate[FileUploader, Project]"
gitlab-rake "gitlab:uploads:migrate[PersonalFileUploader, Snippet]"
gitlab-rake "gitlab:uploads:migrate[NamespaceFileUploader, Snippet]"
gitlab-rake "gitlab:uploads:migrate[FileUploader, MergeRequest]"





#Source
#Use RAILS_ENV=production for every task.

# sudo -u git -H bundle exec rake gitlab:uploads:migrate

# Avatars
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AvatarUploader, Project, :avatar]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AvatarUploader, Group, :avatar]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AvatarUploader, User, :avatar]"

# Attachments
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AttachmentUploader, Note, :attachment]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AttachmentUploader, Appearance, :logo]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[AttachmentUploader, Appearance, :header_logo]"

# Favicon
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[FaviconUploader, Appearance, :favicon]"

# Markdown
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[FileUploader, Project]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[PersonalFileUploader, Snippet]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[NamespaceFileUploader, Snippet]"
sudo -u git -H bundle exec rake "gitlab:uploads:migrate[FileUploader, MergeRequest]"
```



<br/>
<br/>
<br/>


#### 操作

Performing Operations in GitLab
Operations: Keeping GitLab up and running (clean up Redis sessions, moving repositories, Sidekiq MemoryKiller, Unicorn)

<br>

##### 清理陈旧的Redis回话

Cleaning up stale Redis sessions

在GitLab v7.3之前，用户会话不会自动从Redis expire。


<br/>
<br/>


##### 移动库

Moving repositories

将GitLab管理的所有库移动到另一个文件系统或另一个服务器。


<br/>
<br/>


##### Sidekiq MemoryKiller

配置Sidekiq MemoryKiller以重启Sidekiq。

GitLab Rails应用程序代码有内存泄漏。对于Web请求，使用`unicorn-worker-killer`可以管理这个问题，在需要时会在请求之间重新启动Unicorn工作进程。 `Sidekiq MemoryKiller`对GitLab用于处理后台作业的Sidekiq进程应用相同的方法。

与自从GitLab 6.4以来默认启用的所有GitLab安装的`unicorn-worker-killer`不同，`Sidekiq MemoryKiller`默认仅对Omnibus软件包启用。原因是MemoryKiller依赖于Runit在内存引发的关闭后重新启动Sidekiq，并且来自源的GitLab安装并不都使用Runit或等效的。

使用默认设置，`MemoryKiller`将导致Sidekiq重启频率不超过每15分钟一次，重启会导致传入后台作业延迟大约一分钟。

<br>

**配置MemoryKiller**
MemoryKiller使用环境变量进行控制。

- `SIDEKIQ_MEMORY_KILLER_MAX_RSS`
如果设置了此变量，并且其值大于0，则在每个Sidekiq作业之后，MemoryKiller将检查执行该作业的Sidekiq进程的RSS。如果Sidekiq进程的RSS（KB）超过SIDEKIQ_MEMORY_KILLER_MAX_RSS，则会触发延迟关闭。
默认值可在`gitlab.rb`当中查看。

- `SIDEKIQ_MEMORY_KILLER_GRACE_TIME`
默认为900s（15min）。当触发关闭时，Sidekiq进程将继续正常工作15分钟。

- `SIDEKIQ_MEMORY_KILLER_SHUTDOWN_WAIT`
默认为30秒。当宽限时间到期时，MemoryKiller告诉Sidekiq停止接受新的工作，现有工作有30s去完成。之后，MemoryKiller告诉Sidekiq去关闭，外部监督机制必须重启Sidekiq


<br/>
<br/>


##### Unicorn

Understand Unicorn and unicorn-worker-killer

**Unicorn**
GitLab使用Unicorn，一个pre-forking的Ruby Web服务器来处理Web请求。Unicorn是一个用Ruby和C编写的Daemon，可以加载和运行Ruby on Rails Application(如GitLab CE/EE)。

Unicorn具有多进程(multi-process)架构，可以更好地利用可用的CPU核心并具有更强的容错能力。在启动时，Unicorn的Master进程使用GitLab应用程序代码加载一个干净的Ruby环境，然后生成继承这个干净的初始环境的Worker。 Master永远不会处理任何请求，而是留给Worker。操作系统网络堆栈对传入的请求进行排队，并在Worker之间分配它们。

Unicorn的主要可调参数(Tunables)是工作进程(work process)的数量和请求超时(request timeout)。

<br>

**unicorn-worker-killer**

GitLab存在内存泄漏。这些内存泄漏在长期运行的进程中表现出来，如Unicorn worker。
为了使这些内存泄漏易于管理，GitLab附带了`unicorn-worker-killer`。这个gem修补了Unicorn工作人员在每16个请求后进行内存自检。如果Unicorn工作程序的内存超过预设限制，则工作进程退出。然后Unicorn Master自动替换Wroker。
这是一种处理内存泄漏的强大方法：Unicorn旨在处理“崩溃”的Worker，因此不会丢弃任何用户请求。`unicorn-worker-killer` gem旨在仅在请求之间终止工作进程，因此不会影响用户请求。


<br/>
<br/>


##### 加快SSH操作

Speed up SSH operations by Authorizing SSH users via a fast, indexed lookup to the GitLab database, and/or by doing away with user SSH keys stored on GitLab entirely in favor of SSH certificates.

**快速查找数据库中的授权SSH密钥(Fast lookup of authorized SSH keys in the database)**

<https://docs.gitlab.com/ce/administration/operations/fast_ssh_key_lookup.html>

<br>

**通过Open SSH查找(User lookup via OpenSSH's AuthorizedPrincipalsCommand)**

<https://docs.gitlab.com/ce/administration/operations/ssh_certificates.html>


<br/>
<br/>


##### 文件系统性能基准测试

Filesystem Performance Benchmarking

文件系统性能对整体GitLab性能有很大影响，特别是对于读取或写入Git Repo的操作。

**写性能(Write Performance)**

```sh
#进入Repo root path
cd /var/opt/gitlab/git-data/repositories/test/


#创建一个空目录，便于测试后删除
mkdir test && cd test


#运行命令
time for i in {0..1000}; do echo 'test' > "test${i}.txt"; done


#删除测试目录
cd .. && rm -rf ./test
```

以下是消耗时间范围：

| Rating | Benchmark result |
| - | - |
| Best | Less than 10 seconds |
| OK | 10-18 seconds |
| Poor | 18-25 seconds |
| Very poor | Greater than 25 seconds |



<br/>
<br/>
<br/>


#### 重启

Restart GitLab: Learn how to restart GitLab and its components

依据安装方式，有几种不同的方式：

- Omnibus GitLab restartGitLab Workhorse
	+ Sidekiq
	+ PostgreSQL (if you are using the bundled one)
	+  NGINX (if you are using the bundled one)
	+ Redis (if you are using the bundled one)
	+ Mailroom
	+ Logrotate
- Omnibus GitLab reconfigure
- Source installation restart

<br>

```sh
#Omnibus GitLab restart


#GitLab
sudo gitlab-ctl restart

#Nginx组件
sudo gitlab-ctl restart nginx
#其它组件类似


#GitLab Status
sudo gitlab-ctl status

#Nginx组件状态
sudo gitlab-ctl status nginx
#其它组件类似


#有时，组件在重新启动期间会超时，有时会卡住
#你可以发送kill信号
gitlab-ctl kill <service>
```

```sh
#Omnibus GitLab reconfigure
#在更改/etc/gitlab/gitlab.rb之后，需要重新配置GitLab


sudo gitlab-ctl reconfigure
```

```sh
#Installations from source


sudo service gitlab restart
```




<br/>
<br/>
<br/>



### 更新

Updating GitLab


<br/>


#### GitLab版本和维护策略

GitLab versions and maintenance policy: Understand GitLab versions and releases (Major, Minor, Patch, Security), as well as update recommendations.



GitLab releases:

- Major version: 主要版本，重要内容
- Minor verson: 次要版本，小功能
- Patch number: 补丁，fix bug
- Security: 安全，临时添加的安全补丁

<br>

```
#栗子
GitLab v10.5.7

#10 represents major version
#5 represents minor version
#7 represents patch number
```

<br>

**升级建议:**

GitLab鼓励每个人运行最新的稳定版本(latest stable release)，以确保您可以轻松升级到最安全，功能最丰富的GitLab体验。
如果您无法遵循GitLab的月度发布周期，则需要考虑几种情况：

在一个主要版本(Major)中升级补丁版本(Patch)和次要版本(Minor)被认为是安全的。

```
#Upgrade the patch version:
8.9.0 -> 8.9.7
8.9.0 -> 8.9.1


#Upgrade the minor version:
8.9.4 -> 8.12.3
9.2.3 -> 9.5.5
```

升级主要版本需要多加小心。GitLab无法保证主要版本之间的升级是无缝的。
GitLab建议您首先升级到主要版本中的最新可用次要版本。通过执行此操作，您可以解决可能会在下一个主要版本中更改行为的任何弃用消息。

| Latest stable version	| Your version	| Recommended upgrade path	| Note |
| - | - | - | - |
| `9.4.5` | `8.13.4`	| `8.13.4` -> `8.17.7` -> `9.4.5`	| `8.17.7` is the last version in version 8 |
| `10.1.4` | `8.13.4`	| `8.13.4` -> `8.17.7` -> `9.5.10` -> `10.1.4`	| `8.17.7` is the last version in version 8, 9.5.10 is the last version in version 9 |
| `11.3.4` | `8.13.4`	| `8.13.4` -> `8.17.7` -> `9.5.10` -> `10.8.7` -> `11.3.4`	| `8.17.7` is the last version in version 8, 9.5.10 is the last version in version 9, 10.8.7 is the last version in version 10 |


<br/>
<br/>


#### 更新GitLab

Update GitLab: Update guides to upgrade your installation to a new version.


根据安装方式与GitLab版本，有多种升级方法：

- Omnibus packages
- Source installation
- Docker installation


<br/>


##### 使用软件包的方式进行更新

Updating GitLab installed with the Omnibus GitLab package

<br>

**特定版本:**

- GitLab 11
- GitLab 10
- GitLab 8
- GitLab 7
- GitLab 6

<br>

**升级方法:**

- 使用官方Repo
- 手动下载Package

<br>

**零停机更新(Zero downtime updates)**

> 注意：这仅适用于GitLab 9.1.0或更高版本。

地址: <https://docs.gitlab.com/omnibus/update/README.html#zero-downtime-updates>


<br>

**降级(Downgrading):**

> 注意：本指南假定您在要还原的版本下创建了备份存档。

步骤：

- Download the package of a target version
- Stop GitLab
- Install the old package
- Reconfigure GitLab
- Restoring the backup
- Starting GitLab


<br/>
<br/>


#### 其它项的更新

- MySQL to PostgreSQL
- PostgreSQL to MySQL
- 更新失败之后从备份文件进行还原



<br/>
<br/>
<br/>



### CE-EE

Upgrading or downgrading GitLab


- Upgrade from GitLab CE to GitLab EE
- Downgrade from GitLab EE to GitLab CE




<br/>
<br/>
<br/>
<br/>




## 平台集成

GitLab platform integrations


<br/>


### 集成Mattermost

Mattermost是一个开源，可托管的聊天服务。它被设计为组织和公司的内部聊天，并且主要将自己作为Slack的替代品。

<https://docs.gitlab.com/omnibus/gitlab-mattermost/>



<br/>
<br/>
<br/>



### 集成PlantUML

PlantUML是一个开源工具，允许用户使用纯文本语言创建UML图表。

<https://docs.gitlab.com/ce/administration/integration/plantuml.html>



<br/>
<br/>
<br/>



### 集成Web终端


从GitLab的`CI/CD`环境中提供对部署到Kubernetes的应用程序的终端访问。
随着Kubernetes集成的引入，GitLab获得了为Kubernetes集群存储和使用凭证的能力。它使用这些凭据的一个原因是提供对环境的Web终端的访问。

Web终端的体系结构及其工作原理：

- GitLab依靠用户提供他们自己的Kubernetes凭据，并在部署时适当地标记他们创建的pod。
- 当用户到环境的终端页面时，它们将被提供一个JavaScript应用程序，该应用程序将WebSocket连接返回给GitLab。
- WebSocket在Workhorse中处理，而不是Rails Application Server。
- Workhorse查询Rails的连接细节和用户权限; Rails使用Sidekiq在后台查询Kubernetes
- Workhorse充当用户浏览器和Kubernetes API之间的代理服务器，在两者之间传递WebSocket frame
- Workhorse定期轮询Rails，如果用户不再具有访问终端的权限，或者连接详细信息已更改，则终止WebSocket连接。

> ps: WebSocket是一种在单个TCP连接上进行全双工通信的协议。

<br>

**启用/禁用终端支持(Enabling and disabling terminal support)**

当Web终端使用WebSockets时，Workhorse前面的每个HTTP/HTTPS反向代理都需要配置为将Connection和Upgrade头传递给链中的下一个，在GitLab v8.15+，这是默认选项，不需要你配置。

但是，如果在GitLab前面运行负载均衡器，则可能需要对配置进行一些更改:

- Apache
- NGINX
- HAProxy
- Varnish

Workhorse不会让WebSocket请求通过non-WebSocket端点，因此可以安全地在全局范围内启用对这些Header的支持。如果您宁愿使用较窄的规则集，则可以将其限制为以`/terminal.ws`结尾的URL。

如果您想在GitLab中禁用Web终端支持，只需停止在链中的第一个HTTP反向代理中传递`Connection`和`Upgrade`逐跳Header。对于大多数用户来说，这将是与Omnibus GitLab捆绑在一起的NGINX服务器：

```
#在gitLab.rb中找到 nginx['proxy_set_headers']
#移除或注释 Connection和Upgrade

# nginx['proxy_set_headers'] = {
#  "Upgrade" => "$http_upgrade",
#  "Connection" => "$connection_upgrade"
```

<br>

**限制Websocket连接时间(Limiting WebSocket connection time)**

> ps: GitLab v8.17+

终端会话使用长期连接。默认情况下，这些可能永远持续下去。如果从可伸缩性或安全性角度发现这是不受欢迎的，您可以在GitLab实例的Admin区域中配置最长会话时间。




<br/>
<br/>

---

<br/>
<br/>




## 用户设置和权限

User settings and permissions


<br/>


### Libravatar

Use Libravatar instead of Gravatar for user avatars.


<https://docs.gitlab.com/ce/customization/libravatar.html>



<br/>
<br/>
<br/>



### 注册限制

Sign-up restrictions: block email addresses of specific domains, or whitelist only specific domains.


您可以通过管理区域中的“应用程序设置”阻止特定域的电子邮件地址，或仅将某些特定域列入白名单。

- Whitelist email domains
- Blacklist email domains

白名单和黑名单支持通配符。
如可对白名单加自己信任的域(如：`company.com`)，再把所有加入黑名单(如： `*`)



<br/>
<br/>
<br/>



### 访问限制

Access restrictions: Define which Git access protocols can be used to talk to GitLab (SSH, HTTP, HTTPS).

**启用Git访问协议**

- SSH 和 HTTP(s)
- 仅SSH
- 仅HTTP(s)



<br/>
<br/>
<br/>



### 认证和授权

Authentication/Authorization: Enforce 2FA, configure external authentication with LDAP, SAML, CAS and additional Omniauth providers.


<https://docs.gitlab.com/ce/topics/authentication/index.html>



<br/>
<br/>
<br/>



### 传入电子邮件

Incoming email: Configure incoming emails to allow users to reply by email, create issues by email and merge requests by email, and to enable.


GitLab有几个基于接收传入电子邮件的功能：

- Reply by Email: 允许GitLab用户通过回复notification电子邮件对issues发表comment并merge request
- New issue by email: 允许GitLab用户通过向用户特定的电子邮件地址发送电子邮件来创建新Issue
- New merge request by email: 允许GitLab用户通过向用户特定的电子邮件地址发送电子邮件来创建新的 merge request

<br>

**依赖(Requirements)：**

- Email sub-addressing
- Dedicated email address
- Catch-all mailbox

<br>

**配置：**

```sh
#Omnibus

#在gitlab.rb中找到incoming_email，启用该功能并填写IMAP信息和账户信息


### Reply by email
###! Allow users to comment on issues and merge requests by replying to
###! notification emails.
###! Docs: https://docs.gitlab.com/ce/administration/reply_by_email.html
# gitlab_rails['incoming_email_enabled'] = true

#### Incoming Email Address
####! The email address including the `%{key}` placeholder that will be replaced
####! to reference the item being replied to.
####! **The placeholder can be omitted but if present, it must appear in the
####!   "user" part of the address (before the `@`).**
# gitlab_rails['incoming_email_address'] = "gitlab-incoming+%{key}@gmail.com"

#### Email account username
####! **With third party providers, this is usually the full email address.**
####! **With self-hosted email servers, this is usually the user part of the
####!   email address.**
# gitlab_rails['incoming_email_email'] = "gitlab-incoming@gmail.com"

#### Email account password
# gitlab_rails['incoming_email_password'] = "[REDACTED]"

#### IMAP Settings
# gitlab_rails['incoming_email_host'] = "imap.gmail.com"
# gitlab_rails['incoming_email_port'] = 993
# gitlab_rails['incoming_email_ssl'] = true
# gitlab_rails['incoming_email_start_tls'] = false

#### Incoming Mailbox Settings
####! The mailbox where incoming mail will end up. Usually "inbox".
# gitlab_rails['incoming_email_mailbox_name'] = "inbox"
####! The IDLE command timeout.
# gitlab_rails['incoming_email_idle_timeout'] = 60





#重载配置和重启
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart

#验证邮箱配置
sudo gitlab-rake gitlab:incoming_email:check
```






<br/>
<br/>

---

<br/>
<br/>




## 项目设置

Project settings


<br/>


### Repo检查

Repository checks: Periodic Git repository checks.

> 在GitLab 8.7中引入。它默认关闭，因为它仍会导致过多的误报。


Git有一个内置机制`git fsck`，用于验证提交到存储库的所有数据的完整性。GitLab管理员可以通过管理面板下的项目页面触发对项目的检查。检查以异步方式运行，因此可能需要几分钟才能在项目管理页面上显示检查结果。如果检查失败，您可以在`repocheck.log`下的管理日志页面上看到它们的输出。

<br>

**定期检查(Periodic checks)**

启用后，GitLab会定期对所有项目存储库和wiki存储库运行存储库检查，以检测数据损坏。一个项目每月检查不超过一次。如果任何项目未通过其存储库检查，则所有GitLab管理员都将收到有关该情况的电子邮件通知。\

<br>

**禁用**

可在管理员面板上禁用定期检查。

<br>

**检查失败**

如果某个存储库检查失败，你应该在`repocheck.log`查找错误信息:

- 管理员面板
- 磁盘日志文件
  - `/var/log/gitlab/gitlab-rails` for Omnibus installations
  - `/home/git/gitlab/log` for installations from source

如果由于某种原因定期检查导致大量错误警报，您可以在管理员设置里来选择清除所有存储库检查状态。



<br/>
<br/>
<br/>



### Repo存储路径

Repository storage paths: Manage the paths used to store repositories.


**GitLab允许您定义多个存储库存储路径，以在多个挂载点之间分配存储负载。**

**注意:**
- 您必须至少有一个名为`default`的存储路径
- 路径以键值对进行定义
- 目标目录及其任何子路径都不能是符号链接
- 目标目录不能是制定路径的子目录，因为不能嵌套

```
#栗子
default:
  path: /mnt/git-storage-1
storage2:
  path: /mnt/git-storage-2


#错误栗子
default:
  path: /mnt/git-storage-1
storage2:
  path: /mnt/git-storage-1/git-storage-2 # <- NOT OK because of nesting
```


<br/>
<br/>


#### 配置GitLab

> **注意:**
> 为了使备份正常工作，存储路径不能是挂载点，GitLab用户应具有路径父目录的正确权限。在Omnibus GitLab中，这是自动处理的，但对于Source Code安装，您应该格外小心。

```
gitlab.rb


git_data_dirs({
  "default" => { "path" => "/var/opt/gitlab/git-data" },
  "nfs" => { "path" => "/mnt/nfs/git-data" },
  "cephfs" => { "path" => "/mnt/cephfs/git-data" }
})

#Omnibus将存储库数据存储在git-data/repositories子目录下
```


<br/>
<br/>


#### 选择新项目存储库的存储位置

设置了多个存储路径后，可在Admin下Application Setting选择新项目的存储路径。
从GitLab 8.13.4开始，可以选择多个路径。新项目将随机放置在其中一个选定路径上。



<br/>
<br/>
<br/>



### Repo存储靶任务

Repository storage rake tasks: A collection of rake tasks to list and migrate existing projects and attachments associated with it from Legacy storage to Hashed storage


以下靶任务(rake task)，可用于帮助您列出现有项目以及与之关联的附件，从旧存储到新的Hashed存储类型。


<br/>


#### 将现有项目迁移到哈希存储

Migrate existing projects to Hashed storage


在迁移现有项目之前，还应为新项目启用哈希/散列存储。

```sh
#Omnibus
sudo gitlab-rake gitlab:storage:migrate_to_hashed


# to migrate any non migrated project from ID 20 to 50.
export ID_FROM=20 
export ID_TO=50
```

你可在` Admin > Monitoring > Background jobs`里面进行查看。

在它达到零之后，您可以通过运行以下命令来确认已迁移每个项目。如果您认为有必要，可以再次运行此迁移脚本以安排缺少的项目。


<br/>
<br/>


#### 列出旧版存储的项目

List projects on Legacy storage

```sh
#获取旧项目存储摘要
#Ominibus
sudo gitlab-rake gitlab:storage:legacy_projects


#列出项目使用的旧存储
#Ominibus
sudo gitlab-rake gitlab:storage:list_legacy_projects
```


<br/>
<br/>


#### 列出哈希散列上的项目

List projects on Hashed storage

```sh
#使用哈希存储的项目的简单摘要
#Ominibus
sudo gitlab-rake gitlab:storage:hashed_projects


#列出项目使用的散列存储
#Ominibus
sudo gitlab-rake gitlab:storage:list_hashed_projects
```


<br/>
<br/>


#### 列出旧版存储上的附件

List attachments on Legacy storage

```sh
#使用旧版存储的附件的简单摘要
#Ominibus
sudo gitlab-rake gitlab:storage:legacy_attachments


#列出使用旧版存储的项目附件
#Ominibus
sudo gitlab-rake gitlab:storage:list_legacy_attachments
```


<br/>
<br/>


####列出哈希存储上的附件

List attachments on Hashed storage

```sh
#使用哈希存储的附件的简单摘要
#Ominibus
sudo gitlab-rake gitlab:storage:hashed_attachments


#列出使用哈希存储的项目附件
#Ominibus
sudo gitlab-rake gitlab:storage:list_hashed_attachments
```



<br/>
<br/>
<br/>



### 限制Repo大小

Limit repository size: Set a hard limit for your repositories’ size

> Introduced in GitLab Enterprise Edition 8.12.


GitLab实例中的存储库可能会快速增长，尤其是在使用LFS时。它们的大小可以指数级增长，并且可以非常快速地耗尽您的存储设备。为了避免这种情况发生，您可以为存储库的大小设置硬限制。
可以全局，按组或按项目设置此限制，每个项目限制具有最高优先级。

只有GitLab管理员才能设置这些限制。将限制设置为0表示没有限制。

到目前为止，无法检查新项目的第一次推送的大小，因此第一次推送将允许您上传超过限制规定，但每次后续推送都将被拒绝。但是，LFS对象可以在第一次推送时检查，如果它们的大小总和超过允许的最大存储库大小，则会被拒绝。




<br/>
<br/>

---

<br/>
<br/>



## CI

[GitLab CI/CD](https://docs.gitlab.com/ce/ci/)


<br>

Continuous Integration settings

![](/images/GitLab/cicd_pipeline_infograph.png)



<br/>
<br/>



### Auto DevOps

![](/images/GitLab/idealAutoDevOps.png)

<br>

GitLab Auto DevOps:

- Auto Build
- Auto Test
- Auto Code Quality (GitLab Ultimate)
- Auto SAST (Static Application Security Testing) (GitLab Ultimate)
- Auto Dependency Scanning (GitLab Ultimate)
- Auto License Management (GitLab Ultimate)
- Auto Container Scanning
- Auto Review Apps
- Auto DAST (Dynamic Application Security Testing) (GitLab Ultimate)
- Auto Deploy
- Auto Browser Performance Testing (GitLab Ultimate)
- Auto Monitoring



<br/>
<br/>



### MR规范

![](/images/GitLab/MRSpec.png)

![](/images/GitLab/MRSpec2.png)

<br>

**一些注意事项：**

- 分支情况
  - 生产环境: `master`
  - 测试环境: `test`
  - 代码分支: `dev`
  - 其它分支
  - 分支`master, test, dev`受到保护
  - 可以根据需要添加受保护的分支

| Protected Branch | Allowed to MR | Allowed to push |
| - | - | - |
| `master` | Maintainer | None |
| `test` | Developer+Maintainer | None |
| `dev` | Developer+Maintainer | None |

- 合并请求
  - 合并请求有权限控制
  - 开发者发起MR请求时，即触发对MR源分支的相关检测
  - MR受理者根据检测结果决定是否进行分支合并
  - 运维通过调用检测结果，将分析结果发送给相应的开发人员
  - MR时请注意是否修改到了CICD相关文件
  - 任何触发的Pipeline，都可以人工终止和重做
  - 构建时，构建过程(STAT, 编译，打包，部署，UT...)可以手动选择关闭，各个过程强依赖
  - 可以接受从任何源分支到任何目的分支的合并请求

- CICD相关文件
  - GitLab根据`.gitlab-ci.yml`文件触发相应的动作
  - 将CICD所需文件放于`cicd`目录下
  - 开发者请勿修改CICD相关文件

```yaml
# CICD需要的文件

cicdFile: .gitlab-ci.yml

DockerFile:
  - Dockerfile-test
  - Dockerfile-prod

k8sFile:
  - k8s-xxx-test.yaml
  - k8s-xxx-prod.yaml

# 一些忽略文件
ignoreFile:
  - .gitignore
  - .dockerignore
```

- 打包构建
  - Maven使用云端私有仓库，确保开发环境与Runner的统一
  - Maven公共基础包可配置为阿里云、华为云仓库等
  - Node版本会影响项目的运行，所以Node基础镜像的版本与开发者本地保持一致
  - Docker镜像使用`git commit id`作为`tag`
  - k8s yaml配置文件中的镜像tag使用`TTTAAAGGG`这个唯一字符，在CD时替换成对应的镜像tag
  - k8s yaml文件只应用工作负载(Deployment)，不应用服务(Service)。由于服务基本上不用更改，所以关联的服务一律在容器服务界面上配置

- 检测
  - 开发者发起MR请求时，即触发对MR源分支的相关检测
  - 运维通过调用检测结果，将分析结果发送给相应的开发人员
  - `dev`分支将定时触发Pipeline，并将检查结果发送给相应的开发人员，开发人员可登录Web UI进行查看

- 持续集成/发布(CI/CD)包含哪些内容
  - 代码检测(STAT)
  - 单元测试(UT)
  - 项目打包(Maven, Node, Go...)
  - Docker构建(build, push)
  - K8s集群
  - 自动化测试(Testing)




<br/>
<br/>



### 启用/禁用CICD

Enable/disable GitLab CI/CD: Enable or disable GitLab CI/CD for your instance.


您可以在站点范围内禁用GitLab CI/CD，方法是修改配置文件。

有两点需要注意：

- 禁用GitLab CI/CD只会影响新创建的项目。在此修改之前启用它的项目将像以前一样工作
- 即使禁用了GitLab CI/CD，用户仍然可以在项目设置中启用它

```
#Source
vim gitlab.yml

## Default project features settings, set build to false
default_projects_features:
  issues: true
  merge_requests: true
  wiki: true
  snippets: false
  builds: false



#Omnibus
/etc/gitlab/gitlab.rb

gitlab_rails['gitlab_default_projects_features_builds'] = false


#重载
sudo gitlab-ctl reconfigure
```



<br/>
<br/>
<br/>



### CI/CD admin设置

GitLab CI/CD admin settings: Enable or disable  DevOps site-wide and define the artifacts’ max size and expiration time.


在以管理员登录GitLab Web UI，在Admin area里面，您将找到Auto DevOps，Runners和job artifacts的设置。

<br>

**Auto DevOps**

要为所有项目启用/禁用 Auto DevOps：

- 进入`Admin area > Settings > Continuous Integration and Deployment`
- 检查`Default to Auto DevOps pipeline for all projects`
- 可为Auto DevOps添加基本域
- 保存更改

从现在开始，每个现有项目和新创建的项目都没有`.gitlab-ci.yml`，将使用Auto DevOps pipeline。

<br>

**Maximum artifacts size**

可为GitLab实例设置`[job artifacts][art-yml]`的最大大小。它的单位为`MB`，默认为每个Job设置为`100MB`。`GitLab.com`上它被设置为`1GB`。

<br>

**Default artifacts expiration**

可为GitLab实例的`job artifacts`设置默认到期时间。`GitLab.com`它`never expire`。
这里面的设置是按Job设置的，可在`.gitlab-ci.yml`中覆盖它。将其设置为0表示禁用过期，默认单位是秒。

**Archive jobs**

归档作业通过删除作业的一些功能（运行作业所需的元数据）来减少系统上的CI/CD占用空间，但是为了审计目的而保留跟踪(traces)和工件(artifacts)。
一旦该时间过去，作业将被存档，不再能够重试。让它变空成为永不过期的工作(它必须不少于1天)。



<br/>
<br/>
<br/>



### Jobs artifacts

Job artifacts: Enable, disable, and configure job artifacts (a set of files and directories which are outputted by a job when it completes successfully).


Artifacts是在成功完成后附加到作业的文件和目录的列表。此功能在所有的安装中默认启用。

<br>

**禁用 job artifacts**

```sh
#Omnibus
#/etc/gitlab/gitlab.rb

gitlab_rails['artifacts_enabled'] = false


#重载
```

<br>

**存储 job artifacts**
成功完成作业后，GitLab Runner将job artifacts的存档上传到GitLab。

- 使用本地存储

```
#默认路径
# gitlab_rails['artifacts_path'] = "/var/opt/gitlab/gitlab-rails/shared/artifacts"
```

- 使用对象存储

| Setting | Description | Default |
| - | - | - |
| enabled | Enable/disable object storage | false |
| remote_directory | The bucket name where Artifacts will be stored |   |
| direct_upload | Set to true to enable direct upload of Artifacts without the need of local shared storage. Option may be removed once we decide to support only single storage for all files. | false |
| background_upload | Set to false to disable automatic upload. Option may be removed once upload is direct to S3 | true |
| proxy_download | Set to true to enable proxying all files served. Option allows to reduce egress traffic as this allows clients to download directly from remote storage instead of proxying all data | false |
| connection | Various connection options described below | - |

<br>

**Expiring artifacts**
如果工件使用了失效日期，则在该日期过后立即标记为删除。文件由`expire_build_artifacts_worker` cron job清理，该作业由Sidekiq每小时的第50分钟（`50 * * * *`）运行。

更改工件过期的默认调度计划：

```sh
#Omnibus
#/etc/gitlab/gitlab.rb
gitlab_rails['expire_build_artifacts_worker_cron'] = "50 * * * *"

#重配
```

<br>

**依赖验证(dependencies validation)**
要禁用依赖验证，可在Rail Console设置。

```
#Omnibus

#rails console
sudo gitlab-rails console

#禁用
 Feature.enable('ci_disable_validates_dependencies')
```

<br>

**实施细节**
当GitLab收到工件存档时，GitLab Workhorse也会生成存档元数据文件。此元数据文件描述了工件存档本身中的所有条目。元数据文件采用二进制格式，具有额外的GZIP压缩。

GitLab不解压工件存档以节省Disk，Mem和I/O。它改为检查包含所有相关信息的元数据文件。当存在大量工件或存档是非常大的文件时，这一点尤为重要。
单击特定文件时，GitLab Workhorse会从存档中提取它并开始下载。此实现可节省空间，内存和磁盘I/O.



<br/>
<br/>
<br/>



### Job traces

Job traces: Information about the job traces (logs).


作业跟踪由GitLab Runner在处理作业时发送。您可以在job, pipeline, email notification查看工作踪迹。


<br/>


#### 数据流

Data flow

通常，作业踪迹中有两种状态：
- 实时跟踪(live trace)
- 存档跟踪(archived trace)

| Phase | State | Condition | Data flow | Stored path |
| - | - | - | - | - |
| 1: <br> patching | Live trace | When a job is running | GitLab Runner => Unicorn => file storage | `#{ROOT_PATH}/builds/#{YYYY_mm}/#{project_id}/#{job_id}.log` |
| 2: <br> overwriting | Live trace | When a job is finished | GitLab Runner => Unicorn => file storage | `#{ROOT_PATH}/builds/#{YYYY_mm}/#{project_id}/#{job_id}.log` |
| 3: <br> archiving | Archived trace | After a job is finished | Sidekiq moves live trace to artifacts folder | `#{ROOT_PATH}/shared/artifacts/#{disk_hash}/#{YYYY_mm_dd}/#{job_id}/#{job_artifact_id}/job.log` |
| 4: <br> uploading | Archived trace | After a trace is archived | Sidekiq moves archived trace to object storage (if configured) | `#{bucket_name}/#{disk_hash}/#{YYYY_mm_dd}/#{job_id}/#{job_artifact_id}/job.log` |


<br/>
<br/>


#### 修改工作踪迹本地位置

Changing the job traces local location

更改存储Job Log的位置：

```
#Omnibus
#/etc/gitlab/gitlab.rb
gitlab_ci['builds_directory'] = '/mnt/to/gitlab-ci/builds'



#Source
#/home/git/gitlab/config/gitlab.yml
gitlab_ci:
  # The location where build traces are stored (default: builds/).
  # Relative paths are relative to Rails.root.
  builds_path: path/to/builds/
```

<br/>
<br/>


#### 将踪迹上传到对象存储

Uploading traces to object storage

存档的踪迹被视为工作工件。因此，在设置对象存储集成时，作业踪迹会自动与其他作业工件一起迁移到它。


<br/>
<br/>


####　如何归档旧的作业踪迹文件

How to archive legacy job trace files

旧的作业踪迹指的是在GitLab 10.5之前创建的，未定期归档的作业踪迹。那么你可能需要手动进行操作：

```sh
#执行此任务后，GitLab实例将Sidekiq作业（异步进程）排队，以将作业跟踪文件从本地存储迁移到对象存储。完成所有迁移工作可能需要一些时间。
gitlab-rake gitlab:traces:archive


sudo gitlab-rails console

#如果计数变为零，则归档过程完成
[1] pry(main)> Sidekiq::Stats.new.queues['pipeline_background:archive_trace']
 => 100
```


<br/>
<br/>


#### 如何将归档的作业踪迹迁移到对象存储

How to migrate archived job traces to object storage

> 在GitLab 11.3中引入

如果作业踪迹已存档到本地存储中，并且您希望将这些踪迹迁移到对象存储：

- 确保已启用Job Artifacts的对象存储集成
- 执行此命令: ` gitlab-rake gitlab:traces:migrate`


<br/>
<br/>


#### 如何删除作业踪迹

How to remove job traces

没有办法自动使旧的作业日志过期，但如果它们占用太多空间，则可以安全地删除它们。如果手动删除日志，则UI中的作业输出将为空。


<br/>
<br/>


#### 新的实时踪迹架构

New live trace architecture

> 在GitLab 10.4中引入。在GitLab 11.0中宣布的一般可用性。
> 此功能默认禁用。

这是一个详细的数据流：

- GitLab Runner picks a job from GitLab
- GitLab Runner sends a piece of trace to GitLab
- GitLab appends the data to Redis
- Once the data in Redis reach 128KB, the data is flushed to a persistent store (object storage or the database).
- The above steps are repeated until the job is finished.
- Once the job is finished, GitLab schedules a Sidekiq worker to archive the trace.
- The Sidekiq worker archives the trace to object storage and cleans up the trace in Redis and a persistent store (object storage or the database)

<br>

**Enabling live trace**

```
#console

# Omnibus GitLab
gitlab-rails console

# Installation from source
cd /home/git/gitlab
sudo -u git -H bin/rails console RAILS_ENV=production


#检查实时踪迹
Feature.enabled?('ci_enable_live_trace')

#启用
Feature.enable('ci_enable_live_trace')

#禁用
Feature.disable('ci_enable_live_trace')
```

<br>

**潜在影响(Potential implications)**

在某些情况下，将数据存储在Redis上可能会导致数据丢失：

- **Case 1: When all data in Redis are accidentally flushed**
可以通过重新发送追踪来恢复实时踪迹。未归档的已完成作业的实时踪迹将丢失踪迹数据的最后一部分。

- **Case 2: When Sidekiq workers fail to archive**
目前，Redis中的所有踪迹数据将在一周后删除。如果Sidekiq Worker无法在过期之前完成，则踪迹数据的一部分将丢失。

- 可能出现的另一个问题是它可能占用Redis实例上的所有内存
如果作业数为1000，则消耗128MB（`128KB*1000`）。




<br/>
<br/>
<br/>



### 配置GitLab Runner

Configuring GitLab Runners
Register Shared and specific Runners: Learn how to register and configure Shared and specific Runners to your own instance.


在GitLab CI中，Runners运行`.gitlab-ci.yml`中定义的代码。它们是隔离(虚拟)机器，通过GitLab CI的协调器API获取作业。
Runner可以特定于某个项目，也可以为GitLab CI中的任何项目提供服务。为所有项目提供服务的Runner称为**shared Runner**。
理想情况下，GitLab Runner不应与GitLab安装在同一台机器上。你可以为GitLab实例配置多个Runner。


<br/>
<br/>


#### Runner的状态

Shared, specific and group Runners

安装Runner后，您可以将其注册为共享的或特定的。如果您具有GitLab实例的管理员访问权限，则只能注册shared Runner。

每个Runner可处于一下状态；

- `shared`: Runner runs jobs from all unassigned projects
- `group`: Runner runs jobs from all unassigned projects in its group
- `specific`: Runner runs jobs from assigned projects
- `locked`: Runner cannot be assigned to other projects
- `paused`: Runner will not receive any new jobs


<br/>
<br/>


#### 注册共享的Runner

Registering a shared Runner


如果您是GitLab实例的管理员，则只能注册shared Runner。

在Web UI -> Admin Area -> Runner里面用它提供的URL和Token进行Runner注册。
![](/images/GitLab/Register_Runner.png)

<br>

默认情况下启用shared runner，但可在 Admin Area -> CI/CD里面禁用。

![](/images/Disable_SharedRunner.png)


<br/>
<br/>


#### 注册特定的Runner

Registering a specific Runner

注册特定的Runner有两种方式：

- 使用project registration token来注册Runner
- 将shared Runner 转换为 specific Runner(单向，仅限管理员)

<br>

**使用项目Token注册特定的Runner：**
创建一个没有GitLab实例管理员权限的特定Runner。进入此项目， Setting -> CI/CD -> Runner进行配置。

![](/images/GitLab/Register_SpecificRunner.png)


<br/>
<br/>


#### 注册一个组Runner

Registering a group Runner

创建一个group Runner，然后访问词组，Setting -> CI/CD -> Runner。

![](/images/GitLab/Register_GroupRunner.png)

<br>

**将共享的Runner特定化(Making an existing shared Runner specific)**

如果您是GitLab实例的管理员，则可以将任何shared Runner转换为specific Runner。请记住，这是一种单向转换，不能逆向转换。

- Admin Ares -> Overview -> Runner -> 需要的Runner
- 对项目启用Restrict projects for this Runner

这样，shared Runner便特定于某些项目。

![](/images/GitLab/Convert_Shared_Specific.png)

![](/images/GitLab/Convert_Shared_Specific_View.png)

之后此Runner的状态便发生了改变。


<br/>
<br/>


#### 锁定特定Runner

Locking a specific Runner from being enabled for other projects

您可以配置Runner以将其专门分配给一个项目。当Runner以这种方式锁定时，不能再为其他项目启用它。

- Visit your project’s Settings > CI/CD
- Find the Runner you wish to lock/unlock and make sure it’s enabled
- Click the pencil button
- Check the Lock to current projects option
- Click Save changes for the changes to take effect

![](/images/GitLab/Lock_Runner.png)


<br/>
<br/>


#### 将Runner分配给另外的项目

Assigning a Runner to another project

如果您是分配了特定Runner的项目的维护人员，并且Runner未仅锁定到该项目(not locked only to that project)，则还可以在具有Maintainer权限的任何其他项目上启用Runner。

> 请注意，如果您没有将特定的Runner锁定到特定项目，那么您项目中具有Maintainer角色的任何用户都可以将Runner分配给另一个任意项目，而无需您的授权，因此请谨慎使用。

启用：

- Visit your project’s Settings > CI/CD
- Find the Runner you wish to enable/disable
- Click Enable for this project or Disable for this project

管理员可以为项目启用/禁用特定的Runner：

- Navigate to Admin > Runners
- Find the Runner you wish to enable/disable
- Click edit on the Runner
- Click Enable or Disable on the project


<br/>
<br/>


#### 受保护的Runner

Protected Runners

> 在GitLab 10.0中引入。

你可以保护Runner免于泄露敏感信息。每当Runner受到保护时，Runner仅选择在**受保护的分支**或**受保护的标签**上创建的作业，并忽略其他作业。

protect/unprotect:

- Visit your project’s Settings > CI/CD
- Find a Runner you want to protect/unprotect and make sure it’s enabled
- Click the pencil button besides the Runner name
- Check the Protected option
- Click Save changes for the changes to take effect

![](/images/GitLab/Protected_Runner.png)


<br/>
<br/>


#### 手动清理Runner缓存

Manually clearing the Runners cache

- Navigate to your project’s CI/CD > Pipelines page.
- Click on the Clear Runner caches button to clean up the cache.
- On the next push, your CI/CD job will use a new cache.


<br/>
<br/>


#### 共享Runner如何选择作业

How shared Runners pick jobs

共享的Runner遵守我们称之为合理使用的进程队列(process queue)。公平的使用算法尝试从当前在shared Runners上运行的作业数量最少的项目中将作业分配给shared Runners。


<br/>
<br/>


#### 有效地使用共享Runner

Using shared Runners effectively

如果您打算使用共享的Runners，您应该记住几件事。

<br>

**使用tags**

您必须设置一个Runner才能运行所有不同类型的作业，它可能会在共享的项目中遇到。如果不使用tags，则对于大型项目可能会出现问题。
通过为Runner打tag来标记它可以处理的作业类型，您可以确保shared Runners只运行它们配备的作业(only run the jobs they are equipped to run)。

例如，在GitLab中，如果Runners包含运行Rails测试套件的相应依赖项，那么我们将Runners标记为“rails”

<br>

**Preventing Runners with tags from picking jobs without tags**
您可以配置Runner以防止在Runner没有分配tag时使用tag选择作业。

Runner pick tagged/untagged jobs:

- Visit your project’s Settings ➔ CI/CD
- Find the Runner you wish and make sure it’s enabled
- Click the pencil button
- Check the Run untagged jobs option
- Click Save changes for the changes to take effect

![](/images/GitLab/Tagged_Runner.png)

<br>

**为Runner设置做大作业超时**

对于每个Runner，您可以指定最大作业超时时间。如果小于项目定义的超时，则此类超时将优先。

<br>

**小心敏感信息**

对于一些Runner Executors，如果您可以在Runner上运行作业，您就可以访问它运行的任何代码并获取Runner的Token。使用shared Runners，这意味着在Runner上运行作业的任何人都可以访问在Runner上运行的任何其他人的代码。

通过在大型公共GitLab实例上限制shared Runners的使用，控制对GitLab实例的访问以及使用更安全的Runner Executor，可以轻松避免上述情况。

<br>

**Forks**

每当项目forked时，它都会复制与其相关的作业的设置。这意味着如果您为项目设置了shared Runners并且有人fork该项目，则shated Runners也将为该项目的作业提供服务。


<br/>
<br/>


#### tags

tags用于从允许运行此项目的所有Runner列表中选择特定的Runner。你可以制定Runner的tag.

tags允许您使用分配了指定tag的Runners运行作业：

```
job:
  tags:
    - ruby
    - postgres
```

<br>

例子：

```yaml
windows job:
  stage:
    - build
  tags:
    - windows
  script:
    - echo Hello, %USERNAME%!

osx job:
  stage:
    - build
  tags:
    - osx
  script:
    - echo "Hello, $USER!"
```


<br/>
<br/>


#### 共享Runner的管道配额

Shared Runners pipelines quota: Limit the usage of pipeline minutes for Shared Runners.


在Web UI的Admin Area下的Auto DevOps里面进行配置。



<br/>
<br/>



### Auto DevOps

Enable/disable Auto DevOps: Enable or disable Auto DevOps for your instance

> 在GitLab 10.0中引入。一般在GitLab 11.0上可用。


Auto DevOps提供预定义的`CI/CD`配置，允许您自动检测(detect)，构建(build)，测试(test)，部署(deploy)和监控(monitor)应用程序。利用`CI/CD`最佳实践和工具，Auto DevOps旨在简化成熟和现代软件开发生命周期的设置和执行。


<br/>


#### 综述

> 从`GitLab v11.3`开始，默认情况下为所有项目启用Auto DevOps pipeline。如果尚未为项目显式启用，则会在第一个管道故障时自动禁用Auto DevOps。如果找到一个，您的项目将继续使用备用`CI/CD`配置文件。

借助Auto DevOps，软件开发过程变得更容易设置，因为每个项目都可以拥有从验证到监控的完整工作流程，并且配置最少。只需推送您的代码，GitLab就会处理其他所有事情。这样可以更轻松地启动新项目，并使整个公司的应用程序设置更加一致。


<br/>
<br/>


#### 与应用程序平台和PaaS相比较

Comparison to application platforms and PaaS

Auto DevOps提供通常包含在应用程序平台或PaaS的功能。它有多个灵感：

- Auto DevOps适用于任何k8s集群;你不仅限于在GitLab的基础设施上运行。
- 没有额外成本，你可在任何公共云上使用自托管的k8s集群。
- Auto DevOps包括了安全测试，性能测试和代码质量测试等众多功能。
- Auto DevOps提供增量分级路径。如果您需要高级自定义，则可以开始修改模板，而无需在完全不同的平台上重新开始。


<br/>
<br/>


#### 特性

特性(Features):

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

<br>

由于Auto DevOps依赖于许多不同的组件，因此最好具备以下基本知识：

- Kubernetes
- Helm
- Docker
- GitLab Runner
- Prometheus

<br>

Auto DevOps为所有阶段提供了很好的默认值;但是，您可以根据需要自定义几乎所有内容。


<br/>
<br/>


#### 依赖

Requirements


要充分利用Auto DevOps，您需要:

- **GitLab Runner**(所有阶段都需要)
Runner需要配置为能够运行Docker(通常，这意味着使用Docker或Kubernetes executor，并启用特权模式)。Runner不需要安装在k8s集群中，但k8s executor易于使用并且可以自动进行自动伸缩。基于Docker的Runner也可以使用Docker Machine配置为自动伸缩。应将Runners注册为整个GitLab实例的shared Runners，或分配给特定项目的specific Runner。

- **Base domain**(自动审阅和自动部署所需)
您将需要一个配置了通配符DNS的域，该域将由您的所有Auto DevOps应用程序使用。

- **K8s**(自动审阅、自动部署和自动监控所需)
要启用部署，您需要`k8s v1.5+`。您需要项目的Kubernetes集群，或整个GitLab安装的Kubernetes默认服务模板。
负载均衡器——您可以使用nginx-ingress Helm Chart将NGINX Ingress部署到Kubernetes集群，从而使用NGINX ingress。

- **Prometheus**(自动监控所需)
要启用自动监控，您需要在某处（集群内部或外部）安装Prometheus并配置为刮取您的Kubernetes集群。要获得除系统指标外的响应指标(Metrics)，您还需要配置Prometheus。

<br>

> **注意**：
> 如果您没有安装Kubernetes或Prometheus，则将自动跳过自动审阅，自动部署和自动监控。


<br/>
<br/>


#### 自动化运维基本域

Auto DevOps base domain


如果要使用自动审阅和自动部署，则需要启用Auto DevOps base domain。它可在三个地方定义：

- 在项目下的`CI/CD`
- 在Admin Area -> Setting -> CI/CD
- 在项目下配置变量: `AUTO_DEVOPS_DOMAIN`
- 在组级别配置变量: `AUTO_DEVOPS_DOMAIN`

<br>

需要一个与基本域匹配的通配符DNS A记录，如:

```
*.example.com   3600     A     1.2.3.4


#在这种情况下，`example.com`是用于部署应用程序的域名，`1.2.3.4`是负载均衡器的IP地址(通常是NGINX)。如何设置DNS记录超出了本文档的范围;您应该咨询您的DNS提供商。
#设置完成后，所有请求都会到达负载均衡器，然后负载均衡器会将它们路由到运行应用程序的Kubernetes pod
```


<br/>
<br/>


#### 使用多个k8s集群

Using multiple Kubernetes clusters


使用Auto DevOps时，您可能希望将不同的环境部署到不同的Kubernetes集群。
在Auto DevOps template中，您需要知道3个已定义的环境名称：

- `review/` (从`review/`开始每个环境)
- `staging`
- `production`

<br>

这些环境与使用自动部署的作业相关联，因此除了环境范围之外，它们还需要具有部署到的不同域。这就是您需要根据环境为上述所有内容定义单独的`AUTO_DEVOPS_DOMAIN`变量的原因。

<br>

下表是如何配置三个不同群集的示例:

| 集群名 | 集群环境范围 | `AUTO_DEVOPS_DOMAIN`变量值 | 环境变量范围 | 备注 |
| - | - | - | - | - |
| review | `review/*` | `review.example.com` | `review/*` | The review cluster which will run all Review Apps. `*` is a wildcard, which means it will be used by every environment name starting with `review/.` |
| staging | `staging` | `staging.example.com` | `staging` | (Optional) The staging cluster which will run the deployments of the staging environments. You need to enable it first. |
| production | `production` | `example.com` | `production` | The production cluster which will run the deployments of the production environment. You can use incremental rollouts. |

<br>

要为每个环境添加不同的群集：

- 项目的Operations -> Kubernetes并使用各自的环境范围创建Kubernetes集群，如上表所述
- 创建群集后，到每个群集并安装Helm Tiller和Ingress
- 确保已使用指定的自动化运维域配置DNS
- 到项目的Settings -> CI/CD -> Variables，添加`AUTO_DEVOPS_DOMAIN`变量及其各自的环境范围。

<br>

> **注意：**
> 具有多个群集的组不支持自动DevOps，因为无法在组级别上为每个环境设置`AUTO_DEVOPS_DOMAIN`。


<br/>
<br/>


#### 启用/禁用Auto DevOps


首次使用Auto Devops时，请查看要求以确保可以使用所有必要的组件来充分利用Auto DevOps。

<br>

**在实例级别启用/禁用Auto DevOps（仅限管理员）**

- Admin area -> Settings -> Continuous Integration and Deployment
- `Default to Auto DevOps pipeline for all projects`
- `base domain`

<br>

**在项目级别启用/禁用 Auto DevOps**

- project’s Settings -> CI/CD -> Auto DevOps
- `Default to Auto DevOps pipeline`
- `Domain`
- `Deployment strategy`

<br>

**部署策略(Deployment strategy)**

> Introduced in GitLab 11.0

你可以更改项目的部署策略。有三种策略:

- **Continuous deployment to production**: 允许`master`分支启用Auto Deploy来直接部署到生产环境
- **Continuous deployment to production using timed incremental rollout**: 将`INCREMENTAL_ROLLOUT_MODE`变量设置为`timed`，并且将在`rollout`的每个增量之间延迟5分钟执行生产部署
- **Automatic deployment to staging, manual deployment to production**: 设置`STAGING_ENABLED`为1，`INCREMENTAL_ROLLOUT_MODE`为`manual`。提供手动操作以部署到生产环境


<br/>
<br/>


#### 自动化运维的阶段

Stages of Auto DevOps


以下部分描述了Auto DevOps的各个阶段。仔细阅读它们以了解每个工作原理。


<br/>


##### Auto Build

自动化构建有两种方式创建应用程序的构建：

- 如果有`Dockerfile`, 则使用`docker build`来创建镜像
- 否则，它将使用Herokuish和Heroku buildpacks自动检测并将应用程序构建到Docker镜像中

无论哪种方式，生成的Docker镜像都会自动推送到Container Registry并使用commit SHA进行标记。

> **重要提示：**
> 如果您还使用Auto Review和Auto Deploy并选择提供自己的`Dockerfile`，请确保将应用程序expose到端口5000，因为这是默认Helm图表所假定的端口。


<br/>


##### Auto Test

Auto Test通过分析您的项目来检测语言和框架，使用Herokuish和Heroku buildpacket自动为您的应用程序运行相应的测试。自动检测多种语言和框架，但如果未检测到您的语言，您可以使用自定义构建包(Custom buildpacks)。可检查当前支持的语言。

> **注意：**
> 自动测试使用您在应用程序中已有的测试。如果没有测试，则由您来添加它们。


<br/>


##### Auto Code Quality

> GitLab STARTER BRONZE

Auto Code Quality使用`Code Quality image`对当前代码运行静态分析和其他代码检查。报告已创建，并作为工件上传，您可以在以后下载和检查。
源分支和目标分支之间的任何差异也会显示在合并请求窗口小部件中。


<br/>


##### Auto SAST

> GitLab ULTIMATE GOLD

Static Application Security Testing(SAST)使用`SAST Docker image`对当前代码运行静态分析并检查潜在的安全问题。创建报告后，它将作为工件上载，您可以在以后下载和检查。


<br/>


##### Auto Dependency Scanning

> GitLab ULTIMATE GOLD

Dependency Scanning使用`Dependency Scanning Docker image`对项目依赖关系进行分析并检查潜在的安全问题。创建报告后，它将作为工件上载，您可以在以后下载和检查。


<br/>
<br/>


##### Auto License Management

> GitLab ULTIMATE GOLD

License Management使用`License Management Docker image`搜索项目依赖项以获取其许可证。创建报告后，它将作为工件上载，您可以在以后下载和检查。


<br/>


##### Auto Container Scanning

> GitLab ULTIMATE

容器的漏洞静态分析使用`Clair`在Docker image上运行静态分析并检查潜在的安全问题。创建报告后，它将作为工件上载，您可以在以后下载和检查。


<br/>


##### Auto Review Apps

> **注意：** 这是一个可选步骤，因为许多项目没有可用的Kubernetes集群。如果不满足要求，将默默跳过作业。
> **警告**：不应在Helm之外操作您的应用程序(直接使用Kubernetes)。这可能会导致Helm无法检测到更改，并且随后使用Auto DevOps进行部署可以撤消您的更改。此外，如果您更改某些内容并希望通过再次部署来撤消它，Helm可能无法检测到任何更改，因此没有意识到它需要重新应用旧配置。

Review App 是基于分支代码的临时应用程序环境，因此开发人员，设计人员，QA，产品经理和其他审阅者可以在审阅过程中实际查看代码更改并与之交互。Auto Review Apps为每个分支创建一个Review App。

Auto Review Apps 仅将您的应用部署到您的Kubernetes群集。如果没有可用的群集，则不会进行部署。
Review App将具有基于项目名，分支名、唯一编号以及Auto DevOps基本域的唯一URL。如：`user-project-branch-1234.example.com`。审阅应用程序的链接显示在合并请求窗口小部件中，以便于发现。删除分支时，例如合并合并请求后，将自动删除Review App。


<br/>


##### Auto DAST

> GitLab Ultimate

Dynamic Application Security Testing (DAST)使用流行的开源工具`OWASP ZAProxy`对当前代码执行分析并检查潜在的安全问题。创建报告后，它将作为工件上载，您可以在以后下载和检查。


<br/>


##### Auto Browser Performance Testing

> GitLab Premium

自动浏览器性能测试利用`Sitespeed.io`容器来衡量网页的性能。创建JSON报告并将其作为工件上载，其中包括每个页面的整体性能分数。默认情况下，将测试Review和Production环境的根页面。如果要添加其他URL以进行测试，只需将路径添加到根目录中名为`.gitlab-urls.txt`的文件中，每行一个。

栗子:

```
/
/features
/direction
```


<br/>


##### Auto Deploy

> **注意**：这是一个可选步骤，因为许多项目没有可用的Kubernetes集群。如果不满足要求，将默默跳过作业。
> **警告**：不应在Helm之外操作您的应用程序（直接使用Kubernetes）。这可能会导致Helm无法检测到更改，并且随后使用Auto DevOps进行部署可以撤消您的更改。此外，如果您更改某些内容并希望通过再次部署来撤消它，Helm可能无法检测到任何更改，因此没有意识到它需要重新应用旧配置。

将branch或merge request合并到项目的默认分支（通常是master）后，Auto Deploy将应用程序部署到Kubernetes集群中的生产环境，其中包含基于项目名称和唯一项目ID的命名空间。
您可以使用环境变量自动伸缩pod副本。

值得注意的是，当项目部署到Kubernetes集群时，它依赖于已推送到GitLab Container Registry的Docker image。k8s获取此镜像并运行应用。如果项目是公共的，Kubernetes可以在不进行任何身份验证的情况下访问该映像，从而使我们可以使部署更加可用。如果项目是私有/内部的，则注册表需要凭据才能提取镜像。目前，通过提供`CI_JOB_TOKEN`作为可以使用的密码来解决此问题，但是一旦部署作业完成，此标记将不再有效。这意味着Kubernetes可以运行应用程序，但是如果它应该重新启动或在其他地方执行，则无法再次访问。


<br>


##### Auto Monitoring

> **注意**：检查自动监控的要求以使此阶段工作。

部署应用程序后，自动监控可以立即监控应用程序的服务器和响应指标。自动监控使用Prometheus直接从Kubernetes获取系统指标，如CPU和内存使用情况，以及来自NGINX服务器的响应指标，如HTTP错误率，延迟和吞吐量。

指标有：

- **Response Metrics**: latency, throughput, error rate
- **System Metrics**: CPU utilization, memory utilization

为了使用监控，你需要：

- 将Prometheus部署到k8s集群中
- 配置Prometheus以获取想要的指标


<br/>
<br/>


#### 自定义

Customizing


虽然Auto DevOps提供了很好的默认设置来帮助您入门，但您可以自定义几乎所有内容以满足您的需求;从自定义buildpacks到Dockerfiles，Helm chart，甚至将完整的CI/CD配置复制到项目中进行部署。

<br/>

##### Custom buildpacks

如果项目的自动buildpack检测失败，或者您想使用自定义buildpack，则可以使用项目变量或项目中的`.buildpacks`文件覆盖buildpack:

- **Project variable**: 使用要使用的buildpack的URL创建项目变量`BUILDPACK_URL`
- `.buildpacks`文件: 在项目中添加一个名为`.buildpacks`的文件，并添加要在文件中的一行使用的buildpack的URL(多个使用多行，一行一个)

> **警告**：Auto DevOps尚不支持使用多个buildpack

<br/>

##### Custom `Dockerfile`

如果您的项目的根目录中有一个`Dockerfile`，则Auto DevOps将基于Dockerfile而不是使用buildpacks构建Docker镜像。这可以更快，并导致更小的图像，尤其是如果您的Dockerfile基于Alpine。

<br/>

##### Custom Helm Chart

Auto DevOps使用Helm将您的应用程序部署到Kubernetes。您可以通过将chart捆绑到项目仓库中或通过指定项目变量来覆盖使用的Helm chart：

- **Bundled chart**: 如果您的项目有一个带有`Chart.yaml`文件的`./chart`目录，Auto DevOps将检测chart并使用它而不是默认chart。这可以很好地控制应用程序的部署方式
- **Project variable**: 使用要使用的自定义chart的URL创建项目变量`AUTO_DEVOPS_CHART`

<br/>

##### Customizing `.gitlab-ci.yml`

如果要修改Auto DevOps使用的CI/CD pipeline，可以将[Auto DevOps template](https://gitlab.com/gitlab-org/gitlab-ce/blob/master/lib/gitlab/ci/templates/Auto-DevOps.gitlab-ci.yml "Template")复制到项目的repo中并根据需要进行编辑。

假设您的项目是新的或者没有`.gitlab-ci.yml`文件：

- 在项目 CI/CD里面新建文件
- 选择`.gitlab-ci.yml`模板
- 选择Auto-DevOps
- 编辑此模板
- 提交

> **提示**：Auto DevOps模板包含有用的注释，可帮助您自定义它。如果您希望部署转到临时(staging)环境而不是直接转到生产(production)环境，则可以通过将`.staging`重命名为`staging`来启用`staging`作业；然后确保取消注释生产作业的`when`，将其转换为手动操作，而不是自动部署。

<br/>

##### PostgreSQL

为了支持需要数据库的应用程序，默认情况下会配置PostgreSQL。访问数据库的凭据已预先配置，但可以通过设置关联的变量进行自定义。这些凭据可用于定义`DATABASE_URL`的格式：`postgres://user:password@postgres-host:postgres-port/postgres-database`

<br/>

##### Environment variables

以下变量可用于设置Auto DevOps domain，提供自定义Helm chart或扩展应用程序。 PostgreSQL也可以自定义，您可以轻松使用自定义buildpack。

| 变量 | 描述 |
| - | - |
| `AUTO_DEVOPS_DOMAIN` |  Auto DevOps domain |
| `AUTO_DEVOPS_CHART` | 用于部署应用的Helm Chart |
| `REPLICAS` | 要部署的副本数，默认为1 |
| `PRODUCTION_REPLICAS` | 要在生产环境中部署的副本数。这优先于`REPLICAS`;默认为1 |
| `CANARY_REPLICAS` | Canary Deployments部署的副本数，默认为1 |
| `CANARY_PRODUCTION_REPLICAS` | 生产环境的，优先于`CANARY_REPLICAS`，默认为1 |
| `POSTGRES_ENABLED` | 是否启用PostgreSQL,默认为`true` |
| `POSTGRES_USER` | PostgreSQL用户，默认为`user` |
| `POSTGRES_PASSWORD` | PostgreSQL密码，默认为`testing-password` |
| `POSTGRES_DB` | PostgreSQL数据库名称;默认值为`$CI_ENVIRONMENT_SLUG` |
| `BUILDPACK_URL` | buildpack的完整URL |
| `SAST_CONFIDENCE_LEVEL` | 您希望报告的安全问题的最低置信度; 1为低，2为中，3为高;默认为3 |
| `DEP_SCAN_DISABLE_REMOTE_CHECKS` | 是否禁用远程依赖扫描检查;默认为`false` |
| `DB_INITIALIZE` | 从GitLab 11.4开始，此变量可用于指定运行以初始化应用程序的PostgreSQL数据库的命令。它在应用程序pod内运行 |
| `DB_MIGRATE` | 从GitLab 11.4开始，此变量可用于指定运行以迁移应用程序的PostgreSQL数据库的命令。它在应用程序pod内运行 |
| `STAGING_ENABLED` | 可用于定义部署策略 |
| `CANARY_ENABLED` | 定义canary部署策略 |
| `INCREMENTAL_ROLLOUT_MODE` | 从GitLab 11.4开始，此变量（如果存在）可用于为生产环境启用应用程序的增量部署 |
| `TEST_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用测试作业 |
| `CODE_QUALITY_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用代码质量作业 |
| `SAST_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用sast作业 |
| `DEPENDENCY_SCANNING_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用`dependency_scanning`作业 |
| `CONTAINER_SCANNING_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用`sast：container`作业 |
| `REVIEW_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用审核和手动审核：停止作业 |
| `PERFORMANCE_DISABLED` | 从GitLab 11.0开始，此变量可用于禁用性能作业 |

<br>

> **提示**：使用项目变量设置副本变量，并通过重新部署来扩展应用程序！
> **小心**: 你不应该直接使用k8s来扩展你的应用程序，这可能会导致Helm异常。

<br/>

**高级副本变量设置(Advanced replica variables setup)**

除了上面提到的两个与副本相关的生产变量之外，您还可以将其它变量用于不同的环境。




<br/>
<br/>


#### 目前支持的语言

Currently supported languages

从GitLab 10.0开始，支持的构建包是：

```yaml
- heroku-buildpack-multi     v1.0.0
- heroku-buildpack-ruby      v168
- heroku-buildpack-nodejs    v99
- heroku-buildpack-clojure   v77
- heroku-buildpack-python    v99
- heroku-buildpack-java      v53
- heroku-buildpack-gradle    v23
- heroku-buildpack-scala     v78
- heroku-buildpack-play      v26
- heroku-buildpack-php       v122
- heroku-buildpack-go        v72
- heroku-buildpack-erlang    fa17af9
- buildpack-nginx            v8
```



<br/>
<br/>



### Auto DevOps template

各种模板: <https://gitlab.com/gitlab-org/gitlab-ce/tree/master/lib/gitlab/ci/templates>

在项目也可选择新建`.gitlab-ci.yml`模板文件，然后根据需要就行适当的修改。



<br/>
<br/>
<br/>



### `CI/CD`环境变量

CI/CD Variables - Learn how to use variables defined in your `.gitlab-ci.yml` or the ones defined in your project’s settings


当从GitLab CI接收作业时，Runner准备构建环境。首先，设置预定义变量列表**predefined variables**（环境变量）和用户定义变量列表**user-defined variables**。


<br/>
<br/>


#### 变量优先级

Priority of variables

变量可以被覆盖，并且它们按此顺序优先于彼此：

1. Trigger variables / scheduled pipeline variables
2. Project-level variables / protected variables
3. Group-level variables / protected variables
4. YAML-defined job-level variables
5. YAML-defined global variables
6. Deployment variables
7. Predefined variables


<br/>
<br/>


#### 不支持的变量

在某些情况下，某些变量无法在`.gitlab-ci.yml`定义的上下文中使用——如在`script`下定义的变量。


<br/>
<br/>


#### 预定义变量

Predefined variables (Environment variables)

> **注意：**
> 从`GitLab 9.0`开始，我们已经弃用了一些变量。阅读`9.0`重命名部分以找出它们的替代品。强烈建议您使用新变量，因为我们将在以后的GitLab版本中删除旧变量。


| Variable | GitLab | Runner | Description |
| - | - | - | - |
| `ARTIFACT_DOWNLOAD_ATTEMPTS` | 8.15 | 1.9 | Number of attempts to download artifacts running a job |
| `CI` | all | 0.4 | Mark that job is executed in CI environment |
| `CI_COMMIT_BEFORE_SHA` | 11.2 | all | The previous latest commit present on a branch before a push request. |
| `CI_COMMIT_DESCRIPTION` | 10.8 | all | The description of the commit: the message without first line, if the title is shorter than 100 characters; full message in other case. |
| `CI_COMMIT_MESSAGE` | 10.8 | all | The full commit message. |
| `CI_COMMIT_REF_NAME` | 9.0 | all | The branch or tag name for which project is built |
| `CI_COMMIT_REF_SLUG` | 9.0 | all | $CI_COMMIT_REF_NAME lowercased, shortened to 63 bytes, and with everything except 0-9 and a-z replaced with -. No leading / trailing -. Use in URLs, host names and domain names. |
| `CI_COMMIT_SHA` | 9.0 | all | The commit revision for which project is built |
| `CI_COMMIT_SHORT_SHA` | 11.7 | all | The first eight characters of CI_COMMIT_SHA |
| `CI_COMMIT_TAG` | 9.0 | 0.5 | The commit tag name. Present only when building tags. |
| `CI_COMMIT_TITLE` | 10.8 | all | The title of the commit - the full first line of the message |
| `CI_CONFIG_PATH` | 9.4 | 0.5 | The path to CI config file. Defaults to .gitlab-ci.yml |
| `CI_DEBUG_TRACE` | all | 1.7 | Whether debug tracing is enabled |
| `CI_DEPLOY_PASSWORD` |  10.8 |  all | Authentication password of the GitLab Deploy Token, only present if the Project has one related. |
| `CI_DEPLOY_USER` | 10.8 | all | Authentication username of the GitLab Deploy Token, only present if the Project has one related. |
| `CI_DISPOSABLE_ENVIRONMENT` | all | 10.1 | Marks that the job is executed in a disposable environment (something that is created only for this job and disposed of/destroyed after the execution - all executors except shell and ssh). If the environment is disposable, it is set to true, otherwise it is not defined at all. |
| `CI_ENVIRONMENT_NAME` | 8.15 | all | The name of the environment for this job |
| `CI_ENVIRONMENT_SLUG` | 8.15 | all | A simplified version of the environment name, suitable for inclusion in DNS, URLs, Kubernetes labels, etc. |
| `CI_ENVIRONMENT_URL` | 9.3 | all | The URL of the environment for this job |
| `CI_JOB_ID` | 9.0 | all | The unique id of the current job that GitLab CI uses internally |
| `CI_JOB_MANUAL` | 8.12 | all | The flag to indicate that job was manually started |
| `CI_JOB_NAME` | 9.0 | 0.5 | The name of the job as defined in .gitlab-ci.yml |
| `CI_JOB_STAGE` | 9.0 | 0.5 | The name of the stage as defined in .gitlab-ci.yml |
| `CI_JOB_TOKEN` | 9.0 | 1.2 | Token used for authenticating with the GitLab Container Registry and downloading dependent repositories |
| `CI_JOB_URL` | 11.1 | 0.5 | Job details URL |
| `CI_MERGE_REQUEST_ID` | 11.6 | all | The ID of the merge request if it’s pipelines for merge requests |
| `CI_MERGE_REQUEST_IID` | 11.6 | all | The IID of the merge request if it’s pipelines for merge requests |
| `CI_MERGE_REQUEST_PROJECT_ID` | 11.6 | all | The ID of the project of the merge request if it’s pipelines for merge requests |
| `CI_MERGE_REQUEST_PROJECT_PATH` | 11.6 | all | The path of the project of the merge request if it’s pipelines for merge requests (e.g. namespace/awesome-project) |
| `CI_MERGE_REQUEST_PROJECT_URL` | 11.6 | all | The URL of the project of the merge request if it’s pipelines for merge requests (e.g. http://192.168.10.15:3000/namespace/awesome-project) |
| `CI_MERGE_REQUEST_REF_PATH` | 11.6 | all | The ref path of the merge request if it’s pipelines for merge requests. (e.g. refs/merge-requests/1/head) |
| `CI_MERGE_REQUEST_SOURCE_BRANCH_NAME` | 11.6 | all | The source branch name of the merge request if it’s pipelines for merge requests |
| CI_MERGE_REQUEST_SOURCE_PROJECT_ID | 11.6 | all | The ID of the source project of the merge request if it’s pipelines for merge requests |
| CI_MERGE_REQUEST_SOURCE_PROJECT_PATH | 11.6 | all | The path of the source project of the merge request if it’s pipelines for merge requests |
| CI_MERGE_REQUEST_SOURCE_PROJECT_URL | 11.6 | all | The URL of the source project of the merge request if it’s pipelines for merge requests |
| CI_MERGE_REQUEST_TARGET_BRANCH_NAME | 11.6 | all | The target branch name of the merge request if it’s pipelines for merge requests |
| CI_NODE_INDEX | 11.5 | all | Index of the job in the job set. If the job is not parallelized, this variable is not set. |
| CI_NODE_TOTAL | 11.5 | all | Total number of instances of this job running in parallel. If the job is not parallelized, this variable is set to 1. |
| CI_API_V4_URL | 11.7 | all | The GitLab API v4 root URL |
| CI_PIPELINE_ID | 8.10 | all | The unique id of the current pipeline that GitLab CI uses internally |
| CI_PIPELINE_IID | 11.0 | all | The unique id of the current pipeline scoped to project |
| CI_PIPELINE_SOURCE | 10.0 | all | Indicates how the pipeline was triggered. Possible options are: push, web, trigger, schedule, api, and pipeline. For pipelines created before GitLab 9.5, this will show as unknown |
| CI_PIPELINE_TRIGGERED | all | all | The flag to indicate that job was triggered |
| CI_PIPELINE_URL | 11.1 | 0.5 | Pipeline details URL |
| CI_PROJECT_DIR | all | all | The full path where the repository is cloned and where the job is run |
| CI_PROJECT_ID | all | all | The unique id of the current project that GitLab CI uses internally |
| CI_PROJECT_NAME | 8.10 | 0.5 | The project name that is currently being built (actually it is project folder name) |
| CI_PROJECT_NAMESPACE | 8.10 | 0.5 | The project namespace (username or groupname) that is currently being built |
| CI_PROJECT_PATH | 8.10 | 0.5 | The namespace with project name |
| CI_PROJECT_PATH_SLUG | 9.3 | all | $CI_PROJECT_PATH lowercased and with everything except 0-9 and a-z replaced with -. Use in URLs and domain names. |
| CI_PROJECT_URL | 8.10 | 0.5 | The HTTP address to access project |
| CI_PROJECT_VISIBILITY | 10.3 | all | The project visibility (internal, private, public) |
| CI_REGISTRY | 8.10 | 0.5 | If the Container Registry is enabled it returns the address of GitLab’s Container Registry |
| CI_REGISTRY_IMAGE | 8.10 | 0.5 | If the Container Registry is enabled for the project it returns the address of the registry tied to the specific project |
| CI_REGISTRY_PASSWORD | 9.0 | all | The password to use to push containers to the GitLab Container Registry |
| CI_REGISTRY_USER | 9.0 | all | The username to use to push containers to the GitLab Container Registry |
| CI_REPOSITORY_URL | 9.0 | all | The URL to clone the Git repository |
| CI_RUNNER_DESCRIPTION | 8.10 | 0.5 | The description of the runner as saved in GitLab |
| CI_RUNNER_EXECUTABLE_ARCH | all | 10.6 | The OS/architecture of the GitLab Runner executable (note that this is not necessarily the same as the environment of the executor) |
| CI_RUNNER_ID | 8.10 | 0.5 | The unique id of runner being used |
| CI_RUNNER_REVISION | all | 10.6 | GitLab Runner revision that is executing the current job |
| CI_RUNNER_TAGS | 8.10 | 0.5 | The defined runner tags |
| CI_RUNNER_VERSION | all | 10.6 | GitLab Runner version that is executing the current job |
| CI_SERVER | all | all | Mark that job is executed in CI environment |
| CI_SERVER_NAME | all | all | The name of CI server that is used to coordinate jobs |
| CI_SERVER_REVISION | all | all | GitLab revision that is used to schedule jobs |
| CI_SERVER_VERSION | all | all | GitLab version that is used to schedule jobs |
| CI_SERVER_VERSION_MAJOR | 11.4 | all | GitLab version major component |
| CI_SERVER_VERSION_MINOR | 11.4 | all | GitLab version minor component |
| CI_SERVER_VERSION_PATCH | 11.4 | all | GitLab version patch component |
| CI_SHARED_ENVIRONMENT | all | 10.1 | Marks that the job is executed in a shared environment (something that is persisted across CI invocations like shell or ssh executor). If the environment is shared, it is set to true, otherwise it is not defined at all. |
| GET_SOURCES_ATTEMPTS | 8.15 | 1.9 | Number of attempts to fetch sources running a job |
| GITLAB_CI | all | all | Mark that job is executed in GitLab CI environment |
| GITLAB_USER_EMAIL | 8.12 | all | The email of the user who started the job |
| GITLAB_USER_ID | 8.12 | all | The id of the user who started the job |
| GITLAB_USER_LOGIN | 10.0 | all | The login username of the user who started the job |
| GITLAB_USER_NAME | 10.0 | all | The real name of the user who started the job |
| RESTORE_CACHE_ATTEMPTS | 8.15 | 1.9 | Number of attempts to restore the cache running a job |

<br>

**GitLab 9.0 renaming**

| 8.x name | 9.0+ name |
| - | - |
| CI_BUILD_ID | CI_JOB_ID |
| CI_BUILD_REF | CI_COMMIT_SHA |
| CI_BUILD_TAG | CI_COMMIT_TAG |
| CI_BUILD_BEFORE_SHA | CI_COMMIT_BEFORE_SHA |
| CI_BUILD_REF_NAME | CI_COMMIT_REF_NAME |
| CI_BUILD_REF_SLUG | CI_COMMIT_REF_SLUG |
| CI_BUILD_NAME | CI_JOB_NAME |
| CI_BUILD_STAGE | CI_JOB_STAGE |
| CI_BUILD_REPO | CI_REPOSITORY_URL |
| CI_BUILD_TRIGGERED | CI_PIPELINE_TRIGGERED |
| CI_BUILD_MANUAL | CI_JOB_MANUAL |
| CI_BUILD_TOKEN | CI_JOB_TOKEN |



<br/>
<br/>


#### `.gitlab-ci.yml`定义的变量

GitLab CI允许您添加在构建环境中设置的`.gitlab-ci.yml`变量。因此，变量保存在存储库中，它们用于存储非敏感项目配置。



<br/>
<br/>


#### 组/库级别变量


这个变量在Web UI上进行配置。




<br/>
<br/>
<br/>



### `.gitlab-ci.yml`配置

使用`.gitlab-ci.yml`配置你的Jobs，该文件是GitLab Runner用来管理项目作业的文件。


<br/>


#### Jobs

YAML文件定义了一组具有约束的作业，说明应该何时运行它们。您可以指定无限数量的作业，这些作业被定义为具有任意名称的顶级元素，并且始终必须至少包含`script`子句。
可以是直接运行命令，也可以写成`xxx.sh`脚本，然后执行此脚本。

```yaml
#两个单独的作业，执行各自的命令
job1:
  script: "execute-script-for-job1"

job2:
  script: "execute-script-for-job2"
```

Runner选择Job并在Runner的环境中执行。重要的是，每项工作都是相互独立运作的，这里可对比Jenkins里面的workspace。

每个作业必须具有唯一的名称，但有一些**保留的关键字(keywords)**不能用作作业名称:

- `image`
- `services`
- `stages`
- `types`
- `before_script`
- `after_script`
- `variables`
- `cache`

<br>

**作业由定义作业行为的参数列表定义:**

| Keyword | Required | Description |
| - | - | - |
| `script` | yes | Defines a shell script which is executed by Runner |
| `extends` | no | Defines a configuration entry that this job is going to inherit from |
| `image` | no | Use docker image, covered in Using Docker Images |
| `services` | no | Use docker services, covered in Using Docker Images |
| `stage` | no | Defines a job stage (default: `test`) |
| `type` | no | Alias for `stage` |
| `variables` | no | Define job variables on a job level |
| `only` | no | Defines a list of git refs for which job is created |
| `except` | no | Defines a list of git refs for which job is not created |
| `tags` | no | Defines a list of tags which are used to select Runner |
| `allow_failure` | no | Allow job to fail. Failed job doesn’t contribute to commit status |
| `when` | no | Define when to run job. Can be `on_success`, `on_failure`, `always` or `manual |
| `dependencies` | no | Define other jobs that a job depends on so that you can pass artifacts between them |
| `artifacts` | no | Define list of job artifacts |
| `cache` | no | Define list of files that should be cached between subsequent runs |
| `before_script` | no | Override a set of commands that are executed before job |
| `after_script` | no | Override a set of commands that are executed after job |
| `environment` | no | Defines a name of environment to which deployment is done by this job |
| `coverage` | no | Define code coverage settings for a given job |
| `retry` | no | Define when and how many times a job can be auto-retried in case of a failure |
| `parallel` | no | Defines how many instances of a job should be run in parallel |


<br/>
<br/>


#### `extends`

`extends`定义了一个使用`extends`的作业将继承的条目名称。

这是使用YAML锚点(anchor)的替代方案，并且更加灵活和可读：

```yaml
.tests:
  script: rake test
  stage: test
  only:
    refs:
      - branches

rspec:
  extends: .tests
  script: rake rspec
  only:
    variables:
      - $RSPEC
```

在上面的示例中，rspec作业继承自`.tests`模板作业。GitLab将根据键执行反向深度合并。GitLab将:

- 将`rspec`内容以递归方式合并到`.tests`中
- Not merge the values of the keys

这导致以下`rspec`作业:

```yaml
#注意，script: rake test将被script: rake rspec覆盖
rspec:
  script: rake rspec
  stage: test
  only:
    refs:
      - branches
    variables:
      - $RSPEC
```

如果想要包含`rake test`, 请查看`before_script-and-after_script`.
`extends`支持多级继承，但不建议使用三级以上。支持的最大嵌套级别为10。

一下栗子具有两级继承:

```yaml
.tests:
  only:
    - pushes

.rspec:
  extends: .tests
  script: rake rspec

rspec 1:
  variables:
    RSPEC_SUITE: '1'
  extends: .rspec

rspec 2:
  variables:
    RSPEC_SUITE: '2'
  extends: .rspec

spinach:
  extends: .tests
  script: rake spinach
```


<br/>
<br/>


#### `pages`

`pages`是一项特殊工作，用于将静态内容上传到GitLab，可用于为您的网站提供服务。它有一个特殊的语法，因此必须满足以下两个要求：

- 任何静态内容都必须放在`public/`目录下
- 须定义具有`public/`目录路径的`artifacts`

```yaml
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - master
```

更多详细信息请参考GitLab Pages。


<br/>
<br/>


#### `image` and `services`

这允许指定自定义Docker镜像和可用于作业时间的服务列表。


<br/>
<br/>


#### `before_script` and `after_script`

`before_script`用于定义应在所有作业（包括部署作业）之前，在恢复工件(artifacts)之后，运行的命令；这可以是数组或多行字符串。
`after_script`用于定义将在所有作业（包括失败的作业）之后运行的命令。这必须是数组或多行字符串。

`before_script`和`main script`连接在一个上下文/容器中运行。`after_script`是单独运行的，因此根据执行程序，在工作树之外完成的更改可能不可见。

如果在每个工作中定义了`before_script`和`after_script`，则可以覆盖全局定义：

```yaml
before_script:
  - global before script

job:
  before_script:
    - execute this instead of global before script
  script:
    - my command
  after_script:
    - execute this after my script
```


<br/>
<br/>


#### `stages`

`stages`用于在全局范围定义可由作业使用的阶段。
`stages`规范允许具有灵活的多级阶段管道(multi stage pipeline)。`stages`元素的排序定义了作业执行的顺序:

- 同一阶段的作业是并行运行的
- 下一阶段的作业在上一阶段的作业成功完成之后运行

让我们考虑以下示例，它定义了3个阶段：

```yaml
stages:
  - build
  - test
  - deploy


#首先，build阶段的所有作业都是并行执行的
#如果build阶段的所有作业都成功，则test阶段的作业将并行执行
#如果test阶段的所有作业都成功，则deploy阶段的作业将并行执行
#如果deploy阶段的所有作业都成功，则commit将被标记为passwd
#如果任何先前的作业失败，则commit被标记为failed，并且不执行其他阶段的作业
```

有两个边缘案例值得注意：

- 如果在`.gitlab-ci.yml`文件中没有定义`stages`，`build`、`test`和`deploy`用作默认情况允许的作业阶段；
- 如果作业未指定`stage`，则为作业分配`test`阶段


<br/>
<br/>


#### `stage`

`stage`是按工作定义的，依赖于全局定义的`stages`。它允许将作业分组到不同的阶段，并且同一阶段的作业并行执行:

```yaml
stages:
  - build
  - test
  - deploy

job 1:
  stage: build
  script: make build dependencies

job 2:
  stage: build
  script: make build artifacts

job 3:
  stage: test
  script: make test

job 4:
  stage: deploy
  script: make deploy
```

<br/>
<br/>


#### `types`

不推荐使用`types`，可以在以后的某个版本中删除。请使用`stages`替代它。


<br/>
<br/>


#### `script`

`script`是作业所需的唯一必需关键字。这是一个由Runner执行的shell script。

```yaml
#栗子
job:
  script: "bundle exec rspec"
```

此参数还可以包含使用数组的多个命令：

```yaml
job:
  script:
    - uname -a
    - bundle exec rspec
```

有时，脚本命令需要用单引号或双引号括起来，例如命令中有特殊字符的时候。


<br/>
<br/>


#### `only`和`except`(简单)

`only`和`except`两个参数，用于创建作业时设置作业策略来限制它:

- `only`定义作业将运行的branch和tag的名称
- `except`定义作业不会运行的branch和tag的名称

有一些适用于作业策略的规则：

- `only`和`except`是包容性的，如果在作业规范中定义了`only`和`except`，则ref被`only`和`except`过滤
- `only`和`except`允许使用正则表达式: [Ruby regexp syntax](https://ruby-doc.org/core-2.6/Regexp.html)
- `only`和`except`允许指定一个Repo path来为forks过滤作业

另外，`only`和`except`允许使用如下关键字:

| Value | Description |
| - | - |
| `branches` | When a git reference of a pipeline is a branch |
| `tags` | When a git reference of a pipeline is a tag |
| `api` | When pipeline has been triggered by a second pipelines API (not triggers API) |
| `external` | When using CI services other than GitLab |
| `pipelines` | For multi-project triggers, created using the API with `CI_JOB_TOKEN` |
| `pushes` | Pipeline is triggered by a `git push` by the user |
| `schedules` | For scheduled pipelines |
| `triggers` | For pipelines created using a trigger token |
| `web` | For pipelines created using Run pipeline button in GitLab UI (under your project’s Pipelines) |
| `merge_requests` | When a merge request is created or updated  |

<br>

```yaml
#job将仅针对以issue-开头的refs运行，而所有分支都将被跳过
job:
  # use regexp
  only:
    - /^issue-.*$/
  # use special keyword
  except:
    - branches


#job将仅对tagged refs
job:
  # use special keywords
  only:
    - tags
    - triggers
    - schedules



#repo path可用于仅为parent repo而不是forks执行作业
#将为除了master的gitlab-org/gitlab-ce上的所有分支运行job
job:
  only:
    - branches@gitlab-org/gitlab-ce
  except:
    - master@gitlab-org/gitlab-ce
```

<br>

如果作业既没有`only`也没有`except`规则，则默认设置为`only: ['branches', 'tags']`:

```yaml
#未配置
job:
  script: echo 'test'


#它被转换为下面这个样子
job:
  script: echo 'test'
  only: ['branches', 'tags']
```


<br/>
<br/>


#### `only`和`except`(复杂)

> - `refs` and `kubernetes` policies introduced in GitLab 10.0.
> - `variables` policy introduced in GitLab 10.7.
> - `changes` policy introduced in GitLab 11.4.

> 这是一个alpha功能，它可能随时更改，恕不另行通知！

GitLab支持简单和复杂的策略，因此可以使用数组和哈希配置方案。
提供了4个key:

- refs
- variables
- changes
- kubernetes

可以使用`AND`组合多可键。

<br>

- `only: refs`
- `except: refs`

```yaml
deploy:
  only:
    refs:
      - master
      - schedules
```

<br>

- `only: kubernetes`
- `except: kubernetes`

```yaml
#kubernetes策略只接受active关键字
deploy:
  only:
    kubernetes: active
```

<br>

- `only: variables`
- `except: variables`

```yaml
#variables关键字用于定义变量表达式
deploy:
  script: cap staging deploy
  only:
    refs:
      - branches
    variables:
      - $RELEASE == "staging"
      - $STAGING
```

<br>

- `only: changes`
- `except: changes`

```yaml
#是否应该根据git push事件修改的文件来创建作业
docker build:
  script: docker build -t my-image:$CI_COMMIT_REF_SLUG .
  only:
    changes:
      - Dockerfile
      - docker/scripts/*
      - dockerfiles/**/*
      - more_scripts/*.{rb,py,sh}

#这个例子中，只要有上面几个文件或文件夹内的内容发生了commit push
```


<br/>
<br/>


#### `tags`

`tags`从允许运行此项目的所有Runner中选择特定Runner。在注册Runner期间，您可以指定Runner的tag。
`tags`允许你使用分配了特定标签的Runner运行作业。

```yaml
job:
  tags:
    - ruby
    - postgres


#栗子
windows job:
  stage:
    - build
  tags:
    - windows
  script:
    - echo Hello, %USERNAME%!

osx job:
  stage:
    - build
  tags:
    - osx
  script:
    - echo "Hello, $USER!"
```


<br/>
<br/>


#### `allow_failure`

`allow_failure`允许作业失败而不会影响CI套件的其余部分。除手动作业外，默认值为`false`。
启用并且作业失败后，作业将在UI中显示橙色警告。但是，管道的逻辑流程将认为作业成功/通过，并且不会被阻止。假设所有其它作业都成功，作业的阶段及其管道将显示相同的橙色警告。但是，关联的提交将被标记为`passed`，而不会发出警告。

```yaml
job1:
  stage: test
  script:
    - execute_script_that_will_fail
  allow_failure: true

job2:
  stage: test
  script:
    - execute_script_that_will_succeed

job3:
  stage: deploy
  script:
    - deploy_to_staging
```


<br/>
<br/>


#### `when`

`when`用于实现在发生故障或尽管失败时运行的作业，它有以下值:

- `on_success`： 只有当前几个阶段的所有工作都成功时才执行工作
- `on_failure`： 仅当前一阶段中的至少一个作业失败时才执行作业
- `always`： 无论先前阶段的工作状态如何，都可以执行工作
- `manual`： 手动执行作业
- `delayed`： 延迟执行作业(GitLab v11.4)

```yaml
stages:
  - build
  - cleanup_build
  - test
  - deploy
  - cleanup

build_job:
  stage: build
  script:
    - make build

cleanup_build_job:
  stage: cleanup_build
  script:
    - cleanup build when failed
  when: on_failure

test_job:
  stage: test
  script:
    - make test

deploy_job:
  stage: deploy
  script:
    - make deploy
  when: manual

cleanup_job:
  stage: cleanup
  script:
    - cleanup after jobs
  when: always
```

手动操作是一种特殊类型的作业，不会自动执行，需要由用户明确启动。(例如，部署到生产环境)
手动操作可以是可选的也可以是阻止的。阻止手动操作将在定义此操作的阶段阻止管道的执行。当有人通过单击播放按钮执行阻止手动操作时，可以继续执行管道。默认情况下，手动操作是非阻止的。如果要阻止手动操作，则需要添加`allow_failure：false`。
手动操作被视为写入操作，因此当用户想要触发操作时，将使用受保护分支的权限。换句话说，为了触发分配给管道运行的分支的手动操作，用户需要具有合并到该分支的能力。

<br>

`when: delayed`，延迟作业用于在一段时间后执行脚本。如果要避免作业立即进入暂挂(`pending`)状态，这非常有用。
你可以使用`start_in`键来设置时期，它的值是以秒(s)为单位的经过时间，或者你提供时间单位，它的值必须小于等于一小时。
当阶段中的作业延迟时，管道将不会进展，直到延迟作业完成。这意味着此关键字也可用于在不同阶段之间插入延迟。
延迟作业的计时器在前一阶段完成后立即开始。与其他类型的作业类似，除非前一阶段过去，否则延迟作业的计时器将无法启动。

```
#10 seconds
#30 minutes
#1 hour

#栗子
timed rollout 10%:
  stage: deploy
  script: echo 'Rolling out 10% ...'
  when: delayed
  start_in: 30 minutes
```

您可以通过单击**Unschedule**按钮来停止延迟作业的活动计时器。除非您手动执行作业，否则将来不会执行此作业。
您可以通过单击**Play**按钮立即开始延迟作业。 GitLab Runner很快就会选择你的工作并开始工作。


<br/>
<br/>


#### environment

`environment`用于定义作业部署到特定环境。如果指定了`environment`且该名称下没有环境，则将自动创建一个新环境。
它有如下几个值:

- `name`
- `url`
- `on_stop`
- `action`


```yaml
#常见的名字有qa, staging, production
#但你可以为你的工作流使用任何名称
deploy to production:
  stage: deploy
  script: git push production HEAD:master
  environment:
    name: production



#url是一个可选值
#在设置时，它会在GitLab中的各个位置公开按钮，单击这些按钮会转到定义的URL
#如果作业成功完成，它将在合并请求和environments/deployments页面中创建指向url的按钮
deploy to production:
  stage: deploy
  script: git push production HEAD:master
  environment:
    name: production
    url: https://prod.example.com



#on_stop来实现closing(stopping)环境。它声明了一个不同的工作，以便关闭环境
#action与on_stop一起使用，在被调用以关闭环境的作业中定义
review_app:
  stage: deploy
  script: make deploy-app
  environment:
    name: review
    on_stop: stop_review_app

stop_review_app:
  stage: deploy
  script: make delete-app
  when: manual
  environment:
    name: review
    action: stop
```


<br/>
<br/>


#### Dynamic environments

```yaml
deploy as review app:
  stage: deploy
  script: make deploy
  environment:
    name: review/$CI_COMMIT_REF_NAME
    url: https://$CI_ENVIRONMENT_SLUG.example.com/
```


<br/>
<br/>


#### `cache`

> **Notes:**
> - Introduced in GitLab Runner v0.7.0
> - cache can be set globally and per-job
> - From GitLab 9.0, caching is enabled and shared between pipelines and jobs by default
> -From GitLab 9.2, caches are restored before artifacts

`cache`用于指定应在作业之间缓存的文件和目录列表，您只能使用项目工作区内的路径。
如果在作业范围之外定义了`cache`，则表示它是全局设置的，并且所有作业都将使用该定义。

它的几个值:

- `paths`
- `key`
- `untracked`
- `policy`

```yaml
#paths指令选择要缓存的文件或目录。支持通配符
rspec:
  script: test
  cache:
    paths:
      - binaries/*.apk
      - .config



#由于cache是在作业之间共享的，如果对不同的作业使用不同的路径，则还应设置不同的cache:key，否则缓存内容可以被覆盖
#key指令允许您定义作业之间的缓存关联，允许为所有作业提供single cache，cache per-job，cache per-branch或适合您工作流的任何其他方式
cache:
  key: "$CI_COMMIT_REF_SLUG"
  paths:
    - binaries/



#untracked：true缓存Git存储库中未跟踪的所有文件
rspec:
  script: test
  cache:
    untracked: true
    paths:
      - binaries/



#policy的默认行为是在执行开始时下载文件，并在结束时重新上载它们
#这允许将作业所做的任何更改保留以供将来运行，并称为pull-push缓存策略
#这有助于加快作业执行速度并减少缓存服务器上的负载，尤其是当您有大量并行执行缓存的作业时。
stages:
  - setup
  - test

prepare:
  stage: setup
  cache:
    key: gems
    paths:
      - vendor/bundle
  script:
    - bundle install --deployment

rspec:
  stage: test
  cache:
    key: gems
    paths:
      - vendor/bundle
    policy: pull
  script:
    - bundle exec rspec ...
```


<br/>
<br/>


#### `artifacts`

> **Notes:**
> - Introduced in GitLab Runner v0.7.0 for non-Windows platforms.
> - Windows support was added in GitLab Runner v.1.0.0.
> - From GitLab 9.2, caches are restored before artifacts.
> - Not all executors are supported.
> - Job artifacts are only collected for successful jobs by default.

`artifacts`用于指定成功后应附加到作业的文件和目录列表。作业成功完成后，工件将被发送到GitLab，并可在GitLab UI中下载。

有以下值:

- `paths`
- `name`
- `untracked`
- `when`
	- `when: on_success`
	- `when: on_failure`
	- `when: always`
- `expire_in`
- `reports`
	- `reports:junit`
	- `reports:codequality`
	- `reports:sast`
	- `reports:dependency_scanning`
	- `reports:container_scanning`
	- `reports:dast`
	- `reports:license_management`
	- `reports:performance`

<br>

```yaml
#artifacts:paths
#要在不同作业之间传递工件，只能使用项目工作区内的路径
default-job:
  script:
    - mvn test -U
  except:
    - tags

release-job:
  script:
    - mvn package -U
  artifacts:
    paths:
      - target/*.war
  only:
    - tags



#artifacts:name
#定义创建的工件归档的名称
job:
  artifacts:
    name: "$CI_JOB_NAME"
    paths:
      - binaries/



#artifacts:untracked
#用于将所有Git未跟踪文件添加为工件
artifacts:
  untracked: true
  paths:
    - binaries/



#artifacts:when
#用于在作业失败时上传工件
job:
  artifacts:
    when: on_failure



#artifacts:expire_in
#允许您指定工件在到期之前应该存在多长时间并因此被删除，从它们上载和存储在GitLab上的时间开始计算
#如果未定义到期时间，则默认为实例范围设置(默认30天)
#到期后，每小时定时任务删除工件
#默认单位是秒，支持提供时间单位

#‘42’
#‘3 mins 4 sec’
#‘2 hrs 20 min’
#‘2h20min’
#‘6 mos 1 day’
#‘47 yrs 6 mos and 4d’
#‘3 weeks and 2 days’

job:
  artifacts:
    expire_in: 1 week




#artifacts:reports
#用于从工作中收集测试报告并在GitLab UI中公开它们
```


<br/>
<br/>


#### `dependencies`

此功能应与`artifacts`结合使用，并允许您定义要在不同作业之间传递的工件
要使用此功能，请在作业上下文中定义`dependencies`，并传递应从中下载工件的所有先前作业的列表。

如果作为依赖项设置的作业的工件已过期或已擦除，则相关作业将失败。

```yaml
#当执行test:osx时，将在构建的上下文中下载并提取build:osx中的工件
#test:linux也是如此，要从build: linux拉取工件

build:osx:
  stage: build
  script: make build:osx
  artifacts:
    paths:
      - binaries/

build:linux:
  stage: build
  script: make build:linux
  artifacts:
    paths:
      - binaries/

test:osx:
  stage: test
  script: make test:osx
  dependencies:
    - build:osx

test:linux:
  stage: test
  script: make test:linux
  dependencies:
    - build:linux

deploy:
  stage: deploy
  script: make deploy
```


<br/>
<br/>


#### `coverage`

允许您配置从作业输出中提取代码覆盖率的方式，正则表达式是此处唯一有效的值。

```yaml
job1:
  script: rspec
  coverage: '/Code coverage: \d+\.\d+/'
```


<br/>
<br/>


#### `retry`

允许您配置在发生故障时重试作业的次数。如果重试作业成功完成，则不会再进行剩余的重试。它的值为`2>=retry>=0`的正整数。

要更好的控制retry，可使用以下key:

- `max`: 最大重试次数
- `when`: 败的情况下重试
	- `always`: 重试任何失败 (default)
	- `unknown_failure`: 失败原因未知时重试
	- `script_failure`: 脚本失败时重试
	- `api_failure`: API失败重试
	- `stuck_or_timeout_failure`: 当作业卡住或超时时重试
	 - `runner_system_failure`: 如果Runner故障，重试
	- `missing_dependency_failure`: 如果缺少依赖项，重试
	- `runner_unsupported`: 如果Runner不受支持，重试

```yaml
test:
  script: rspec
  retry: 2


test:
  script: rspec
  retry:
    max: 2
    when:
      - runner_system_failure
      - stuck_or_timeout_failure
```


<br/>
<br/>


#### `parallel`

允许您配置并行运行的作业实例数，它的值`50>=parallel>=2`。

```yaml
#简单栗子
test:
  script: rspec
  parallel: 5
```


<br/>
<br/>


#### `include`

> Introduced in GitLab Premium 10.5

使用`include`，可以允许包含外部YAML文件(本地Repo或远程URL)，但也需要为`.yml`和`.yaml`扩展格式。

```yaml
# Content of .gitlab-ci.yml

include: 'https://gitlab.com/awesome-project/raw/master/.before-script-template.yml'

rspec:
  script:
    - bundle exec rspec
```


<br/>
<br/>


#### `variables`

> 整数(浮点数)对于变量是有效的，浮点数无效。

GitLab CI/CD允许你在`.gitlab-ci.yml`中定义变量，然后在作业环境中传递。变量可以是全局的，也可以是基于每个作业的。当在作业级别定义了与全局或项目相同名称的变量时，则作业级别的变量会覆盖它们。

```yaml
variables:
  DATABASE_URL: "postgres://postgres@postgres/my_database"
```


<br/>
<br/>


#### 特殊YAML功能

可使用特殊的YAML功能，如锚点(anchors`&`)、别名(aliases`*`)、map merging (`<<`)等，这大大降低了`.gitlab-ci.yml`的复杂性。


<br/>


##### Hidden keys (jobs)

如果要暂时禁用作业，而不是注释掉定义作业的所有行，你可是在作业名前加一个点(`.`)，这样GitLab CI将会忽略它。

```yaml
#hidden_job:
#  script:
#    - run test



.hidden_job:
  script:
    - run test
```


<br/>
<br/>


##### Anchors

YAML的锚点功能此处就不赘述了。

```yaml
#使用锚点和map merging
.job_template: &job_definition
  script:
    - test project

.postgres_services:
  services: &postgres_definition
    - postgres
    - ruby

.mysql_services:
  services: &mysql_definition
    - mysql
    - ruby

test:postgres:
  <<: *job_definition
  services: *postgres_definition

test:mysql:
  <<: *job_definition
  services: *mysql_definition

---

#扩展应该为这个样子
.job_template:
  script:
    - test project

.postgres_services:
  services:
    - postgres
    - ruby

.mysql_services:
  services:
    - mysql
    - ruby

test:postgres:
  script:
    - test project
  services:
    - postgres
    - ruby

test:mysql:
  script:
    - test project
  services:
    - mysql
    - ruby
```



<br/>
<br/>
<br/>



### Pipelines for MR

[Pipelines for merge requests](https://docs.gitlab.com/ce/ci/merge_request_pipelines/)

> **Note:** 从GitLab v11.10开始，由于`recent refspecs changes`，Pipeline for merge requests需要GitLab Runner v11.9+。任何较低版本将导致Pipeline失败。
> Introduced in GitLab 11.6.

<br>

通常，在创建新的合并请求(MR)时，管道(pipeline)会使用新的更改运行，并检查它是否有资格合并到目标分支中。此管道仅包含用于验证新更改的必要作业。例如，在此周期中使用单元测试...

使用合并请求的管道，你可以在合并请求中运行管道时设计特定的管道结构。这可能是添加或删除管道中的步骤，以确保管道尽可能高效。


<br/>
<br/>


#### 为MR配置管道

Configuring pipelines for merge requests

要为合并请求配置管道，请将`only: merge_requests`参数添加到要仅为合并请求运行的作业。
然后，当开发人员每次创建或更新合并请求时，每次将提交推送到GitLab时都会运行管道。

<br>

> **Note:**  If you use this feature with `merge when pipeline succeeds`, pipelines for merge requests take precedence over the other regular pipelines.

<br>

栗子`.gitlab-ci.yml`：

```yaml
build:
  stage: build
  script: ./build
  only:
  - master

test:
  stage: test
  script: ./test
  only:
  - merge_requests
# multi conditions
# only:
# - test
# - merge_requests
# - tags
# only默认是或操作，上面三个条件即是 test OR merge_requests OR tags
# 下面介绍如何使用不并操作
# only:
# - test && tags
# - test AND $name == 'bingo'
# - test AND NOT $name == 'zhang'

deploy:
  stage: deploy
  script: ./deploy
  only:
  - master
```

使用新提交(commit)更新合并请求后：

- GitLab检测到已发生更改并为合并请求创建新管道
- 管道从源分支获取(fetch)最新代码并对其进行测试

Pipelines tagged with the **detached** badge indicate that they were triggered when a merge request was created or updated.

![](/images/GitLab/merge_request.png)


<br/>
<br/>


#### Pipelines for Merged Results

> Introduced in GitLab Premium 11.10. This feature is disabled by default until we resolve issues with contention handling, but can be enabled manually.

商业版的功能，此处不介绍。



<br/>
<br/>


#### Merge when pipeline succeeds

查看准备合并但仍有一个或多个CI作业运行的合并请求时，可以将其设置为在作业管道成功时自动合并(merged automatically)。这样，你不必等待作业完成，并记住去手动合并请求。

![](/images/GitLab/merge_when_pipeline_succeeds_enable.png)


<br/>

##### 如何工作

当您点击**管道成功时合并(Merge When Pipeline Succeeds)**按钮时，将更新合并请求的状态以表示即将发生的合并。如果你无法等待管道成功并希望立即合并，则可以在配置中启用此选项。

开发者和MR发起者都可以选择取消自动合并，如果他们找到了不应该自动合并的原因。

![](/images/GitLab/merge_when_pipeline_succeeds_status.png)

管道成功后，合并请求将自动合并。当管道发生故障时，MR作者有机会重试任何失败的作业，或推送新的提交来修复失败。
在第二次尝试作业并成功时，合并请求将自动合并。使用新提交更新合并请求时，将自动取消自动合并以允许检查新更改。


<br/>
<br/>


##### Only allow MR to be merged if the pipeline succeeds

如果合并请求的管道未成功或有待解决的问题，则可以阻止合并请求的合并。

在`项目->设置->通用->合并请求->Merge checks`中，选择**Pipelines must succeed**，单击保存以生效。

![](/images/GitLab/merge_when_pipeline_succeeds_only_if_succeeds_settings.png)

现在开始，每个管道发生故障时，你都无法合并Web UI中的合并请求，知道你通过所有相关的作业为止。

![](/images/GitLab/merge_when_pipeline_succeeds_only_if_succeeds_msg.png)




<br/>
<br/>
<br/>



### Scheduling Pipelines


我们也可以使用定时任务的方式来触发Pipeline。在`项目->CI/CD->计划(Sheduling)`里面配置流水线计划，来定时运行Pipeline。

![](/images/GitLab/scheduling_pipeline.png)



<br/>
<br/>
<br/>



### 在GitLab CI中使用git submodules

[Using Git submodules with GitLab CI](https://docs.gitlab.com/ee/ci/git_submodules.html)


> 注意：
> GitLab 8.12 introduced a new CI job permissions model and you are encouraged to upgrade your GitLab instance if you haven’t done already. If you are not using GitLab 8.12 or higher, you would need to work your way around submodules in order to access the sources of e.g., `gitlab.com/group/project` with the use of SSH keys.
> With GitLab 8.12 onward, your permissions are used to evaluate what a CI job can access. More information about how this system works can be found in the Jobs permissions model.（CI jobs 默认有权限拉取所有项目）
> The HTTP(S) Git protocol must be enabled in your GitLab instance.


<br/>


好像之前需要先关联子模块，否则GitLab里面的`.gitmodules`它不生效。这个在你配置的时候先不做，如果不生效再来坐这一步。

```
# project 关联 project-01
git submodule add git@git.xxx.com/groupB/project-01.git
```


<br/>
<br/>


#### 配置`.gitmodules`文件

如果要处理Git submodules，你的项目需要一个名为`.gitmodules`的文件。

如果你使用GitLab v8.12+，并且你的子模块也位于同一GitLab Server上，则必须更新`.gitmodules`文件以使用**relative URLs**。由于Git允许使用`.gitmodules`配置相对URL，因此你可以轻松使用HTTP(S)克隆所有CI作业，并使用SSH进行所有本地`checkout`。

项目在同一个GitLab Server的`.gitmodules`示例：

```
[submodule "project-01"]
  path = project-01
  url = ../groupB/project-01.git
  branch = test

[submodule "project-02"]
  path = project-02
  url = ../project-02.git
  branch = master
```

上述配置将指示Git自动推断克隆源时应使用的URL。

<br>

对于不在同一GitLab Server上的其它子模块，请使用完整的HTTP(S)协议的URL：

```
[submodule "project-x"]
  path = project-x
  url = https://gitserver.com/group/project-x.git
  branch = master
```

<br>

一旦正确配置了`.gitmodules`，你就可以去配置`.gitlab-ci.yml`。


<br/>
<br/>


#### 在CI中使用git submodules

1. 首先，确保你已使用位于同一GitLab Server中的子模块的相对URL
2. 如果你使用GitLab Runner v1.10+，你可将`GIT_SUBMODULE_STRATEGY`变量设置为`normal`或`recursive`，以告知Runner在作业之前获取子模块

```yaml
variables:
  GIT_SUBMODULE_STRATEGY: recursive


before_script:
  - git submodule sync --recursive
  - git submodule update --init --recursive --remote
  #- git submodule update --init --recursive
  # 如果你使用旧版本的GitLab Runner，使用 git submodule sync/update
  # --recursive should be used in either both or none (sync/update) depending on whether you have recursive submodules
  # - git submodule sync/update
```

在`before_script`中设置同步和更新的基本原理是由于Git子模块的工作方式。在新的Runner Workspace中，Git将根据`.gitmodules`和当前远程URL设置`.git/config`中包含子模块的URL。

```
cat .git/config

[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@git.xxx.com:groubA/test/project.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[submodule "project-01"]
        url = git@git.xxx.com:groupB/project-01.git
```

这里面显示的是子模块的完整URL。

<br>

最后，在GitLab Web UI中看一下显示效果。

![](/images/GitLab/gitSubmodules.png)



<br/>
<br/>
<br/>



### GitLab CI/CD栗子

各种语言、框架 、操作系统 CI/CD栗子: <https://docs.gitlab.com/ce/ci/examples/README.html>

- PHP
- Ruby
- Python
- Java
- Scala
- Clojure
- Elixir
- IOS and MacOS
- Android
- Debian
- Maven

<br>












<br/>
<br/>

---

<br/>
<br/>




## Git配置项

Git configuration options


<br/>


### 自定Git hooks

Custom Git hooks: Custom Git hooks (on the filesystem) for when webhooks aren’t enough


> **注意**：必须在GitLab服务器的文件系统上配置自定义Git hooks。只有GitLab服务器管理员才能完成这些任务。

Git本身支持在不同操作上执行的hooks。服务器端git hooks的示例包括预接收，后接收和更新。从gitlab-shell 2.2.0版（需要GitLab 7.5+）开始，GitLab管理员可以为任何GitLab项目添加自定义git hooks。


<br/>
<br/>


#### 配置

通常，Githooks放在存储库或项目的hooks目录中。 GitLab从每个项目的hooks目录创建一个符号链接到`gitlab-shell` hooks目录，以便于`gitlab-shell`升级之间的维护。因此，自定义挂钩的实现方式略有不同。但是，一旦创建了钩子，行为就完全相同了。

请按照以下步骤设置自定义hooks：

- 选择一个需要自定义Git hook的项目
- 在GitLab Server，导航到项目的存储库目录(如: `/var/opt/gitlab/git-data/repositories/user/xx.git`)
- 此位置创建名为`custom_hooks`的新目录
- 在`custom_hooks`目录中，创建一个名称与hook类型匹配的文件(如: `pre-hook`)
- 修改hook文件属主为git，添加可执行权限
- 编写代码以使Git hook函数按预期方式运行，可以是任何语言。确保顶部的`shebang`(`#!/bin/python3`)正确反映语言类型

假设正确实现了hook代码，hook将适当地触发。


<br/>
<br/>


#### 链式hook

Chained hooks support

> 在GitLab Shell 4.1.0和GitLab 8.15中引入

hook也可以放在`hook/<hook_name>.d`（全局）或`custom_hooks/<hook_name>.d`（每个项目）目录中，支持钩子的链式执行。
注意：`<hook_name>.d`需要`pre-receive.d`，`post-receive.d`或`update.d`才能正常工作。任何其他名称都将被忽略

要查看全局自定义hook（`hook/<hook_name.d>`）中的不同目录，请在`gitlab-shell config`中设置`custom_hooks_dir`。对于Omnibus安装，可在`gitlab.rb`中设置。

按以下顺序搜索并执行hook：

- `gitlab-shell/hooks` directory as known to Gitaly
- `<project>.git/hooks/<hook_name>` - executed by `git` itself, this is `gitlab-shell/hooks/<hook_name>`
- `<project>.git/custom_hooks/<hook_name>` - per project hook
- `<project>.git/custom_hooks/<hook_name>.d/*` - per project hooks
- `<project>.git/hooks/<hook_name>.d/* OR <custom_hooks_dir>/<hook_name.d>/*` - global hooks: all executable files


<br/>
<br/>


#### 自定义错误信息

Custom error messages

> 在GitLab 8.10中引入

如果commit被拒绝或在Git hook检查期间发生错误，则钩子的STDERR或STDOUT消息将出现在GitLab的UI中，STDERR优先于STDOUT。



<br/>
<br/>
<br/>



### Git LFS

- Git LFS: <https://docs.gitlab.com/ce/workflow/lfs/manage_large_binaries_with_git_lfs.html>
- Git LFS config: <https://docs.gitlab.com/ce/workflow/lfs/lfs_administration.html>

<br>

管理音频，视频和图形文件等大文件一直是Git的缺点之一。一般建议是不要让Git存储库大于1GB以保持性能。



<br/>
<br/>
<br/>



### Housekeeping

Housekeeping(管家): Keep your Git repositories tidy and fast

> 在GitLab 8.4中引入


<br/>


#### Automatic housekeeping

在Git push后，GitLab会自动在存储库上运行`git gc`和`git repack`命令。如果需要，您可以更改这种情况发生的频率，或者将其关闭。在Admin ares -> Setting


<br/>
<br/>


#### Manual housekeeping

housekeeping功能将运行`gc`还是`repack`，取决于你的设置。



<br/>
<br/>
<br/>



### Git协议

Configuring Git Protocol v2: Git protocol version 2 support

> 在GitLab 11.4中引入


Git第二版协议以多种方式改进了第一版协议，并且在GitLab中默认为HTTP请求启用。要为SSH启用，管理员需要进一步配置。

<br>

**Requirements：**

- 客户端，git v2.18.0+
- 服务端，如果要配置SSH，需要设置sshd以接受`GIT_PROTOCOL`环境变量

```
#/etc/ssh/sshd_config
AcceptEnv GIT_PROTOCOL


sudo service ssh restart



#配置新协议

#局部
git -c protocol.version=2

#全局
git config --global protocol.version 2




#验证

#HTTP

#C端
GIT_TRACE_CURL=1 git -c protocol.version=2 ls-remote https://your-gitlab-instance.com/group/repo.git 2>&1 | grep Git-Protocol

#S端
GIT_TRACE_PACKET=1 git -c protocol.version=2 ls-remote https://your-gitlab-instance.com/group/repo.git 2>&1 | head


#SSH

#C端
GIT_SSH_COMMAND="ssh -v" git -c protocol.version=2 ls-remote ssh://your-gitlab-instance.com:group/repo.git 2>&1 |grep GIT_PROTOCOL
```














<br/>
<br/>

---

<br/>
<br/>



## 监控

Monitoring GitLab































<br/>
<br/>

---

<br/>
<br/>







## 故障排除

Troubleshooting
























<br/>
<br/>

---

<br/>
<br/>





# Runner


**Runner**是`GitLab CI`的客户端。 作为GitLab持续集成和持续部署(CI/CD)的一部分，主要用来配置和运行构建脚本以及其他的任务。
GitLab Runner 是一个开源项目， 它用来运行你定制的任务（jobs）并把结果返回给 GitLab。 GitLab Runner配合GitLab CI（GitLab 内置的持续集成服务）协调完成任务。

<br>

**要求(Requirements)**

GitLab Runner是用Go编写的，可以作为单个二进制文件运行，不需要语言特定的要求。它可在多个操作系统上运行，只要你在此平台上编译成二进制文件。支持Docker v1.5+。

<br>

**特点(Feature)**

- Allows to run:
	+ multiple jobs concurrently(同时)
	+ use multiple tokens with multiple server (even per-project)
	+ limit number of concurrent(并发) jobs per-token
- Jobs can be run:
	+ locally
	+ using Docker containers
	+ using Docker containers and executing job over SSH
	+ using Docker containers with autoscaling on different clouds and virtualization hypervisors
	+ connecting to remote SSH server
- Is written in Go and distributed as single binary without any other requirements
- Supports Bash, Windows Batch and Windows PowerShell
- Works on GNU/Linux, OS X and Windows (pretty much anywhere you can run Docker)
- Allows to customize the job running environment
- Automatic configuration reload without restart
- Easy to use setup with support for Docker, Docker-SSH, Parallels or SSH running environments
- Enables caching of Docker containers
- Easy installation as a service for GNU/Linux, OSX and Windows
- Embedded Prometheus metrics HTTP server

<br>

**兼容性图表(Compatibility chart)**

GitLab Runner的版本应该与GitLab同步。如果存在版本差异，则功能可能无法使用或无法正常工作。



<br/>
<br/>
<br/>



## 安装

Install GitLab Runner


- Install using GitLab’s repository for Debian/Ubuntu/CentOS/RedHat (preferred)
- Install on GNU/Linux manually (advanced)
- Install on macOS
- Install on Windows
- Install as a Docker service
- Install in autoscaling mode using Docker machine
- Install on FreeBSD
- Install on Kubernetes
- Install the nightly binary manually (development)


<br/>


### Repository

Install GitLab Runner using the official GitLab repositories


**安装：**

```sh
#添加镜像库
# For Debian/Ubuntu/Mint
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash


# For RHEL/CentOS/Fedora
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash




#安装最新版
# For Debian/Ubuntu/Mint
sudo apt-get install gitlab-runner


# For RHEL/CentOS/Fedora
sudo yum install gitlab-runner




#安装制定版本
# for DEB based systems
apt-cache madison gitlab-runner
sudo apt-get install gitlab-runner=10.0.0


# for RPM based systems
yum list gitlab-runner --showduplicates | sort -r
sudo yum install gitlab-runner-10.0.0-1



#注册Runner
#注册Runner参考后面
```

<br>

**更新：**


```sh
# For Debian/Ubuntu/Mint
sudo apt-get update
sudo apt-get install gitlab-runner


# For RHEL/CentOS/Fedora
sudo yum update
sudo yum install gitlab-runner
```

<br>

**手动下载包安装**

下载地址: <https://packages.gitlab.com/runner/gitlab-runner>

<br>

**升级到GitLab Runner 10**

```sh
#移除旧库
# For Debian/Ubuntu/Mint
sudo rm /etc/apt/sources.list.d/runner_gitlab-ci-multi-runner.list


# For RHEL/CentOS/Fedora
sudo rm /etc/yum.repos.d/runner_gitlab-ci-multi-runner.repo



#安装新库


#再安装
```



<br/>
<br/>
<br/>



### 手动安装


```sh
#下载二进制包
# Linux x86-64
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Linux x86
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-386

# Linux arm
sudo wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm


#添加可执行权限
sudo chmod +x /usr/local/bin/gitlab-runner

#如果想使用Docker
curl -sSL https://get.docker.com/ | sh


#Create a GitLab CI user:
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash


#Install and run as service:
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start


#Register the Runner


#更新的话重新下载二进制包安装
```



<br/>
<br/>
<br/>



### Docker


```
#挂载运行
 docker run -d --name gitlab-runner --restart always \
   -v /srv/gitlab-runner/config:/etc/gitlab-runner \
   -v /var/run/docker.sock:/var/run/docker.sock \
   gitlab/gitlab-runner:latest


#Register the Runner


#更新的话，停止旧容器，拉取新镜像


#GitLab Runner Logs
#可以把Runner Logs目录挂载到宿主机，也可是使用docker 读取
```



<br/>
<br/>
<br/>


### k8s




<br/>
<br/>
<br/>



### Autoscale






<br/>
<br/>
<br/>



## 注册

Register GitLab Runner


安装GitLab Runner后，需要将其注册到GitLab。注册Runner是将Runner与GitLab实例绑定的过程。

<br>

**要求(Requirements)**，在注册Runner之前，你需要：

- 将其安装在与安装GitLab位置不同的Server上
- 通过GitLab的界面获取共享或特定Runner的Token

<br>

**注册环境：**

- GNU/Linux
- macOS
- Windows
- FreeBSD
- Docker
- ...


<br/>
<br/>



### GNU/Linux

在GNU / Linux下注册Runner：

```sh
#URL和Token在GitLab实例的runner里面去看


#运行命令
sudo gitlab-runner register


#输入GitLab 实例 URL
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.com


#输入获得的token
Please enter the gitlab-ci token for this runner
xxx


#输入Runner的描述，之后可在Web UI下更改
Please enter the gitlab-ci description for this runner
[hostame] my-runner


#输入与Runner相关联的tag，之后可在Web UI下更改
Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag


#输入Runner executor
Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker


#如果您选择Docker作为执行程序，则会要求您未在.gitlab-ci.yml中定义的用于项目的默认image
Please enter the Docker image (eg. ruby:2.1):
alpine:latest



#启动runner
sudo systemctl start gitlab-runner
```


<br/>
<br/>
<br/>


### 单行注册命令

One-line registration command

如果要使用非交互模式注册Runner，可以使用register子命令或使用其等效的环境变量。

```sh
#查看帮助
gitlab-runner register -h


#注册
sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.com/" \
  --registration-token "PROJECT_REGISTRATION_TOKEN" \
  --executor "docker" \
  --docker-image alpine:3 \
  --description "docker-runner" \
  --tag-list "docker,aws" \
  --run-untagged \
  --locked="false" \
```



<br/>
<br/>
<br/>



## 执行器

Executors

GitLab Runner实现了许多执行程序，可用于在不同的场景中运行构建。


执行器：

- Shell
- Docker
- Docker Machine and Docker Machine SSH (autoscaling)
- Parallels
- VirtualBox
- SSH
- Kubernetes


<br/>
<br/>


### 选择执行器

Selecting the executor


GitLab Runner实现了许多执行程序，可用于在不同的场景中运行构建。如果您不确定要选择什么，请阅读“我不确定”部分。访问兼容性图表，了解每个执行程序支持哪些功能，哪些功能不支持。

执行器支持不同平台和方法的项目构建：

| Executor | SSH | Shell | VirtualBox | Parallels | Docker | Kubernetes |
| - | - | - | - | - | - | - |
| Clean build environment for every build | ✗ | ✗ | ✓ | ✓ | ✓ | ✓ |
| Migrate runner machine | ✗ | ✗ | partial | partial | ✓ | ✓ |
| Zero-configuration support for concurrent builds | ✗ | ✗ (1) | ✓ | ✓ | ✓ | ✓ |
| Complicated build environments | ✗ | ✗ (2) | ✓ (3) | ✓ (3) | ✓ | ✓ |
| Debugging build problems | easy | easy | hard | hard | medium | medium |


<br>
<br/>


#### 不清楚该选择哪个执行器

I am not sure


- **Shell**
Shell是最简单的配置执行器。需要在安装Runner的同一台机器上手动安装构建的所有必需依赖项。

- **Virtual Machine**
此类执行器允许您使用已创建的虚拟机，该虚拟机已克隆并用于运行构建。我们提供两种完整的系统虚拟化选项：VirtualBox和Parallels。如果您希望在不同的操作系统上运行构建，它们可以证明是有用的，因为它允许在Windows，Linux，OSX或FreeBSD上创建虚拟机，然后GitLab Runner连接到虚拟机并在其上运行构建。它的使用对于降低基础设施成本也很有用。

- **Docker**
一个很好的选择是使用Docker，因为它允许一个干净的构建环境，并且易于依赖管理（构建项目的所有依赖项都可以放在Docker镜像中）。 Docker执行程序允许您轻松创建具有依赖服务的构建环境，如MySQL

- **Kubernetes**
Kubernetes执行程序允许您使用现有的Kubernetes集群进行构建。执行程序将调用Kubernetes集群API并为每个GitLab CI作业创建一个新的Pod（带有构建容器和服务容器）。

- **SSH**
添加SSH执行程序是为了完整性，但它是所有执行程序中支持最少的。它使GitLab Runner连接到外部服务器并在那里运行构建。(通常建议使用其它案例)



<br/>
<br/>



#### 兼容性

Compatibility

不同执行器支持的功能：

| Executor | SSH | Shell | VirtualBox | Parallels | Docker | Kubernetes |
| - | - | - | - | - | - | - |
| Secure Variables | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| GitLab Runner Exec command | ✗ | ✓ | ✗ | ✗ | ✓ | ✓ |
| gitlab-ci.yml: image | ✗ | ✗ | ✗ | ✗ | ✓ | ✓ |
| gitlab-ci.yml: services | ✗ | ✗ | ✗ | ✗ | ✓ | ✓ |
| gitlab-ci.yml: cache | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| gitlab-ci.yml: artifacts | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Absolute paths: caching, artifacts | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Passing artifacts between stages | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Use GitLab Container Registry private images | n/a | n/a | n/a | n/a | ✓ | ✓ |
| Interactive Web terminal | ✗ | ✓ (bash) | ✗ | ✗ | ✓ | ✓ |

<br>

不同shell支持的系统：

| Shells | Bash	| Windows Batch	| PowerShell
| - | - | - | - |
| Windows | ✓ | 	✓ (default)	| ✓ |
| Linux	| ✓ (default)	| ✗	| ✗ |
| OSX | ✓ (default)	| ✗	| ✗ |
| FreeBSD |	✓ (default)	| ✗	| ✗ |

<br>

不同shell支持的交互式Web终端：

| Shells | Bash | Windows Batch | PowerShell |
| - | - | - | - |
| Windows | ✗ | ✗ | ✗ |
| Linux | ✓ | ✗ | ✗ |
| OSX | ✓ | ✗ | ✗ |
| FreeBSD | ✓ | ✗ | ✗ |



<br/>
<br/>
<br/>



### Shell

Shell executor 是一个简单的执行程序，它允许您在运行Runner的机器上本地执行构建。它支持可以安装Runner的所有系统。这意味着它可使用Bash和PowerShell。

在Bash中，在`gitlab-runner command`命令之后加上`--user`，表示使用非特权用户运行。

<br>

源项目被切换到: `<working-directory>/builds/<short-token>/<concurrent-id>/<namespace>/<project-name>`
项目的缓存放于: `<working-directory>/cache/<namespace>/<project-name>`

这些都在GitLab-runner的配置: `/etc/gitlab-runner/atom.config.toml`

<br>

**以非特权用户运行(Running as unprivileged user)**

在Linux上(rpm/dpk)，安装程序将尝试使用`gitlab_ci_multi_runner`用户(如果找到)；如果找不到，它将创建一个`gitlab-runner`用户并改为使用它。
然后，所有shell build都将使用`gitlab-runner`或`gitlab_ci_multi_runner`用户执行。

<br>

在某些场景中，您的构建可能需要访问某些特权资源，例如Docker Engine或VirtualBox。在这种情况下，您需要将gitlab-runner用户添加到相应的组：

```sh
usermod -aG docker gitlab-runner
usermod -aG vboxusers gitlab-runner
```




<br/>
<br/>
<br/>



### Docker

The Docker executor


文档: <https://docs.gitlab.com/runner/executors/docker.html>


<br/>
<br/>
<br/>



### K8s

The Kubernetes executor


文档: <https://docs.gitlab.com/runner/executors/kubernetes.html>






<br/>
<br/>
<br/>
<br/>




## 高级配置

Advanced Configuration


<br/>


### 配置文件

Advanced configuration options Learn how to use the TOML configuration file that GitLab Runner uses.


GitLab Runner配置使用TOML格式，配置文件可能在如下位置:

- `/etc/gitlab-runner/config.toml`
- `~/.gitlab-runner/config.toml`
- `./config.toml`


<br/>


#### global部分

这定义了GitLab Runner的全局配置。

| 配置 | 描述 |
| - | - |
| `concurrent` | 限制全局可以同时运行多少个作业，0并不意味着无限制 |
| `log_level` | 日志级别(debug, info, warn, error, fatal, panic) |
| `log_format` | 日志格式(runner, text, json) |
| `check_interval` | 定义新作业检查之间的间隔长度(s)。默认值为3，如果设置为0或更低，将使用默认值 |
| `sentry_dsn` | 启用追踪所有系统级错误 |
| `listen_address` | `host:port`，Prometheus应该在其上进行监听 |

<br>

**`check_interval` 如何工作:**

如果`config.toml`配置文件中有多个`[[runner]]`(称之为worker)，那么GitLab请求之间的间隔比人们预期的要频繁。GitLab Runner包含一个循环，该循环不断地为worker针对其配置的GitLab实例调度请求。


<br/>
<br/>


#### `[session_server]`部分

`[session_server]`是系统运行程序级别的配置，因此应该在根级别指定，而不是每个执行器指定，即它应该在`[[runners]]`部分之外。session server允许用户与Runner负责的作业进行交互。
如果想要禁用`[session_server]`部分，删掉它即可。

| 配置 | 描述 |
| - | - |
| `listen_address` | 用于session server的内部URL |
| `advertise_address` | 向GitLab公开的用于访问Runner的URL |
| `session_timeout` | 作业完成后，会话可以在多长时间内保持活动状态(默认1800s) |


<br/>
<br/>


#### `[[runners]]`部分

如下定义了Runner entry:

| 配置 | 描述 |
| - | - |
| `name` | Runner的描述 |
| `url` | GitLab URL |
| `token` | Runner指定的token |
| `tls-ca-file` | HTTPS的CA证书 |
| `tls-cert-file` | HTTP的S端证书 |
| `tls-key-file` | HTTPS的S端Key |
| `limit` | 限制此token可同时处理的作业数，0为不限制 |
| `executor` | 执行器 |
| `shell` | 用于生成脚本的shell的名称 |
| `builds_dir` | 构建将存储在所选执行器的上下文中的目录(local, docker, ssh) |
| `cache_dir` | 构建缓存将存储在所选执行器的上下文中的目录(local, docker, ssh) |
| `environment` | 附加或覆盖环境变量 |
| `request_concurrency` | 限制GitLab新作业的并发请求数（默认值为1）|
| `output_limit` | 最大构建日志大小(默认4096KB) |
| `pre_clone_script` | 在克隆Git存储库之前要在Runner上执行的命令 |
| `pre_build_script` | 克隆Git存储库之后但在执行构建之前要在Runner上执行的命令 |
| `post_build_script` | 在执行构建之后但在执行`after_script`之前在Runner上执行的命令 |
| `clone_url` | 覆盖GitLab实例的URL |

<br>

**`clone_url`怎样工作:**

如果GitLab实例公开给Runner无法使用的URL，则可以配置`clone_url`。


<br/>
<br/>


####　EXECUTORS

- shell
- docker
- docker-ssh
- ssh
- parallels
- virtualbox
- docker+machine
- docker-ssh+machine
- kubernetes


<br/>
<br/>


#### SHELLS

- bash
- sh
- cmd
- powershell


<br/>
<br/>


####　`[runners.docker]`部分

| 参数 | 描述 |
| - | - |
| `host` | 指定Docker endpoint (默认 `$DOCKER_HOST`或`unix:///var/run/docker.sock`) |
| `hostname `| 为Docker容器指定主机名 |
| `runtime` | 为Docker容器指定一个运行环境 |
| `tls_cert_path` | 证书路径 |
| `image` | 使用此镜像进行构建 |
| `memory` | 容器内存限制 |
| `memory_swap` | 总内存限制 |
| `memory_reservation` | 容器内存soft limit |
| `oom_kill_disable` | 容器OOM后也不kill进程 |
| `cpuset_cpus` | 容器使用的CPU |
| `cpus` | CPU数量 |
| `dns` | 容器使用的DNS列表 |
| `dns_search` | DNS搜索域列表 |
| `privileged` | 特权容器 |
| `disable_entrypoint_overwrite` | 禁用镜像端点覆盖 |
| `userns_mode` | 启用usernamespace重映射选项时，为容器设置usernamespace模式 |
| `cap_add` | 向容器添加其他Linux功能 |
| `cap_drop` | 从容器中移除其他Linux功能 |
| `security_opt` | 设置安全选项(key: value) |
| `devices` | 与容器共享其他主机设备 |
| `cache_dir` | 指定缓存目录 |
| `disable_cache` | 禁用缓存 |
| `network_mode` | 将容器添加到一个自定义的网络 |
| `wait_for_services_timeout` | 等待docker的时间，0为禁用(默认30) |
| `volumes` | docker挂载卷 |
| `extra_hosts` | 指定应在容器环境中定义的主机 |
| `shm_size` | 指定镜像共享的内存大小(Byte)) |
| `volumes_from` | 指定从其它容器继承的卷(格式: `\<container name\>[:\<ro|rw\>]`) |
| `volume_driver` | 指定容器使用的卷的驱动 |
| `links` | 指定与其建立链接的容器 |
| `services` | 指定使用build运行的其它服务 |
| `allowed_images` | 指定可在`.gitlab-ci.ym`l中指定的通配符图像列表 |
| `allowed_services` | 指定可在`.gitlab-ci.yml`中指定的通配符服务列表 |
| `pull_policy` | 指定镜像拉取策略 |
| `sysctls` | 指定sysctl options |
| `helper_image` | 覆盖用于克隆repos和上载工件的默认帮助程序镜像 |


<br/>
<br/>


#### `[runners.parallels]`部分

| 参数 | 描述 |
| - | - |
| `base_name` | 将克隆的Parallels VM的名称 |
| `template_name` | Parallels VM链接模板的自定义名称 |
| `disable_snapshots` | 如果禁用，则在构建之后将摧毁VM |


<br/>
<br/>


#### `[runners.virtualbox]`部分

| 参数 | 描述 |
| - | - |
| `base_name` | 要克隆的VirtualBox VM的名称 |
| `base_snapshot` | 要从中创建链接克隆的VM的特定快照的名称或UUID |
| `disable_snapshots` | 如果禁用，则在构建之后将摧毁VM |


<br/>
<br/>


#### `[runners.ssh]`部分

| 参数 | 描述|
| - | - |
| `host` | 指定主机 |
| `port` | 指定端口 |
| `user` | 指定用户 |
| `password` | 指定密码 |
| `identity_file` | 指定私钥 |


<br/>
<br/>


#### `[runners.machine]`部分

| Parameter | Description |
| - | - |
| `IdleCount` | Number of machines, that need to be created and waiting in Idle state. |
| `IdleTime` | Time (in seconds) for machine to be in Idle state before it is removed. |
| `OffPeakPeriods` | Time periods when the scheduler is in the OffPeak mode. An array of cron-style patterns (described below). |
| `OffPeakTimezone` | Time zone for the times given in OffPeakPeriods. A timezone string like Europe/Berlin (defaults to the locale system setting of the host if omitted or empty). |
| `OffPeakIdleCount` | Like IdleCount, but for Off Peak time periods. |
| `OffPeakIdleTime` | Like IdleTime, but for Off Peak time mperiods. |
| `MaxBuilds` | Builds count after which machine will be removed. |
| `MachineName` | Name of the machine. It must contain %s, which will be replaced with a unique machine identifier. |
| `MachineDriver` | Docker Machine driver to use |
| `MachineOptions` | Docker Machine options |


<br/>
<br/>


#### `[runners.cache]`部分

| Parameter | Type | Description |
| - | - | - |
| `Type` | string | One of: `s3`, `gcs` |
| `Path` | string | Name of the path to prepend to the cache URL |
| `Shared` | boolean | Enables cache sharing between runners, `false` by default |



<br/>
<br/>


#### `[runners.kubernetes]`部分

| 参数 | 描述 |
| - | - |
| `host` | k8s master url |
| `cert_file` | k8s master认证证书 |
| `key_file` | k8s master 私钥 |
| `ca_file` | k8s master CA |
| `image` | 当未指定时，用于构建的默认docker镜像 |
| `namespace` | 命名空间 |
| `privileged` | 特权容器(true/false) |
| `node_selector` | 节点选择器 |
| `image_pull_secrets` | 镜像拉取秘钥 |



<br/>
<br/>
<br/>


### 自签名证书

Use self-signed certificates Configure certificates that are used to verify TLS peer when connecting to the GitLab server.


这允许在注册runner时解决由未知权限问题签名的证书(`x509`)。

<br>

**支持自签名的证书:**

1. 默认情况下： GitLab Runner读取系统存储的证书并根据存储在系统中的CA验证GitLab服务器
2. GitLab Runner从预定义文件中读取PEM（不支持DER格式）证书: 如`/etc/gitlab-runner/certs/`
3. GitLab Runner在注册期间和`[[runners]]`部分下的`config.toml`配置中公开`tls-ca-file`选项，允许您指定带证书的自定义文件。每当Runner尝试访问GitLab服务器时，都会读取此文件。

















































