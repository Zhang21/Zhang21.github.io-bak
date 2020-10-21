---
title: Nginx
date: 2017-09-01 16:22:58
tags: Nginx
categories: DevOps
---

参考：

- [Nginx官方文档](https://nginx.org/en/docs/)
- [Nginx-Wikipedia](https://zh.wikipedia.org/wiki/Nginx)
- [Nginx-repo](http://nginx.org/packages/)

环境：

- CentOS7x86_64;
- Nginx1.12.1


<br/>
<br/>

---

<br/>

# Nginx介绍


Nginx（发音同engine x）是一个 Web服务器，也可以用作反向代理，负载平衡器和 HTTP缓存。它能反向代理 `HTTP, HTTPS, SMTP, POP3, IMAP` 的协议连接。
基于BSD-like协议发行，支持多种操作系统。

<!--more-->

作为HTTP服务软件的后起之秀，Nginx有很多优点：

- 在性能上，Nginx占用的系统资源更少，支持更多的并发连接（特别是小静态文件场景下），达到更高的访问效率；
- 在功能上，Nginx不仅是一个优秀的*Web服务软件*，还可以作为*反向代理* *负载均衡*及*缓存*使用。它类似于*LVS负载均衡*及*HAProxy*等专业代理软件，又类似于*Squid*等专业缓存服务软件；
- 在安装配置上，Nginx方便、简单、灵活。

Nginx功能丰富，可作为HTTP服务器、反向代理服务器、邮件服务器。支持*FastCGI, SSL, Virtual Host, URL Rewrite, Gzip*等功能，并支持很多第三方模块扩展。


<br/>

## 与PHP的集成

自PHP-5.3.3起，PHP-FPM加入到了PHP核心，编译时加上--enable-fpm即可提供支持。
PHP-FPM以守护进程在后台运行，Nginx响应请求后，自行处理静态请求，PHP请求则经过fastcgi_pass交由PHP-FPM处理，处理完毕后返回。
Nginx和PHP-FPM的组合，是一种稳定、高效的PHP运行方式，效率要比传统的Apache和mod_php高出不少。

**Nginx的重要特性：**

> 可针对静态资源高速高并发访问及缓存；
> 可使用反向代理加速，并且可进行数据缓存；
> 具有简单负载均衡、节点健康检查和容错功能；
> 支持远程FastCGI、Uwsgi、SCGI、Memcached Servers的加速和缓存；
> 支持SSL、TLS、SNI；
> 具有模块化的架构：过滤器包括gzip压缩、ranges支持、chunked响应、XSLT、SSI及图像缩放等功能。在SSI过滤器中，一个包含多个SSI的页面，如果FastCGI或反向代理处理，可被并行处理；
> 它具备的其他WWW服务特性：
> 支持基于名字、端口及IP的多虚拟主机站点；
> 支持Keep-alived和pipelined连接；
> 可进行修改Nginx配置，并且在代码上线时，可平滑重启，不中断业务访问；
> 可自定义访问日志格式，临时缓冲些日志操作，快速日志轮询及通过rsyslog处理日志；
> 可利用信号控制Nginx进程；
> 支持 3xx-5xx HTTP状态码重定向；
> 支持rewrite模块，支持URI重写及正则表达式匹配；
> 支持基于客户端IP地址和HTTP基本认证的访问控制；
> 支持PUT、DELETE、MKCOL、COPY及MOVE等较特殊的HTTP请求方法；
> 支持FLV流和MP4流技术产品应用；
> 支持HTTP响应速率限制；
> 支持同一IP地址的并发连接或请求数连接；
> 支持邮件服务器代理；


<br>

## Nginx常用功能

### http代理于反向代理

Nginx在做反向代理时，提供性能稳定，并且能够提供配置灵活的转发功能。Nginx可以根据不同的正则匹配，采取不同的转发策略，比如图片文件结尾的走文件服务器，动态页面走web服务器，只要你正则写的没问题，又有相对应的服务器解决方案，你就可以随心所欲的玩。并且Nginx对返回结果进行错误页跳转，异常判断等。如果被分发的服务器存在异常，他可以将请求重新转发给另外一台服务器，然后自动去除异常服务器。

![Nginx代理与反向代理](/images/Nginx/Nginx001.jpg)


<br/>


### 负载均衡

Nginx提供的负载均衡策略有2种：内置策略和扩展策略。内置策略为轮询，加权轮询，Ip hash。扩展策略，就天马行空，只有你想不到的没有他做不到的啦，你可以参照所有的负载均衡算法，给他一一找出来做下实现。

![](/images/Nginx/Nginx002.jpg)


<br>


### web缓存

Nginx可以对不同的文件做不同的缓存处理，配置灵活，并且支持FastCGI_Cache，主要用于对FastCGI的动态程序进行缓存。配合着第三方的ngx_cache_purge，对制定的URL缓存内容可以的进行增删管理。


<br>


### web服务

Nginx作为Web服务器的主要应用场景包括：

- 使用Nginx运行HTML、JS、CSS、小图片等静态数据；
- 结合FastCGI运行PHP等动态程序（如fastcgi_pass）；
- 结合Tomcat/Resin等支持Java动态程序（如proxy_pass）。




<br/>

---

<br>


# Nginx安装

## RPM源安装:

```
yum install -y gcc gcc-c++ make libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel    安装依赖
rpm -ivm http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm 安装RPM源

#安装Nginx
yum install -y nginx

#查询安装
rpm -q nginx
```


<br>


## 添加Nginx yum repository安装

```
vim /etc/yum.repos.d/nginx.repo


#必须唯一
[nginx]

name=nginx-repo
baseurl=http://nginx.org/packages/$OS/$OSRELEASE/$basearch/
gpgcheck=0
enabled=1
```


<br>


## 源码安装

```
#建议解压于此目录
cd /usr/local/src

wget http://xxx.xx.com/nginx.tar.gz
tar -zxvf nginx.tar.gz

cd ./nginx
./configure --prefix=/usr/local

make&&make install
```




<br>

---

<br/>


# Nginx配置


## *.conf

Nginx配置文件主要分为四部分：

- main(全局设置)；
- server(主机设置)；
- upstream(上游服务器设置)，用于反向代理和负载均衡；
- location(URL匹配特定位置)。

栗子：
运行`nginx -t`检查配置文件有误错误，这很重要!

```sh
#main块
user nginx;
worker_processes 4;
client_max_body_size  10M


error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
	worker_connections 102400;
}

#http块
http {
	include /etc/nginx/mime.types;
    default_type application/octet-stream;
    log_format main '$remote_addr - ...';

    access_log /var/log/nginx/access.log main;
    sendfile on;
    gzip on;
    keepalive_timeout 60;

	include /etc/nginx/conf.d/*.conf;


	#upstream块
    upstream up_name{
    	server ip1;
        server ip2:port;
        server domain;
    }


	server {
    	server_name www.zhang21.cn;

    	listen 80;
        listen 443;
        ssl on;
        ssl_certificate /dir/path/xxx.crt;
        ssl_certificate_key /dir/path/xxx.key;

        location / {
        	root /var/www/zhang;
            index index.php index.html index.htm;

            allow 192.168.1.0/22;
            deny all;
        }

        location ~ \.php$ {
        	root /var/www/zhang;
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
            include fastcgi_params;
        }

    }

}
```

- 全局块：
配置影响Nginx全局的指令。一般由运行Nginx服务器的用户组，Nginx进程pid存放路径，日志存放路径，允许生成的worker_processes等。

- events块：
配置影响Nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网络连接，开启多个网络连接序列化等。

- http块：
可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，链接超时时间，单连接请求数等。

- server块：
配置虚拟主机的相关参数，一个http中可以有多个server。

- location块：
配置请求的路由，以及各种页面的处理情况。


<br/>

Nginx http功能模块 | 模块说明
- | -
ngx_http_core_module | 一些核心的http参数配置，对应Nginx的配置中的http块
ngx_http_access_module | 访问控制模块，用来控制网站用户对Nginx的访问
ngx_http_gzip_module | 压缩模块，对Nginx返回的数据压缩，属于性能优化模块
ngx_http_fastcgi_module | FastCGI模块，和动态应用相关的模块，如PHP
ngx_http_proxy_module | proxy代理模块
ngx_http_upstream_module | 负载均衡模块，可实现网站的负载均衡及节点的监控检查
ngx_http_rewrite_module | URL重写模块
ngx_http_limit_conn_module | 限制用户并发连接数及请求数模块
ngx_http_limit_req_module | 根据定义的key限制Nginx请求过程的速率
ngx_http_log_module | 访问日志模块，以指定的格式记录Nginx访问信息
ngx_http_auth_basic_module | web认证模块，设置通过账号，密码访问Nginx
ngx_http_ssl_module | ssl模块
ngx_http_stub_status_module | 记录Nginx基本访问状态信息扥的模块


<br/>

Nginx的日志时自动切割，并且一行可以记录多个日志格式。

Nginx日志格式 | 说明
- | -
$remote_addr | 客户端ip地址
$http_x_forward_for | 当前端有代理服务器时，设置web节点记录web节点记录客户端地址的配置
$remote_user | 客户端用户名称
$time_local | 访问时间和时区
$request | 请求的http协议和URL
$status | 请求状态，如200
$body_bytes_sent | 发送给客户端文件主体内容大小
$http_referer | 从哪个页面链接访问过来
$http_user_agent | 客户端浏览器信息




<br/>
<br>

## server

http服务上支持若干虚拟主机。每个虚拟主机对应一个server配置项，配置项里面包含该虚拟主机的相关配置。每个server里面可同时有多个`server_name`。

在提供mail代理服务时，也可建立若干server，每个server通过监听地址或端口来区分。

```sh
#监听端口，默认80
listen 80;
listern 443;
#listen 88
server_name www.zhang21.cn
```



<br>
<br/>

## location

location是http服务中，某些特定的URL对应的一系列配置项。

- root 定义此location的根目录位置，一般放置在server里
- index 定义路径下的默认访问的文件名

```sh
location / {
	root /dir/path;
    index index.html index.htm;

}
```


<br>


### location的正则写法

location的使用方法：

符号 | 含义 | 优先级 | 用法
- | - | - | -
= | 精确匹配 | 最高 | location =
~ | 区分大小写的正则匹配 | 次次之 | location ~
~* | 不区分大小写的正则匹配 | 次次之 | location ~*
^~ | 常规字符串匹配 | 次之 | location ^~
/ | 通用匹配 | 最低 | location /

**优先级：**

> = > 完整路径 > ^~ > ~, ~* > 部分路径 > /


<br>


### location使用建议

location的使用根据实际情况来定。

但个人觉得至少应该有三个匹配规则：

1. 直接匹配网站跟，通过域名访问网站首页比较频繁
2. 处理静态文件请求，这是Nginx作为http服务器的强项
3. 通用规则，用来转发动态请求到后端的应用服务器(符php-fpm)
4. 根据实际情况的自定义需求

```sh
server {
	listen 80;
    listen 443;
    server_name zhang21.cn www.zhang21.cn;
    root /dir/path/zhang;

    ssl on;
    ssl_certificate /etc/nginx/ssl/zhang.crt;
    ssl_certificate_key /etc/nginx/ssl/zhang.key;

	#rewtire ^(.*)$ https://zhang21.cn/$1 permanent;
	return 301 https://zhang21.cn/$requets_uri


	location = / {
    	rewrite .*? /index.html last;
    }


	location  ^~ /static/ {
    	root /dir/path/zhang/static;
    }
    location ~* \.(gif|jpg|png|css|js)$ {
    	root /dir/path/zhang/static;
    }


	location / {
    	if (!-f $request_filename) {
            rewrite ^([^\?]+)$ /index.php?q=$1 last;
    }


    location ~ \.php$ {
        root           /dir/path;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root/$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```




<br/>
<br>

## Rewrite

Nginx的主要功能是实现URL地址重写。Nginx的rewrite规则需要PCRE软件的支持，即通过Perl兼容正则表达式语法进行规则匹配。

Nginx rewrite语法：

```sh
server, location, if

rewrite regex replacement [flag]
```

- rewrite的功能就是，使用Nginx提供的全局变量或自定义变量，结合正则表达式(re)和标志位实现URL重写以及重定向
- rewrite只能放在server，location，if中，并且只能对域名后边的除去传递的参数外的字符串起作用
- 如果相对域名或参数字符串起作用，可以使用全局变量匹配，也可使用proxy_pass反向代理

<br>

表面看rewrite和location功能有点像，都能实现跳转。主要区别在于：

- rewrite实在同一域名内更改获取资源的路径
- location是对一类路径做访问控制或反向代理，可proxy_pass到其它机器

循环超多10次，返回500 Internal Server Error!


<br>

### flag

- last
	* 表示完成rewrite，继续向下匹配新的规则
- break
	* 停止执行当前虚拟主机的后续rewrite指令集
- redirect
	* 返回302临时重定向，地址栏会显示跳转后的地址
- permanent
	* 返回301永久重定向，地址栏会显示跳转后的地址

- last和break用来实现URL重写，浏览器地址栏的URL地址不变，但在服务器端访问的程序及路径发生了变化
- redirect和permanent用来实现URL跳转，浏览器地址栏会显示跳转后的URL地址

Nginx的rewrite功能应用非常广泛：

- 可调整用户浏览的URL，使其看起来更规范
- 将动态URL地址伪装成静态地址提供服务
- 让旧域名跳转到新域名上
- 根据特殊变量、目录、客户端的信息进行URL跳转


<br>

### if指令

if语法：

```
if (condition) {
	xxx;
}
```

if的条件可以是如下内容：

- 当表达式只是一个变量时，如果值为空或任何以0开头的字符串都会被当作**false**
- 直接比较变量和内容时，使用**=**或**!=**
- **~**正则表达式匹配，**~\***不区分大小写的正则匹配，**!~**不匹配
- **-f**和**!-f**，用来判断是否存在文件
- **-d**和**!-d**，用来判断是否存在目录
- **-e**和**!-e**，用来判断时都存在文件或目录
- **-x**和**!-x**，用来判断文件是否可执行

<br>

栗子：

```sh
if ($http_user_agent ~ MSIE) {
	rewrite ^(.*)$ /msie/$1 break;
}


if ($http_cookie ~* "id=([^;])(?:;|$)") {
	set $id $1;
}


if ($http_method = POST) {
	return 405;
}


if (!-f $request_filename) {
	break;
    proxy_pass http://zhang;
}


if ($invalid_referer) {
	return 403;
}
```



<br>

### Nginx全局变量

常用作`if`判断的全局变量：

变量 | 描述 | 备注
- | - | -
$args | 等于请求行中的参数 | 同$query_string
$body_bytes_sent | 响应是发送的body字节数 | xxx
$content_length | Request Header中的Content-Length字段 | 内容长度
$content_type | Request Header中的Content-Type字段 | 内容类型
$document_root | 当前根路径 | xxx
$host | 请求主机头字段，否则为服务器名称 | xxx
$hostname | 主机名 | xxx
$http_user_agent | 客户端agent信息 | xxx
$http_cookie | 客户端cookie信息 | xxx
$is_args | 如果有$args参数，这个变量等于"?"，否则等于空 | xxx
$limit_rate | 限制连接数度 | xxx
$remote_addr | 客户端IP地址 | xxx
$remote_port | 客户端端口 | xxx
$remote_user | 经过Auth Basic Module验证的用户名 | 要先开启Nginx认证
$request | 用户请求信息 | xxx
$request_method | 客户端请求方法 | 通常为POST或GET
$request_body | 记录POST过来的数据信息 | xxx
$request_filename | 当前请求的文件路径 | 由root或alias指令与URI请求生成
$request_completion | 如果请求结束，设置为OK。否则为空 | xxx
$scheme | HTTP方法 | 如http, https
$server_protocol | 请求使用的协议 | 通常为HTTP/1.0或HTTP/1.1
$server_addr | 服务器地址 | 在完成一次系统调用后可以确定这个值
$server_name | 服务器名称 | xxx
$server_port | 请求到达服务器的端口号 | xxx
$status | 请求的响应状态码 | 如200
$request_uri | 包含请求参数的原始URI，不包含主机名 | 如"/foo/bar.php?arg=abc"
$uri | 不带请求参数的当前URI，不包含主机名 | 如"/foo/bar.html"


栗子：

```sh
http://localhost:88/test1/test2/test.php

$hsot: localhost
$server_port: 88
$request_uri: http://localhost:88/test1/test2/test.php
$document_uri: /test1/test2/test.php
$document_root: /var/www/test
$request_filename: /var/www/test/test1/test2/test.php
```


<br>

### rewrite实例

```sh
http {
	log_format main xxxx;
	rewrite_log on;


    server {
    	root /var/www/zhang;
        location / {
        	error_log logs/rewrite.log notice;
            rewrite '^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)'/data?file=$3.$4;
            set $image_file $3;
            set $image_type $4;
        }

        location /data {
        	access_log logs/images.log main;
            root /data/images;
            type_file /$arg_file /images404.html;
        }

		location = /image404.html {
        	return 404 "Image Not Found\n";
        }
    }
}
```


<br>

## 访问控制

<br>

### 添加用户密码验证

- 参考: <https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/>

<br>

有时需要为我们的网站设置访问账号和密码权限。
注意，此密码需要使用加密软件生成，直接写入密码的明文无效。



具体为这两个参数：

- `auth_basic`
	+ 默认值： `auth_basic off;`
	+ 使用位置：http, server, location, limit_except
- `auth_basic_user_file`
	+ 使用位置： http, server, location, limit_except

栗子：

```sh
cd /etc/nginx/conf.d
vim test.conf


location / {
	auth_basic "nginx auth test";
    auth_basic_user_file /etc/nginx/.auth;
}
```

配置用户密码

```
#Verify that apache2-utils (Debian, Ubuntu) or httpd-tools (RHEL/CentOS/Oracle Linux) is installed.
yum install -y httpd-tools

#htpasswd file username
htpasswd .auth  user1
New password:
Re-type new password:
Adding password for user user1

#查看加密密码
cat .auth
user1:$apr1$NIOeayen$hTAvJ5ZTwUHE6Hm1MiF920
```

<br>

![](/images/Nginx/auth.png)


<br/>
<br/>


###  限制IP访问

- allow
- deny

```sh
server {
	...
    allow IP1
    allow IP2;
    deny all;

	location / {
    	allow IP 1;
        deny all;
    }
}
```

注意：

- deny一定要加一个IP，否则会直接跳转403，不在往下执行。如果403默认页是在同一域名下，会造成死循环访问
- 对于allow的IP短，从允许访问的IP段位从小到大排列，如127.0.0.0/24， 10.10.0.0/16
- 以`deny all`结尾，表示除了上面允许的，其它都禁止


<br>
<br/>

## 语法检查

在启动或重启Nginx服务前检查语法非常重要，可以防止因配置错误导致网站重启或重载配置对用户的影响。

每次更改Nginx配置文件后都需要重新加载，将配置信息加载到内存中。这样设计的目的是大幅度提升Nginx的访问性。

```sh
nginx -t

nginx -s reload
```




<br>

---

<br>


# Nginx优化

<br>

## 常用优化

<br/>

### 隐藏Nginx版本号

一般来说，软件漏洞都和版本有关。因此要尽量隐藏对访问用户显示各类敏感信息。

```sh
vim /etc/nginc/nginx.conf


#nginx版本号默认是开启的
#位置：http, server, location

http {
	server_tokens off|on;
}
```


<br>

### 更改Nginx服务默认用户

1. 修改配置文件

```sh
vim /etc/nginx/nginx.conf

user nginx;
```

2. 如果是编译安装，直接在编译的时候指定用户和组

```sh
./configure --user=nginx --group=nginx
```


<br>


### 优化Nginx进程对应的配置

```sh
vim /etc/nginx/nginx.conf

worker_process n;
#建议n为CPU核数
#高并发场合可考虑为核数*2



#查看CPU核数
cat /proc/cpuinfo | grep processor | wc -l
lscpu
top命令，按1显示所有CPU核数
```


<br>

### 优化绑定不同的Nginx进程到不同的CPU上

默认情况下，Nginx的多个进程有可能跑在某一个CPU或CPU的某一核上，导致Nginx进程使用硬件资源不均。
所以，要尽可能地分配不同的Nginx进程给不同的CPU处理，达到充分有效利用硬件的多CPU多核资源的目的。

4核CPU配置举例：

```sh
vim /etc/nginx/nginx.conf

worker_processes 4;
#CPU亲和力参数
worker_cpu_affinity 0001 0010 0100 1000;
```


<br>

### Nginx事件处理模型优化

Nginx的连接处理机制在不同的操作系统会采用不同的I/O模型，在Linux下，Nginx使用epoll的I/O多路复用模型，在FreeBSD中使用kqueue的I/O多路复用模型，在solaris中使用/dev/poll方式的I/O多路复用模型，在Windows中使用的是icop。

配置：

```sh
#对于linux内核，推荐使用epoll工作模式
#Linux下默认epoll

vim /etc/nginx/nginx.conf

events {
	use epoll;
}
```


<br>

### Nginx单个进程允许的客户端最大连接数

请根据服务器性能和程序的内存使用量来合理制定最大连接数。
这个连接数包括了所有连接，如**代理服务器连接、客户端的连接、实际的并发连接**。

Nginx总并发连接数=worker*worker_connections

```sh
vim /etc/nginx/nginx.conf

events {
	worker_connections 10240;

}
```

仅仅修改了nginx最大连接数可能还不行，由于Linux系统有`ulimit`限制，所以可能还要做额外操作。

如：`nginx: [warn] 10240 worker_connections exceed open file resource limit: 1024`。

配置：

```sh
ulimit -a

ulimit -n 10240
```

注意，使用`ulimit`命令修改的值并不是永久生效的。


<br>

### Nginx worker进程最大打开文件数

可能也要注意`ulimit`系统限制！

```sh
vim /etc/nginx/nginx.conf

events {
	worker_rlimit_nofile 65535;
}
```


<br/>

### 开启高效文件传输

#### sendfile

sendfile()是作用于两个文件描述符之间的数据拷贝，这个拷贝是在内核之中的，被称为零拷贝。sendfile（）比read和write函数要高效很多，因为write和read函数要把数据拷贝到应用层再进行操作。

```sh
#位置：http, server, location, if in location
sendfile on;
```

<br>

#### tcp_nopush

激活或禁用Linux上的TCP_CORK socket选项，仅当开启sendfile生效。允许把 http response和文件的开始部分放在一个文件里发布，其积极作用是减少网络报文段的数量。

```sh
位置： http, server, location
tcp_nopush on;
```


<br>

### Nginx连接参数，连接超时时间

keep-alive可以使客户端到服务器端已经建立的连接一致工作不退出，当服务器有持续请求时，keep-alive会使用已经建立的连接提供服务，从而避免服务器重新建立新连接请求处理。

连接超时的作用：

> 将无用的连接设置为尽快超时，可保护系统资源（CPU、内存、磁盘）
> 连接很多时，及时断掉那些已经建立好但又长时间不做事的连接，以减少其占用的服务器资源。因为服务器维护连接也是消耗资源的
> 黑客和恶意用户攻击网站，也会不断地和服务器建立多个连接，消耗连接数但啥也不干，大量消耗服务器的资源，此时就应该及时断掉这些恶意占用资源的连接
> LNMP环境中，如果用户请求了动态服务，则Nginx就会建立连接，请求FastCGI服务以及后端的MySQL服务，此时这个Nginx连接就要设置一个超时时间，在用户容忍的时间内返回数据，或者再多等一会后端服务返回数据，具体策略根据具体业务进行具体分析
> 后端的FastCGI服务及MySQL服务也有对连接的超时控制

```sh
位置： http, server, location
keepalive_timeout 60;
```

<br>

默认情况下当数据发送时，内核并不会马上发送，可能会等待更多的字节组成一个数据包，这样可以提高I/O性能。
但是，在每次只发送很少字节的业务场景中，不使用tcp_nodelay功能，等待时间会比较长。

```sh
位置： http, server, location
tcp_nodelay on;
```

<br>

读取客户端请求头数据的超时时间，如果超过这个时间，客户端还没有发送完整的header数据，服务器端将返回“Request time out（408）”错误。

```sh
位置： http, server
client_header_timeout 20;
```

<br>

读取客户端请求主体的超时时间，如果在这个超时时间内，客户端没有发送任何数据，Nginx将返回“Request time out（408）”错误。

```sh
位置： http, server, location
client_body_timeout 60;
```

<br>

指定响应客户端的超时时间，为握手后的一个超时。如果超过这个时间，客户端没有任何活动，Nginx将会关闭连接。

```sh
位置： http, server, location
send_timeout 60;
```


<br>

### 上传文件大小限制(动态应用)

设置为0，表示禁止检查客户端请求主体的大小。

```sh
位置： http, server, location
client_max_body_size 20m;
```


<br>

### gzip压缩

Nginx gzip压缩模块提供了压缩文件内容的功能，用户请求的内容在发送到用户客户端之前，Nginx服务器会根据一些具体的策略实施压缩，以节约网络出口带宽，同时加快数据传输效率，提升用户体验。

压缩对象：

- 纯文本内容压缩比很高，如 html, js, css, xml等
- 被压缩的纯文本文件必须要大于1KB，由于压缩算法的特殊原因，极小的文件压缩后可能反而变大
- 图片、媒体等文件尽量不要压缩，因为这些文件大都经过压缩，再压缩很可能不会减小很多，或有可能增大，同时还要消耗系统资源

配置：

```sh
#压缩功能
gzip on;


#允许压缩的页面最小字节数
gzip_min_length  1K;


#申请4个单位为16K的内存作为压缩结果流缓存
gzip_buffers  4 16K;


#http协议版本
gzip_http_version  1.1;


#指定压缩比，1压缩比最小，处理速度最快；9压缩比最大，传输速度最快，处理最慢
gzip_comp_level  5;


#指定压缩类型，对应文件类型参考mime.types
gzip_types  text/html text/css;


#vary header支持
gzip_vary on;
```

在response header中查看效果： `Content-Encofing: gzip`


<br>

### expires缓存

Nginx expires的功能就是为用户访问的网站内容设定一个过期时间。

当用户第一次访问这些内容时，会把这些内容储存在用户浏览器本地，这样用户第二次及以后继续访问该网站时，浏览器会检查加载已经缓存在用户浏览器本地的内容，而不用去服务器下载，直到缓存的内容过期或被清除为止。

缓存也要根据业务！当网站数据更新时，用户端看到的可能还是旧的已经缓存的内容。

配置：

1. 根据文件扩展名进行判断

```sh
location ~ .*\.(gif|png|jpg|swf)$ {
	expires 10d;
}


location ~ .*\.(css|js)$ {
	expires 20d;
}
```

2. 根据目录进行判断

```sh
location ~ ^/(images|static|media)/ {
	expires 50d;
}
```

在response header中查看：

> Expires: 缓存过期时间
> Cache-Control： 缓存总时间



<br>
<br/>

## FastCGI相关参数

FastCGI参数是配合Nginx向后请求PHP动态引擎服务的相关参数，这里指的是Nginx中的配置参数。

Module ngx_http_fastcgi_module： <https://nginx.org/en/docs/http/ngx_http_fastcgi_module.html>

```sh
#给FastCGI服务器设置地址
fastcgi_pass


#设置一个将在 $fastcgi_scripts_name 变量结尾的URI之后添加的文件名
fastcgi_index


#设置一个应该传递给FastCGI服务器的参数，当且仅当fastcgi_param在当前级别上没有定义指令时，这些指令将从上一级继承
fastcgi_param


#指定在哪种情况下将请求传递给下一个服务器；
fastcgi_next_upsteam


#表示Nginx服务器和后端FastCGI服务器连接的超时时间，默认值为60秒，这个参数通常不要超过75秒
fastcgi_connect_timeout


#设置Nginx允许FastCGI服务器端返回数据的超时时间，即在规定时间之内后端服务器必须传完所有的数据。否则，Nginx将断开这个连接
fastcgi_send_timeout


#设置Nginx从FastCGI服务器端读取响应信息的超时时间，表示连接建立成功后，Nginx等待后端服务器的响应时间，是Nginx已经进入后端的排队之中等候处理的时间
fastcgi_read_timeout


#这是Nginx FastCGI的缓冲区大小参数，设定用来读取从FastCGI服务器端收到的第一部分响应信息的缓冲区大小，这里的第一部分通常会包含一个小的响应头部
fastcgi_buffer_size


#设定用来读取从FastCGI服务器端收到的响应信息的缓冲区大小和缓冲区数量
fastcgi_buffers


#用于设置系统很忙时可以使用的proxy_buffers大小，官方推荐大小为proxy_buffers * 2
proxy_busy_buffers_size


 #用于设置系统很忙时可以使用的fastcgi_buffers大小，官方推荐为 fastcgi_buffers * 2
fastcgi_busy_buffers_size


#FastCGI临时文件大小
fastcgi_temp_file_write_size


#表示开启FastCGI缓存并为其指定一个名称
fastcgi_cache cachename_nginx


#fastcgi_cache缓存目录
fastcgi_cache_path


#用来指定应答代码的缓存时间
fastcgi_cache_valid


#设置请求几次之后响应将被缓存
fastcgi_cache_min_uses


#定义在哪些情况下使用过期缓存
fastcgi_cache_use_stale


#定义fastcgi_cache的key
fastcgi_cache_key
```


<br>
<br>

## 日志与安全

现在Nginx 日志已经自动轮询了，所以感觉没有必要自己切割日志！


<br>

### 不记录不需要的日志

日志写入太频繁会消耗大量的磁盘I/O，降低服务性能。

```sh
location ~ .*\.(js|png|css|gif|jpg) {
	access_log off;
}
```


<br>

### 日志权限

因为nginx master process的UID是root，所以可以修改日志权限。
不需要在日志目录上给Nginx用户读或写许可，很多人没注意这个问题，把权限直接给了Nginx用户，这就存在安全隐患。

```sh
chown -R root:root  /path/log/nginx
chmod -R 700 /path/log/nginx
```


<br>
<br>

## 站点目录及URL访问控制


### 根据扩展名限制程序或文件访问

利用Nginx配置禁止访问上传资源目录下的PHP、Shell、Perl、Python程序文件，这样用户即使上传了木马文件也没法执行，从而加强了网站的安全。

对这些的限制必须放在Nginx处理`.php, .py, .sh`等文件的前面！

```sh
#禁止解析指定目录下的程序
location ~ ^/images/.*\.(php|py|sh|pl)$ {
	deny all;
}


location ~ ^/static/.*\.(py|php|pl|sh) {
	deny all;
}



#禁止访问某些文件
location ~* \.(txt|doc)$ {
	root /var/www/file;
    deny all;
}
```


<br>

### 禁止访问指定目录

```sh
location ~ ^/test/ {
	deny all;
}


#禁止访问多个目录
location ~ ^/(test|zhang) {
	deny all;
}


#返回状态码
location ~ ^/haha/ {
	return 403 "Hahaha";
}
```



<br>
<br/>


## 禁止非法域名解析访问网站

防止用户恶意域名解析。

```sh
cd /etc/nginx/conf.d
vim default.conf



#返回HTTP状态码
server {
	listen 80 default_server;
    server_name _;
    return 403;
}



#重定向
server {
	listen 80 default_server;
    server_name _;
    rewrite ^(.*) https://www.baidu.com permanent;
}
```

<br>

利用`default_server`，将网站所有请求定向到维护页面。

```sh
server {
	listen 80 default_server;
    server_name _;
    root /var/www;

	location / {
    	rewrite ^(.*) /maintance.html break;
    }
}


cd /var/www
vim maintance.html


<html>

<head>
<meta charset="UTF-8">
<style type="text/css">
    h1{text-align: center;
       color: red;}
</style>
</head>

<br>
<br>

<body>

<h1>网站维护中！</h1>

</body>

</html>
```




<br>
<br>


## 图片及目录防盗链


<br>


### 什么是资源盗链

简单地说，就是某些不法网站未经许可，通过在其自身网站程序里非法调用其他网站的资源，然后在自己的网站上显示这些调用的资源，达到填充自身网站的效果。

这一举动不仅浪费了调用资源网站的网络流量，还造成其他网站的带宽及服务压力吃紧。

问题：

> 某公司CDN源站流量没有变化，但CDN加速那边流量超了很多。这么大异常流量，全都是钱呀！


<br>

### 常见防盗链解决方案

<br>

#### 根据HTTP referer 实现防盗链

在HTTP 协议中，有一个表头字段叫 referer，使用URL格式来表示是哪里的链接用了当前网页的资源。

通过referer可以检测访问的来源网页，如果是资源文件，可以跟踪到显示它的网页地址，一旦检测出来不是本站，马上进行阻止。

HTTP referer 是header的一部分，当浏览器向web服务器发送请求时，一般会带上referer，告诉服务器我是从哪个页面过来的，服务器借此获得一些信息用于处理。

<br>

#### 根据cookie防盗链

通过加密技术变换访问路径实现防盗链


<br>

### Nginx实现防盗链

利用`referer`，针对指定扩展名进行`rewrite`或其他操作。

请根据实际情况进行域名防盗链！


```sh
location ~* \.(jpg|png|gif|wav|mp3|zip|rar)$ {
	valid_referers none blocked *.zhang.com;
    if ($invalid_regerer) {
    	rewrite https://www.baidu.com;
    }
}
```

或者在产品设计上解决防盗链，如为资源加上水印等措施。



<br>
<br/>

## 错误页面优雅展示

我们可以将404、403等错误信息重定向到其他指定的页面，提升网站的用户访问体验！


```sh
location / {
	xxxx;


	error_page 403 /403.html;
    error_page 404 /404.jpg;
    error_page 500 503 504 /50x.html;

	location = /50x.html {
    	root /var/www/50x.html;
    }
}
```



<br>
<br>


## 目录及文件权限优化

为了保证网站安全，所有站点的目录和用户组都为root，所有目录权限是755，所有文件权限是644。
虽然这样的全线可以防止黑客上传修改站点的文件，但这样合法的用户便也没有了上传权限。

比较好的方法是将用户上传文件的服务器与读取服务器进行分离，这样就可以进行安全授权。不同的服务所在的目录的权限依据业务功能而不同。

严格控制Nginx目录的访问才能降低网站被入侵的风险！



<br>
<br/>


## 反爬虫优化


<br>

### robots.txt机器人协议

robots协议([维基百科](https://zh.wikipedia.org/wiki/Robots.txt))，也称为机器人协议，全称是**网络爬虫排除标准（Robots Exclusion Protocol）**。网站通过Robots协议告诉搜索引擎那些页面可以抓取，那些页面不能抓取。

robots.txt协议并不是一个规范，而只是约定俗成的，所以并不能保证网站的隐私。

```sh
User-Agent: *
Allow: /zhang
Disallow: /
```


<br>

### Nginx反爬虫配置

```sh
if ($http_user_agent ~* LWP::Simple|BBBike|wget) {
	return 403;
}



if （$http_user_agent ~* (Firefox|MSIE) {
rewrite ^（.*） http://www.baidu.com:
}
```



<br>
<br>

## 限制HTTP请求方法

```sh
if ( $request_method !~ ^(GET|POST|HEAD)$ ) {
	return 501;
}
```



<br>
<br/>

## CDN

CDN的全称是 Content Delivery Network，中文意思是内容分发网络。
我们可以利用CDN做网站内容加速。

简单地讲，通过现有的Internet中增加一层新的网络架构，将网站的内容发布到最接近用户的Cache服务器内，通过智能DNS负载均衡技术，判断用户的来源，让用户就近使用与服务器相同线路的带宽访问Cache服务器，取得所需的内容。

> 例如，北京电信用户访问北京电信Cache服务器上的内容，四川网通用户访问成都网通Cache服务器上的内容。这样可以有效减少数据在网络上传输的时间，提高访问速度。
> CDN是一套全国或全球的风不是缓存集群，其实质是通过职能DNS判断用户的来源地域及上网线路，为用户选择一个最接近用户地域，以及和用户上网线路相同的服务器节点。因为低于近，线路相同，所以可以大幅度提升用户浏览网站的体验。

<br>

CDN的价值：

- 提升用户体验
- 阻挡大部分流量攻击

<br>

CDN的特点：

- 通过服务器内存缓存网站数据，提高了企业站点（尤其是含有大量图片、视频等的站点）的访问速度，并大大提高企业站点的稳定性；
- 用户根据智能DNS技术自动选择最适合的Cache服务器，降低不同运营商之间互联瓶颈造成的影响，实现了跨运营商的网络加速，保证不同网络中的用户都能得到良好的访问速度；
- 加快了访问速度，减少了原站点的带宽；
- 用户访问时从服务器的内存中读取数据，分担了网络流量，同时减轻了原站点负载压力；
- 使用CDN可以分担源站的网络流量，同时减轻源站的负载压力，并降低黑客入侵及各种DDOS攻击对网站的影响，保证网站有较好的服务质量；

<br>

### 使用CDN的要求

首先要说的是，不是所有的网站都可以一上来就能用CDN的。要加速的业务数据应该存在独立的域名。如 pub.zhang21.com，业务内容图片、附件、JS、CSS等静态元素，这样的静态网站域名才能使用CDN。

**将域名做CNAME(别名)**
将如上的`pub.zhang21.com`配置成CDN的域名。



<br>
<br/>

## 程序架构优化

**解耦** 是开发人员中流行的一个名词，简单地说就是把一堆程序代码按照业务用途分开，然后提供服务。

> 例如，注册登录、上传、下载、浏览、商品页信息等都应该是独立的程序服务，只不过在客户端看来是一个整体而已。

<br>

分离的最佳方式是分别使用独立的服务器，可以选择改动程序或者在负载均衡器上配置（如Nginx），过滤请求，然后抛给后面对应的服务器。

- 根据扩展名分发，请求图片就抛给图片服务器；
- 根据URL路径转发，请求下载就交给下载服务器；
- 请求动态PHP处理的就交给动态处理器；
- 不符合以上要求的就交给默认服务器；



<br>
<br/>

## 使用no-root用户启动Nginx

默认情况下，Nginx的Master进程使用的是root用户，worker进程使用的是Nginx指定的普通用户。

使用root用户跑Nginx的Master进程有两个最大问题：

- 管理权限必须是root，这就使得最小化分配权限原则遇到困难
- 使用root跑Nginx服务，一旦网站出现漏洞，用户就可以很容易地获取服务器的root权限



<br>
<br/>

## 控制Nginx并发连接数

`ngx_http_limit_conn_module`这个模块用于限制每个定义的Key值的连接数，特别是单IP的连接数。

不是所有的连接数都会被计数，一个符合要求的连接是整个请求头已经被读取的连接。

用法：

```sh
#位置： http
limit_conn_zone key zone=name:size;



#位置： http, server, location
limit_conn zone number;
```

栗子：

```sh
http {
	limit_conn_zone $binary_remote_addr zone=addr:10m;
    xxx;
}

server {
	xxx;
    location /download/ {
    	limit_conn addr 3;
        #限制单IP并发连接为3
    }
}
```



<br>
<br/>


## 控制客户端请求Nginx的速率

`ngx_http_limit_req_module`被用来限制每个IP访问没法key的请求速率。

用法：

```sh
#位置： http
limit_req_zone key zone=name:size rate=rate;



#位置： http, server, location
limit_req zone=name;
```

栗子：

```sh
http {
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;

  server {
	location /search/ {
	limit_req zone=one burst=5;
	}
  }
}
```




<br>

---

<br>



# upstream模块



<br>


## 反向代理与负载均衡


严格地说，Nginx仅仅是作为Nginx Proxy反向代理使用的，因为这个反向代理功能表现的效果是负载均衡集群的效果，所以本文称之为Nginx负载均衡。

- 普通的负载均衡软件，如LVS，其实现的功能只是对请求数据包的转发、传递，从负载均衡下的节点服务器来看，接收到的请求还是来自访问负载均衡器的客户端的真实用户
- 反向代理服务器在接收访问用户请求后，会代理用户 重新发起请求代理下的节点服务器，最后把数据返回给客户端用户。在节点服务器看来，访问的节点服务器的客户端用户就是反向代理服务器，而非真实的网站访问用户
- 即，LVS等负载均衡是转发用户请求的数据包，而Nginx反向代理是接收用户请求后重新发起请求后端节点

> 这里我去看了一下Nginx的access.log，客户端的访问日志全在代理节点上（Nginx-upstream），而后端节点的access.log的来源是前端代理节点的IP


<br>
<br>


## Nginx负载均衡的组件


实现Nginx负载均衡的组件主要有两个:

- proyx
- upstream

Nginx_http模块 | 模块说明
- | -
ngx_http_proxy_module | proxy代理模块，用于把请求后抛给服务器节点或upstream服务器池
ngx_http_upstream_module | 负载均衡模块，可以实现网站的负载均衡功能即节点的健康检查


<br>

### nginx upstream模块

<br>

#### upstream模块介绍

Module ngx_http_upstream_module: <https://nginx.org/en/docs/http/ngx_http_upstream_module.html>

upstream主要是用于七层上的负载均衡和转发。

```sh
Syntax：    upstream name { ... }
Default:    —
Context:    http
```

Nginx的负载均衡功能依赖于ngx_http_upstream_module模。所支持的代理方式包括：

- proxy_pass
- fastcgi_pass
- memcached_pass
- uwsgi_pass
- scgi_pass

<br>

upstream模块允许Nginx定义一组或多组节点服务器组，使用时可通过`proxy_pass`代理方式把网站的请求发送到事先定义好的对应`upstream组`的名字上。

<br/>

**upstream模块内容放置于http{}内**:

```sh
upstream upstream_name {
    server  address  [ parameters ]
}



####栗子
http {
	upstream zhang {
		server 192.168.1.22:8080 weight=5;
    	server www.zhang.cn weigh=5 max_conns=102400;
        server 192.168.33 max_fails=2 fail_timeout=20s;
    	server backup.zhang.cn backup;
    }
}


server {
	location / {
    	proxy_pass http://zhang;
    }
}



####reslove
http {
    resolver 10.0.0.1;
    upstream u {
        zone ...;
        ...
        server example.com resolve;
    }
}
```

- address可以是主机名、域名、ip或Unix Socket，也可以指定端口号
	+ 域名时需要解析的哦
- parameters代表可选参数, 有如下：
	+ `backup`，表示当前server是备用服务器，只有其它非backup后端服务器都挂掉了或很忙才会分配请求给它
	+ `max_conns`，限制同时连接到代理服务器的最大数量。默认值为0，表示没有限制。
	+ `weight`，表示当前server负载权重，权重越大几率愈高
	+ `max_fails`和`fail_timeout`一般会关联使用，如果某台server在fail_timeout时间内出现了max_fails次连接失败，那么Nginx会认为其已经挂掉，从而在 fail_timeout 时间内不再去请求它，fail_timeout默认是 10s，max_fails默认是1，即默认情况只要是发生错误就认为服务器挂了，如果将max_fails设置为0，则表示取消这项检查
	+ `down`，标志服务器永远不可用，可配合ip_hash使用
	+ `resolve`，监视与服务器域名相对应的ip地址的变化，并自动地修改上游配置，而不用重启Nginx
	+ `route`，设置服务器路由名称
	+ `service`
	+ `slow_start`，设置服务器将其weight从零恢复到正常值的时间
	+ `drain`，使服务器进入drain模式，在此模式下，只有绑定到服务器的请求才会被代理

upstream模块参数 | 说明
- | -
weight | 服务器权重
max_fails | Nginx尝试连接后端主机失败的次数，这个值是配合`proxy_next_upstream`、`fastcgi_next_upstream`和`memcached_next_upstream`这三个参数来使用的。当Nginx接收后端服务器返回这三个参数定义的状态码时，会将这个请求转发给正常工作的的后端服务器。如404、503、503、max_files=1
fail_timeout | `max_fails`和`fail_timeout`一般会关联使用，如果某台server在fail_timeout时间内出现了max_fails次连接失败，那么Nginx会认为其已经挂掉，从而在fail_timeout时间内不再去请求它，fail_timeout默认是10s，max_fails默认是1，即默认情况只要是发生错误就认为服务器挂了，如果将max_fails设置为0，则表示取消这项检查
backup | 表示当前server是备用服务器，只有其它非backup后端服务器都挂掉了或很忙才会分配请求给它
down | 标志服务器永远不可用，可配合ip_hash使用

<br/>

如果是两台Web服务器做高可用，可能就需要Keepalived配合。那使用`backup`参数通过负载均衡功能就可以实现Web服务器集群了。


<br/>

#### upstream模块调度算法

调度算法一般分为：

- 静态调度算法: 即负载均衡器根据自身设置的规则进行分配，不需要考虑后端节点服务器的情况
	+ 轮询
	+ 权重
	+ ip_hash
- 动态调度算法:  即负载均衡器会根据后端节点的当前状态来决定是否分发请求，如连接数少或响应时间短的优先获得请求
	+ fair
	+ least_conn
	+ url_hash
	+ 一致性hash

<br>

**轮询(rr)**
默认调度算法。
按照客户端请求顺序把请求逐一分配到不同的后端节点服务器，相当于LVS中的rr算法。如果后端服务器宕机，宕机的服务器会被自动从节点服务器池中剔除，以使客户端的用户访问不受影响，新的请求分配给正常的服务器。

<br>

**权重轮询(wrr)**
权重越大，被转发的请求也就越多。可以根据服务器的配置和性能指定权重大小，有效解决新旧服务器性能不均带来的请求分配问题。

```sh
upstream weight {
  server 191.168.1.11 weight=1;
  server 192.168.1.22 weight=2;
}
```

<br>

**ip_hash**
每个请求按客户端IP的hash结果分配，当新的请求到达时，先将其客户端ip通过哈希算法得出一个值，在随后的客户端请求中，客户IP的哈希值只要相同，就会被分配到同一台服务器。

该调度算法可以解决动态网页的session共享问题，但有时会导致请求分配不均，因为国内大多数都是NAT上网模式，多个客户端对应一个外部IP，所以这些客户端都会被分配到同一个节点服务器，从而导致请求分配不均。

ip_hash中，后端服务器在负载均衡调度中的状态不能有 weight和backup，有也不会生效

```sh
upstream iphash {
  ip_hash;
  server 192.168.1.11;
  server 192.168.1.22:8080;
}
```

<br>

**fair**
根据后端节点服务器的响应时间来分配请求，响应时间短的优先分配。这是更加智能的调度算法。

Nginx本身不支持这种算法，需要upstream_fair模块: <https://github.com/gnosek/nginx-upstream-fair>


```sh
upstream fair {
  server 192.168.1.11;
  server 192.168.1.22;
  fair;
}
```

<br>

**least_conn**
根据后端节点的连接数来决定分配情况，哪个机器少就分发给它。

<br>

**url_hash**
根据访问URL的hash结果来分配请求的，让每个URL定向到同一个后端服务器，后端服务器为缓存服务器时效果显著。

Nginx本身不支持url_hash，需要hash。

```sh
upstream urlhash {
  server hahaha1:5678;
  server hahaha2:5678;
  hash $request_uri;
  hash_method md5;
  #同样不能使用 weight、backup
}
```

<br>

**一致性hash**
一致性hash算法一般用于代理后端业务为缓存服务器（如Memcached）的场景，通过将用户请求的URI或者指定字符串进行计算，然后调度到后端的服务器上，此后任何用户查找同一个URI货值指定字符串都会被调度到这一台服务器上，因此后端的每个节点缓存的内容都是不同的。

```sh
upstream {
  consistent_hash $request_uri;
  server xxx;
  server xxx;
}
```


<br>

### nginx proxy模块

<br/>

#### proxy_pass介绍

```sh
Syntax:		proxy_pass URL;
Default:	—
Context:	location, if in location, limit_except
```

<br/>

proxy_pass指令属于ngx_http_proxy_module模块，此模块可以将请求转发到另一台服务器，在实际的反向代理工作中，会通过location功能匹配指定的URI，然后把接收到服务匹配URI的请求通过proyx_pass抛给定义好的upstream节点池。

```sh
location /download/ {
  proxy_pass http://download/vedio/;
}
#这是前端代理节点的设置
#交给后端upstream为download的节点

location /name/ {
  rewrite /name/([^/]+) /users?name=$1 break;
  proyx_pass http://127.0.0.1;
}
```

<br>

### http_proyx模块参数

ngx_http_proxy_module: <https://nginx.org/en/docs/http/ngx_http_proxy_module.html>

Nginx的代理功能是通过http_proxy模块来实现的。

proxy模块 | 说明
- | -
proxy_next_upstream	| 什么情况下将请求传递到下一个upstream
proxy_limite_rate	| 限制从后端服务器读取响应的速率
proyx_set_header	| 设置http请求header传给后端服务器节点，如：可实现让代理后端的服务器节点获取访问客户端的这是ip
client_body_buffer_size |	客户端请求主体缓冲区大小
proxy_connect_timeout | 代理与后端节点服务器连接的超时时间
proxy_send_timeout	| 后端节点数据回传的超时时间
proxy_read_timeout	| 设置Nginx从代理的后端服务器获取信息的时间，表示连接成功建立后，Nginx等待后端服务器的响应时间
proxy_buffer_size	| 设置缓冲区大小
proxy_buffers	| 设置缓冲区的数量和大小
proyx_busy_buffers_size	| 用于设置系统很忙时可以使用的proxy_buffers大小，推荐为proxy_buffers*2
proxy_temp_file_write_size	| 指定proxy缓存临时文件的大小



<br>
<br>


## Nginx负载均衡配置


<br>

### 配置后端节点

```sh
vi /etc/nginx/nginx.conf

server {
  listen 80;
  root /path/xxx;

  location / {
    xxxx;
  }
}
```


<br>


### 配置反向代理节点

```sh
upstream test {
  server test1 weight=5;
  server test2 weight=5;
  server 192.168.1.33;
}
```

```sh
vi /etc/nginx/nginx.conf


server {
  listen 8888;
  server_name www.test.com www.xx.com;

  location  / {
    proxy_read_timeout 10s;
    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_404;
    proyx_pass http://test;#把用户的请求反向代理定义的upstream服务器池

    #proyx_set_header Host $host;在代理后端服务器发送的http请求头中加入host字段信息
    #proxy_set_header X-Real-IP $remote_addr;后端节点服务器日志获取客户端真实ip，否则全都是代理节点的ip
    #proyx_connect_timeout 30s;
    #proxy_buffers 4m;
    #xxx
}
  xxxxx
}
```


<br>

### 与反向代理配置相关的参数

除了具有多虚拟主机代理以及节点服务器记录真实用户ip的功能外，Nginx还提供了相当多的作为反向代理和后端节点服务器对话的相关控制参数。

由于参数众多，建议把这些参数都写到另外一个配置文件里，然后用 include 方式包含到虚拟主机配置文件里。其他Nginx参数也同样可以使用此方法。

```sh
vim /etc/nginx/proxy.conf


proxy_set_header Host $host;
proxy_set_header $remote_addr;
proxy_connect_timeout 60s;
proxy_read_timeout 20s;
proxy_send_timeout 20s;
proxy_buffer_size 64k;
proxy_buffers 4 64k;
proxy_busy_buffers_size 128k;
proxy_temp_file_write_size 2m;
proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_404
```

```sh
vim /etc/nginx/conf.d/test.conf


server {
  listen 80;
  server_name www.test.com www.xxx.com;
  location / {
    include /etc/nginx/proxy.conf;
  }
}
```

<br>

**proxy_next_upstream参数补充**
当Nginx接收后端服务器发返回的proxy_next_upstream参数定义的状态码时，会将这个请求转发给正常工作的后端服务器，如500、502、503，此参数可以提升用户访问体验。

```sh
proyx_next_upstream error timeout invalid_header http_500 http_503 http_502 http_504;
```



<br>
<br/>


## 根据URL中的目录地址实现代理转发


通过Nginx实现动静分离，即通过Nginx反向代理配置规则实现让动态资源和静态资源及其他业务分别由不同的服务器解析，已解决网站性能、安全、用户体验等重要问题。


<br>

### 动静态分离

**配置upstream.conf**

```sh
upstream static {
  server 192.168.1.11;
  #或server static.com----hosts:static.com 192.168.1.11
}

upstream upload {
  server 192.168.1.22;
}

upstream default {
  server 192.168.1.33;
}


#在http中加入,注意位置
http {
  include upstream.conf;
}
```

<br/>

**配置virtual.conf**

```sh
#方案1：利用location实现

location  /static/ {
  proyx_pass http://static;
  include proyx.conf;
}

location /upload/ {
  proxy_pass http://upload;
  include proxy.conf;
}

location / {
  proxy_pass http://default;
  include proxy.conf;
}


========================================


#方案2：利用if语句实现

if ($request_uri ~* "^/static/(.*)$")
{
      proxy_pass http://static/$1;
}

if ($request_uri ~* "^/upload/(.*)$")
{
    proxy_pass http://upload/$1;
}

location / {
  proxy_pass http://default;
  include  proyx.conf;
}
```


<br>

### URL目录地址转发的应用场景

根据HTTP的URL进行转发的应用情况，被称为 第7层（应用层）的负载均衡；而LVS的负载均衡一般用于TCP等的转发，因此被称为第四层（传输层）的负载均衡 。

有时因为需求，需要在代理服务器上通过配置规则，使得匹配不同规则的请求会交给不同的服务器池处理。


<br>

### 根据客户端的设备(user_agent)转发

为了让不同客户端设备用户有更好的访问体验，需要在后端架设不同服务器来满足不同的客户端访问。如PC端和移动端，移动端又有安卓、苹果、Pad等。


1. 常规4层负载均衡解决方案架构
	- 在常规4层负载均衡架构下，可以使用不同的域名来实现这个需求。
	- 如，分配移动端访问 wap.xxx.com，PC端访问www.xxx.com。
	- 通过不同域名来引导用户到指定后端服务器，但是这样就分别得记住不同的域名。

2. 第7层负载均衡解决方案
	- 在7层负载均衡架构下，对外只需要用一个域名，如www.xxx.com，然后通过获取用户请求中的设备信息$http_user_agent，根据此信息转给后端合适的服务器处理。

<br/>

**根据$user_agent转发**

```sh
location / {
  if ($http_user_agent ~* "android")
      {
          proxy_pass http://android;
      }

if ($http_user_agent ~* "iphone")
      {
          proxy_pass http://iphone;
      }
proxy_pass http://default;
include proyx.conf;
```

**根据文件扩展名实现代理转发**

```sh
location ~*  .*\.(gif|jpg|png|css|js)$ {
  proyx_pass http://static;
  include proxy.conf;
}

#if
if ($request_uri ~* ".*\.php$")
  {
      proxy_pass http://php;
  }
if ($request_uri ~* ".*\.(jpg|png|css|js)$")
  {
      proxy_pass http://static;
  }
```

<br/>

在开发无法通过程序实现动静分离的时候，运维可以根据资源实体进行动静分离，根据不同实现策略制定后端服务器不同的组。在前端代理服务器上通过路径、扩展名等进行规则匹配，从而实现请求的动态分离。




<br>
<br/>


## Nginx负载均衡检测节点状态

淘宝技术团队开发了一个Tengine（Nginx分支）模块nginx_upstream_check_module: <https://github.com/yaoweibin/nginx_upstream_check_module>，用于提供主动式后端服务器健康检查。
通过它检测后端realserver的健康状态，如果后端节点不可用，则所有的请求就不会转发到该节点上。

Nginx需要通过打补丁的方式将该模块添加进去。


```sh
wget https://codeload.github.com/yaoweibin/nginx_upstream_check_module/zip/master
unzip master
cd nginx_upstream_check_module-master    #解压后的文件夹

cd  nginx源码安装包（我是 /usr/local/nginx-1.12.1）
patch -p1 < ../nginx_upstream_check_module-master/check_1.12.1+.patch    #选择对应的Nginx版本号，我的是1.12.1    #打补丁

#编译，注意以前的编译参数
./configure --prefix=/usr/local/nginx \
--user=nginx --group=nginx \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_gzip_static_module \
--with-http_stub_status_module \
--with-http_sub_module \
--with-pcre \
--add-module=../nginx_upstream_check_module-master

make
#给已经安装的Nginx系统打补丁不用执行make install
#make是重新生成Nginx二进制启动命令

#备份
mv /usr/local/nginx/sbin/nginx{,.bak}

#经打过补丁的Nginx二进制程序复制到/usr/local/nginx/sbin/ 下
cp /usr/local/nginx-1.12.1/objs/nginx /usr/local/nginx/sbin/

nginx -t
```

<br>

**配置nginx_upstream_check**

配置upstream.conf

```sh
upstream zhang {
    server 192.168.1.7:5678 weight=1;
    server 192.168.0.99:5678 weight=1;
    check interval=3000 rise=2 fall=5 timeout=1000 type=http;
    #每个3秒对负载均衡中所有节点检测一次，请求2次正常标记realserver状态为up；
    #如果检测5次都失败，则标记realserver状态为down，超时时间为1秒；
    #检查的协议为HTTP；
}
```

配置/status：

```sh
location /status {
    check_status;
    access_log off;
    allow 192.168.1.0/24;
    deny all;
}
```



<br>
<br/>

---

<br/>



# stream模块


Module ngx_stream_core_module: <http://nginx.org/en/docs/stream/ngx_stream_core_module.html>

nginx从1.9.0开始，新增加了一个stream模块，用来实现四层协议(tcp/udp)的转发、代理和负载均衡。
这个模块不是默认构建的，需要使用`--with-stream`参数。


```
yum install -y  nginx-mod-stream

```


这个实现四层反向代理和转发的功能真的是很强大，只需一台反向代理服务器，转发给所有后端机器。

<br>

栗子：

```
vim /etc/nginx/nginx.conf

stream{
  include /etc/nginx/stream.d/*.conf
}




##########################



cd /etc/nginx/stream.d

#转发Elasticsearch
vim elastic.conf


upstream elastic-cluster {
    server ip1:9200;
    server ip2:9200;
    xxx;
}
server {
    listen 9200;
    proxy_pass elastic-cluster;
}




#dns
vim dns.conf


upsetrem dns-cluster {
    server ip:5353;
    server dns.example.com:53;
    xxx
}

#tcp
server {
	listen port;
    proxy_pass dns-cluster;
}

#udp
server {
	listen 53 udp;
    proxy_pass dns-cluster;
}

#ipv6
server {
	listen [::1]:53;
    proxy_pass unix:/xxx/xx.socket
}







#MySQL
vim mysql.conf


upstream mysql-cluster {
	server ip:3306;
    server ip2:3306;
    xxx;
}


server {
	listen 3306;
    proxy_pass mysql-cluster;
}





#SSH转发
upstream ssh {
	server ip:22;
}

server {
	listen port;
    proxy_pass ssh;
}

```




<br/>
<br/>

---

<br/>



# 错误信息


Nginx错误日志的详细说明。

| 错误信息 | 描述 |
| - | - |
| `(13: Permission denied) while reading upstream` | xxx |
| `(98: Address already in use) while connecting to upstream` | xxx |
| `(99: Cannot assign requested address) while connecting to upstream` | xxx
| `(104: Connection reset by peer) while reading response header from upstream` | upstream-fastcgi超时时间`request_terminate_timeout`过小
| `(104: Connection reset by peer)` | 1: 服务器的并发连接数超过其承载量，服务器会将其中一些连接Down掉; <br> 2: 客户关掉了浏览器，而服务器还在给客户端发送数据; <br> 3: 浏览器端按了Stop |
| `(104: Connection reset by peer) while connecting to upstream` | upstream发送了RST，将连接重置 |
| `send() failed (111: Connection refused)` | xxx |
| `(111: Connection refued) while connecting to upstream` | 用户在连接时，若遇到后端upstream挂掉或不通，会收到该错误 |
| `(111: Connection refused) while reading response header from upstream` | 用户在连接成功后读取数据时，若遇到后端upstream挂掉或者不通，会收到此错误 |
| `(111: Connection refused) while sending request to upstream` | Nginx和upstream连接成功后发送数据时，若遇到后端upstream挂掉或不通，会收到该错误 |
| `(110: Connection timed out) while connecting to upstream` | Nginx连接upstream时超时 |
| `(110: Connection rimed out) while reading upstream` | Nginx读取来自upstream的响应时超时 |
| `(110: Connection timed out) while reading response header from upstream` | Nginx读取来自upstream的响应头时超时 |
| `(110: Connection timed out) while reading upstream` | Nginx读取来自upstream的响应时超时 |
| `upstream prematurely closed connection` | 请求URI的时候出现异常，是由于upstream还未返回应答给用户时，用户断掉连接造成。对系统没有影响。 |
| `upstream sent invalid header while reading response header from upstream` | upstream发送的响应头无效 |
| `upstream sent no valid HTTP/1.0 header while reading response header from upstream` | upstream发送的响应头无效 |
| `client intended to send too large body` | 用于设置允许接受的客户端请求内容的最大值，Client发送的body超过了设置 |
| `reopening logs` | 用户发送`kill -USR1`命令 |
| `gracefully shutting down` | 用户发送`kill -WINCH`命令 |
| `no servers are inside upstream` | upstream下未配置server |
| `no live upstreams while connecting to upstream` | upstream下的server全都挂了 |
| `SSL_do_handshake() failed` | SSL握手失败 |
| `SSL_write() failed(SSL) while sending to client` | xxx |
| `ngx_slab_alloc() failed: no memory in SSL session shared cache` | `ssl_session_cache`大小不够 |
| `could not add new SSL session to the session cache while SSL hanshaking` | `ssl_session_cache`大小不够 |

























































