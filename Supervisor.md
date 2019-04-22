---
title: Supervisor
date: 2018-04-08 10:38:33
tags: Supervisor
categories: Linux

---

参考：

- <http://www.supervisord.org>


环境：

- Supervisor 3.3.4
- CentOS7.x86_64


<br>
<br/>

<!--more-->

---

<br/>


# 介绍


<br>


## 综述


Supervisor是一个C/S系统，允许用户在Unix-Like操作系统上控制许多进程。它受如下启发：

- Convenience
- Accuracy
- Delegation
- Process Group


<br>
<br/>


## 特点


- Simple
- Centralized(统一)
- Efficient
- Extensible
- Compatible
- Proven(久经考验)


<br>
<br/>


## Supervisor组件


- supervisord
	+ Supervisor的服务器部分被命名为`supervisord`。负责启动子进程，响应客户端的子进程，重启奔溃或退出的子进程，记录其`stderr`和`stdout`，以及生成对应的事件
	+ 默认使用的配置文件为`/etc/supervisord.conf`——Windows-INI格式的文件，由于它包含了未加密的username和password，请保证它安全
- supervisorctl
	+ Supervisor的客户端部分被命名为`supervisorctl`。用户可连接到不同的supervisord，status/stop/start子进程，获取supervisord中正在运行的进程列表
	+ 通过Unix domain socket或TCP socket与server通信，客户端在执行命令前应该先提供认证。客户端和服务端使用同一个配置文件
- Web server
	+ Web界面，可通过它查看或控制进程状态
- XML-RPC接口
	+ 用于询问和控制管理程序及其运行的程序


<br>
<br/>


## 平台要求


- 在Unix-Like系统上运行良好
- 不支持Windows系统
- Supervisor运行在Python2.4或之后的版本，不支持Python3



<br>
<br/>

---

<br>



# 安装


安装方法取决于你的操作系统。

<br>


## 通过网络安装

- 推荐使用setuptools的`easy_install`
- 下载Supervisor包并调用一个命令


<br>


### 使用Setuptools的网络安装


如果Python解释器安装了Setuptools:

```
easy_install supervisor

```


<br>


## 不使用Setuptools的网络安装

如果系统上未安装Setuptools，那么你需要手动去下载Supervisor发行套件和安装它。

PYPI： <https://pypi.python.org/pypi/supervisor>

```
wget https://pypi.python.org/pypi/supervisor/xxx.tar.gz

tar -xzf xxx.tar.gz

python setup.py install
#它会自动通过网络下载依赖
```


<br>
<br/>


## 安装一个分发包


一些Linux发行版提供了可通过系统包管理工具安装Supervisor。这些包由第三方制作，包含了对特定发行版的一些修改。

```
yum info supervisor
yum search supervisor

yum install -y supervisor
```


<br>
<br/>


## 通过pip安装


```
pip install supervisor
```


<br>
<br/>


## 创建一个配置文件


由于我是通过`yum`安装，所以`supervisor`配置文件自动在`/etc`下自动生成：

- 默认配置文件： `/etc/supervisord.conf`
建议在此配置文件中加入`[include]`，默认已包含此配置

- 目录： `/etc/supervisord.d`
建议将每个配置单独写在此目录下


<br>
<br/>

---

<br>



# 运行Supervisor


<br>

## 添加一个程序


在`supervisord`为你做任何有用的事情之前，你至少需要在配置文件中添加一个程序部分。**program**部分将定义在调用supervisord命令时如何运行和管理一个程序。

一个最简单的栗子：

```
[program:foo]
command=/bin/cat
```

上面的栗子只命名了一个命令，还有很多其它关于程序部分的设置。


<br>
<br/>


## 运行supervisord


使用`supervisord`命令启动supervisord，进程将自我守护，并从终端分离。并将操作日志默认放于`$CWD/supervisor.log`。
你可传递`-n/--nodaemon`标志来将进程放置于前台，这样对于debug很有帮助。

要更改supervisord控制的程序集，请编辑配置文件并`kill- HUP`，或以其它方式重新启动supervisord进程。

`supervisord`命令接受许多命令行选项。这些命令行选项中的每一个都会覆盖配置文件中的任何等效值。

详细选项： <http://www.supervisord.org/running.html#supervisord-command-line-options>


<br>
<br/>


## 运行supervisorctl


使用`supervisorclt`命令启动supervisorctl客户端。如果需要验证supervisord调用，则系统会要求您提供验证凭据。

```
supervisorctl status all

supervisorctl stop all
```


<br>


### supervisorctl Actions


如果在命令行中指定了`-i`或未指定任何操作(action)，则将启动交互式输入的shell解释操作。

```
supervisorctl help
#查看可操作的action


default commands (type help <topic>):
=====================================
add    clear  fg        open  quit    remove  restart   start   stop  update
avail  exit   maintail  pid   reload  reread  shutdown  status  tail  version

```


<br>
<br/>


## Signals


supervisord程序可能会发送某些actions，让它在运行时执行某些操作。你可将这些信号发送到一个单一的supervisord的PID。


<br>


### 信号处理程序


- `SIGTERM`
	+ supervisord及其所有子进程都将关闭
- `SIGINT`
	+ supervisord及其所有子进程都将关闭
- `SIGQUIT`
	+ supervisord及其所有子进程都将关闭
- `SIGHUP`
	+ supervisord将关闭所有进程，重新载入配置文件并启动所有进程
- `SIGUSR2`
	+ supervisord将关闭并重新打开主要活动日志和所有子日志文件


<br>
<br/>


## 运行安全


开发人员尽力确保以root身份运行的supervisord进程不会导致意外的权限升级。但supervisord允许在其配置文件中的任意路径规范写入数据，允许任意路径选择可能会造成符号链接工具的漏洞。
确保supervisord配置文件的权限安全，除此之外，确保Python PATH和标准库都有足够的文件权限保护。


<br>
<br/>


## 开机自启


由于我是`yum`安装，所以能够直接使用系统服务管理来设置开机自启。



<br>
<br/>

---

<br>



# 配置文件

Supervisor的配置文件通常命名为`supervisord.conf`。如果没有指定`-c`配置文件，应用程序会从以下位置去寻找配置文件：

- $CWD/supervisord.conf
- $CWD/etc/supervisord.conf
- /etc/supervisord.conf
- /etc/supervisor/supervisord.conf (since Supervisor 3.3.0)
- ../etc/supervisord.conf (Relative to the executable)
- ../supervisord.conf (Relative to the executable)


<br>
<br/>


## 文件格式


`supervisord.conf` is a Windows-INI-style (Python ConfigParser) file.
它包含section（[header]）和section中的key/value对。

<br>


### 环境变量


使用Python字符串表达式语法`%(ENV_X)%`，可以在配置文件中使用环境中存在的环境变量

```
[program:example]
command=/usr/bin/example --loglevel=%(ENV_LOGLEVEL)s
```


<br>
<br/>


## [unix_http_server]


在此section中应该插入在Unix domain socket上监听的HTTP server的配置参数。
如果没有配置此section，则Unix domain socket HTTP server将不会启动。

```
[unix_http_server]


#supervisor监听HTTP/XML-RPC请求的Unix domain socket的路径
file


#socket文件的权限模式
chmod


#socket的用户和组
chown


#访问HTTP server需要的认证
username


#密码可以是明文，或使用SHA加密的字符串
password
```


<br>
<br/>


## [inet_http_server]


监听TCP(internet) socket 的HTTP server的配置参数。
如果此section未配置，inet HTTP server将不会启动。

```
#tcp host:port，supervisor监听HTTP/XML-RPC请求的地址
port


#HTTP server认证
username


#密码可以是明文，或SHA加密
passwd

```


<br>
<br/>


## [supervisord]


与supervisord进程有关的全局设置。

```
logfile

logfile_maxbytes

logfile_backps

#critical, error, warn, info, debug, trace
logevel

pidfile

umask

nodaemon

minfds

minprocs

#防止supervisord在启动时清除任何现有子日志文件
nocleanup

childlogdir

user

directory

strip_ansi

enviroment

identifier
```


<br>
<br/>


## [supervisorctl]


`supervisorctl`交互式shell程序。

```
serverurl

#与前面设置的验证账户一致
username
password

prompt

history_file

```


<br>
<br/>


## [program:x]


supervisord知道的应该启动和控制的程序。

```
 #该程序启动时将运行的命令
command

 #进程名称
process_name

 #多个实例
numproc

 #用于计算numprocs开始的数量
numprocs_start

 #程序在启动和关闭顺序中的相对优先级
priority

 #当supervisord启动时，改程序将自动启动
autostart

 #程序在启动后需要保持运行以考虑启动成功的总秒数，设置为0表示不需要再任何特定的事件内保持运行
startsecs

 #允许失败的尝试次数，然后放弃并将进程置入fatal状态
startretries

 #自动重启进程
autorestart

 #异常退出码
exitcodes

 #请求停止时用于杀死程序的信号
stopsignal

 #发送停止信号后，等待系统将信号返回给supervisord的秒数
stopwaitsecs

 #将停止信号发送给整个进程组
stopagroup

 #
killasgroup

 #以哪个用户运行该程序
user

redirect_stderr

stdout_logfile
stdout_logfile_maxbytes
stdout_logfile_backups
stdout_capture_maxbytes
stdout_events_enabled

stderr_logfile
stderr_logfile_maxbytes
stderr_logfile_backups
stderr_capture_maxbytes
stderr_events_enabled

environment

directory

umask

serverurl

```


<br>
<br/>


## [include]


如果配置文件包含`[include]`部分，则它必须包含一个名为`files`的key。该key中的值包含了其它配置文件。

```
#文件空间的空格分隔序列，路径可以是相对或绝对。
files
```


<br>
<br/>


## [group:x]


将同质进程组组合成一个异质进程组通常很有用，所以它们可以作为supervisor各种控制器接口的一个单元进行控制。

```
#程序的逗号分隔列表
programs

#优先级
priority

```


<br>
<br/>


## [fcgi-program:x]


```
#程序的fastCGI socket或TCP或Unix domain socket
socket


#为socket指定特定user或group
socket_owner


#指定permission模式
socket_mode
```


<br>
<br/>


## [eventlistener:x]


supervisor允许在配置文件中定义专门的同质进程组(event listener pools)。

```
buffer_size

events

result_handler

```


<br>
<br/>


## [rpcinterface:x]


`[rpcinterface:x]`适用于希望通过自定义行为扩展supervisor的人们。


































