# Filesystem Hierarchy Standard


# FHS介绍

FHS(Filesystem Hierarchy Standard)，文件系统层次化标准：<http://www.pathname.com/fhs>

<br>

FHS主要目的是希望让用户了解安装文件通常放置的目录。所以希望软件开发商、系统制定者以及维护系统的用户，都能够遵循FHS的标准。

**FHS-compliant system：**

-				|	可分享的(shareable)			   |	不可分享的(unshareable)
-				|	-				   			  |	-
不变的(static)	  |	/usr <br> /opt		 			|	/etc <br> /boot
可变的(variable) |	/var/mail <br> /var/spool/news	|	/var/run <br> /var/lock

<!--more-->


* shareable： 可分享给其他系统(主机)挂载使用；
* unshareable： 不适合分享给其他主机；
* static： 有些数据基本是不会变化的；
* variable： 进程变更的数据。


<br>
FHS针对目录树架构仅定义出三层目录下应该放置什么数据，这三个目录下所应该放置的目录也都有特定规定。

* `/`： The root filesystem, 与开机系统有关；
* `/usr`: The /usr hierarchy, Unix software resource；
* `/var`: The /var hierarchy, 与系统运行过程有关。




<br>
<br/>
# The Root Filesystem

**根目录(/)**是系统最重要的一个目录。不但所有目录都是由根目录衍生出来，同时根目录还与**系统的启动、还原、修复**等操作相关。
若系统出现问题，根目录必须要包含能够修复文件系统的程序才行。
破坏根文件系统上的数据的错误比破坏其他任何分区都要严重！

为了平衡这些考虑，建议尽可能保持根分区小。
应用程序不应在根目录中创建特殊文件或子目录！



<br>
The following dirs or symbolic-links, are required in /

目录	   	   |	  描述
-	    	|		-
`/bin`		|		必要的二进制命令
`/boot`		|		boot-loader的静态文件
`/dev`		|		设备文件
`/etc`		|		主机特定的系统配置文件
`/lib`		|		基本的共享库(shared libraries)和内核模块(kernel modules)
`/media`		|		可移除媒体的挂载点
`/mnt`		|		临时挂载文件系统的挂载点
`/opt`		|		第三方软件包放置目录
`/sbin`		|		必要的系统二进制命令
`/srv`		|		系统提供的服务数据
`/tmp`		|		临时文件
`/usr`		|		/usr层次结构
`/var`		|		/var层次结构


<br>
除了上面列出必须存在的目录，下面这些目录很也很重要。

目录			|		描述
-		 	|		 -
`/lost+found`	|		在ext文件系统里，当文件系统发生错误时，将一些遗失的片段放置到此目录下
`/home`		|		用户家目录
`/root`		|		root用户家目录
`/proc`		|		虚拟文件系统，放置的数据都在内存当中，不占磁盘空间
`/sys`		|		虚拟文件系统，记录内核相关信息，不占磁盘空间


<br>
另外需要注意的是，因为根目录与开机有关，开机过程中仅有根目录被挂载。其他分区则是在开机完成后才会持续进行挂载。
因此，根目录下与开机过程有关的目录就不能放到不同的分区中去。

如：
- `/etc`
- `/bin`
- `/sbin`
- `/dev`
- `/lib`



<br>
## /bin

`/bin`, 基本用户二进制命令文件，供所有用户（系统管理员和用户）使用。

`/bin`下不能有子目录(subdirectory)。


<br>
The following commands or symbolic-links to commands, are required in /bin

命令		|		描述
-		 |		-
`cat`		|		将文件连接到stdout的实用程序(Utility)
`chgrp`	|		更改文件所有权
`chmod`	|		更改文件访问权限
`chown`	|		更改文件所有者和和组
`cp`		|		复制文件和目录
`date`	|		打印或设置系统数据和时间
`dd`		|		转换和复制文件
`df`		|		磁盘使用情况
`dmesg`	|		打印或控制kernel消息缓冲区
`echo`	|		显示一行文本
`false`	|		do nothing, 不成功
`true`	|		do nothing, 成功
`hostname`|		系统主机名
`kill`	|		发送信号到进程
`ln`		|		在文件之间创建链接
`login`	|		在系统上开始会话
`ls`		|		列出目录内容
`mkdir`	|		创建目录
`mknod`	|		创建block或character特殊文件
`more`	|		文本翻页
`mount`	|		挂载文件系统
`umount`	|		解挂文件系统
`mv`		|		move/rename文件
`ps`		|		报告进程状态
`pwd`		|		打印当前工作目录
`rm`		|		remove文件或目录
`sed`		|		sed流编辑器
`sh`		|		Bourne command shell
`stty`	|		更改或打印终端设置
`su`		|		change uid
`sync`	|		刷新文件系统缓冲区
`uname`	|		打印系统信息


<br>
The following programs or symbolic-links to programs, must be in /bin if the corresponding-system is installed:

命令		|		描述
-		|		-
`csh`		|		The C shell(可选)
`ed`		|		编辑器(可选)
`tar`		|		tar归档(可选)
`cpio`	|		cpio归档(可选)
`gzip`	|		GNU压缩工具(可选)
`gunzip`	|		GNU解压缩工具(可选)
`netstat`	|		网络统计(可选)
`ping`	|		ICMP网络测试(可选)



<br>
## /boot

`/boot` :static file of the boot-loader

该目录包含引导过程所需所有内容，处理引导是不需要的配置文件和映射安装文件外。
因此，/boot储存kernel开始执行用户模式之前使用的数据。

**
操作系统kernel必须位于 / or /boot**



<br>
## /dev

`/dev` :device files

`/dev` 目录是特殊或设备文件的位置。



<br>
## /etc

`/etc` :host-specific system configuration

配置文件是用来控制程序操作的本地静态文件，不能是可执行的二进制文件。

<br>
The following files or symbolic-links to files, must be in /etc if the corresponding-subsystem is installed.

文件 | 描述 | 备注
- | -
csh.login | C shell登录的系统范围初始化文件 | Optional
exports | NFS文件系统访问控制列表 | Optional
fstab | 文件系统静态信息 | Optional
ftpusers | FTP守护进程用户访问控制列表 | Optional
gateways | 路由网关文件 | Optional
gettydefs | getty终端设置 | Optional
group | 用户组文件 | Optional
passwd | 密码文件 | Optional
host.conf | 解析器配置文件 | Optional
hosts | 主机域名的静态信息 | Optional
hosts.allow | Tcp-wrapper的主机访问文件 | Optional
hosts.deny | Tcp-wrapper的主机禁止文件 | Optional
hosts.equiv | rlogin, rsh, rcp的可信主机列表 | Optional
hosts.lpd | lpd的可信主机列表 | Optional
inetd.conf | inetd配置文件 | Optional
inittab | init配置文件 | inittab is no longer used when using systemd
id.so.conf | 搜索共享库的额外目录 | Optional
issue | 预登录消息和 | CentOS Linux 7(core) kernel \r on an \m
motd | 登录后信息 | Welcome to $host
mtab | 文件系统动态信息 | Optional
mtools.conf | mtools配置文件 | Optional
networks | 网络名称的静态信息 | Optional
printcap | lpd打印机功能数据库 | Optional
profile | sh shell login的系统范围初始化文件 | Optional
protocols | IP协议列表 | Optional
resolv.conf | 域名服务器解析文件 | Optional
rpc | RPC协议列表 | Optional
securetty | root登录的TTY访问控制 | Optional
shells | 有效登录shell的路径名 | Optional
syslog.conf | syslogd配置文件 | Optional


<br>
## /etc/opt

/etc/opt :/opt的配置文件

第三方应用程序软件的特定主机配置文件，必须安装在/etc/opt/<subdir> 中。


<br>
## /etc/xml

`/etc/xml` :XML的配置文件

这里安装和定义XML系统的高级参数同通用配置文件。


<br>
## /home (Optional)

`/home` :用户主目录

`/home`是一个相当标准的概念，但它显然是一个特定于站点的文件系统。设置会因主机而异。
因此，任何程序都不应该依赖这个目录。


<br/>
## /lib

`/lib` :基本的共享库和内核模块

`/lib`目录中包含引导系统和运行在根文件系统的命令，即`/bin`和`/sbin`中的命令。

<br>
至少需要包含以下文件(链接)：

文件 | 描述
- | -
`libc.so.*` | 动态链接C库
`ld*` | 执行时间 链接器/加载器


<br>
## /lib<qual> (Optional)

`/lib<qual>` : 不同格式的基本共享函数库
如：64位的`/lib64`; 32位的`/lib32`。

用来存放与/lib不同格式的二进制函数库，如支持64位的/lib64函数库等。


<br/>
## /media

`/media` :可移除媒体的挂载点

此目录包含的子目录，可作为各移动介质(USB,cdrom,floppy...)的挂载点。

尽管在 /mnt 中使用子目录作为挂载点已经很常见了，但与直接使用/mnt作为临时挂载点的传统相去甚远。


<br/>
## /mnt

`/mnt` :临时挂载文件系统的挂载点


<br>
## /opt

`/opt` :为第三方软件包保留的目录

要安装在`/opt`中的软件包必须将其静态文件放置在单独的`/opt/<packge>`目录树中。

目录/opt/bin, /opt/doc, /opt/include, /opt/info, /opt/lib, /opt/man 是保留给本地系统管理员使用。
如果第三方软件包含Unix手册，而手册必须放置于/opt/<package>/share/man/，必须使用与/usr/share/man相同的子结构。


<br/>
## /root (Optional)

`/root` :root用户的主目录


<br>
## /sbin

`/sbin` :系统二进制文件

系统管理的实用程序(命令)，存储在`/sbin, /usr/sbin, /usr/local/sbin`中。
`/sbin`包含**启动，恢复，修复**系统，以及`/bin`中二进制文件所必须的二进制文件。
本地安装的系统管理程序应放置在`/usr/local/sbin`中。

<br>
The following commands or symbolic-links to commands are required in /sbin
```
shutdown		#关闭系统
```

<br/>
The following files or symbolic-links to files，must be in /sbin if the corresponding subsystem is installed

命令 | 描述 | 备注
- | - | -
`fastboot` | 重启系统而不检查磁盘 | Optional
`fasthalt` | 停止系统而不检查磁盘 | Optional
`fdisk` | 分区表操作器 | Optional
`fsck` | 文件系统检查和修理工具 | Optional
`fsck.*` | 针对特定文件系统检查和修复 | Optional<br>eg：fsck.ext3
`getty` | getty程序 | Optional
`half` | 停止系统 | Optional
`ifconfig` | 配置网络接口 | Optional
`init` | 初始化进程 | Optional
`mkfs` | 创建文件系统 | Optional
`mkfs.*` | 创建特定文件系统 | OPtional<br>eg: mkfs.ext4
`mkswap` | 设置swap分区 | OPtional
`reboot` | 重启系统 | OPtional
`route` | IP路由表实用程序 | OPtional
`swapon` | 启用分页和交换 | OPtional
`swapoff` | Disable paging and swapping | Optional
`update` | 守护进程定期刷新文件系统缓冲区 | Optional


<br>
## /srv

`/srv` :系统提供的服务(service)的数据


<br>
## /tmp

`/tmp` :临时文件

`/tmp`目录为临时需要文件的程序提供。
程序不能在程序的调用之间保留/tmp中的任何文件或目录。
尽管/tmp中数据可能会以某种特定方式删除，但建议在系统启动时删除/tmp中所有文件。




<br>
<br/>
# The /usr Hierarchy

`/usr` 里面放置的数据是可分享与不可变动的。
这就意味着可在各种符合FHS的主机之间共享，但不能写入。
大型软件包不应在/usr层次结构下使用直接子目录。

<br>
The following dirs of symbolic-links to dirs are required in /usr

目录 | 描述
- | -
`/usr/bin` | 大多数用户命令
`/usr/include` | C程序包含的头文件
`/usr/lib` | 库文件
`/usr/local` | 本地层次结构
`/usr/sbin` | 非重要的系统二进制文件
`/usr/share` | 独立于架构的数据


其他选项：

目录| 描述 | 备注
- | - | -
`/usr/lib<qual>` | 可选格式库 | Optional
`/usr/src` | 源代码 | OPtional
`/usr/games` | 游戏和教育二进制文件 | OPtional


<br>
## /usr/bin

`/usr/bin` :大多数用户命令
这是系统上可执行命令的主要目录。

<br>
The following files or symbolic-links to files must be in /usr/bin, if the corresponding subsystem is installed

命令 | 描述 | 备注
- | - | -
`perl` | 实用提取和报告语言 | OPtional
`python` | python解释语言 | Optional
`tclsh` | tcl解释器的简单shell | OPtional
`wish` | 简单 tcl/tk windowing shell | Optional
`expect` | 程序交互式对话 | Optional

因为shell script解释器(在shell script脚本的第一行 #!<path>)不能依赖路径，所以标准化它们的位置是有利的。
Bourne shell 和 C-shell解释器已经被固定在/bin中，但 perl,python,tcl经常在许多不同的地方。


<br>
## /usr/include

`/usr/include` :标准C包含文件的目录

这是C语言所有系统的通用包含文件应该被放置的地方。


<br>
## /usr/lib

`/usr/lib` :编程和包的所需要的库

`/usr/lib`包括 不打算由用户或shell script直接执行的目标文件、库和内部二进制文件。


<br>
## /usr/lib<qual> (Optional)

`/usr/lib<qual>` :可选格式库


<br>
## /usr/local

`/usr/local` :本地层次结构

`/usr/local`是给系统管理员安装本地软件使用。当系统软件更新时，需保证安全。
它可以用于在一组主机之间共享，但在 `usr`中找不到的程序和数据。

本地安装软件必须放在 `/usr/local` 而不是 `/usr`，除非安装它来升级或替换`usr`的软件

<br>
The following dirs or symbolic-links to dis must be in `/usr/local`

目录 | 描述 |
- | -
`/usr/local/bin` | 本地二进制文件
`/usr/local/etc` | 本地二进制文件的特定配置文件
`/usr/local/games` | 本地游戏二进制文件
`/usr/local/include` | 本地C头文件
`/usr/local/lib` | 本地库
`/usr/local/man` | 本地在线手册
`/usr/local/sbin` | 本地系统二进制文件
`/usr/local/share` | 本地独立架构层次结构
`/usr/local/src` | 本地源码


`/usr/local/share`目录内容的要求应与`/usr/share`相同，唯一附加约束是`/usr/local/share/man`和`/usr/local/man`目录必须是同步的。（基本上就是符号链接了！）


<br>
## /usr/sbin

`/usr/sbin` :非必要的标准系统二进制文件

该目录包含系统管理员专门使用的任何非必要的二进制文件。
系统修复、恢复、挂载/usr等其他重要必要功能必须放在/sbin中。


<br>
## /usr/share

`/usr/share` :独立于架构的数据

`/usr/share`层次 是为了所有只读架构独立数据。
该层次可以在给定OS的所有体系架构平台之间共享。如具有i386和PPC平台站点可能会维护一个集中安装的/usr/share目录。
但`/usr/share`一般不打算由不同的操作系统共享，或由同一操作系统的不同版本共享。

<br>
The following dis or symbolic-links to dirs must be in /usr/share

目录 | 描述
- | -
man | 在线手册
misc | 其他独立于架构的数据

<br>
The following dis or symbolic-links to dirs must be in /usr/share, if the corresponding subsystem is installed

目录 | 描述 | 备注
- | - | -
dict | 单词列表 | Optional
doc | 各种文档 | Optional
games | /usr/games的静态文件 | Optional
info | GNU Info system's primary dir | Optional
locale | 支持的区域信息 | Optional
zoneinfo | Timezone info and conf | Optional
NLS | Native language support | Optional
sgml | SGML数据 | Optional
terminfo | terminfo数据库目录 | Optional
xml | xml数据 | Optional


<br/>
### /usr/share/dict

`/usr/share/dict` :单词列表
这个目录是系统上单词列表的家目录，只包含英文单词，它们由`look`和各种拼写程序使用。
它们是所有拼写检查器唯一通用的文件。

文件 | 描述 | 备注
- | - | -
words | 单词列表 | Optional
linu.words | linux可用单词列表 | Optional


<br/>
### /usr/share/man

`/usr/share/man` :手册页
它包含了`/`, `/usr`文件系统下的命令和数据的手册信息

手册页存储在 `/usr/share/man/<locale>/man<section>/<arch>`中。

每个部分的描述：
- man1: 可公开访问的命令的手册页，用户需要使用的大多数程序文档放置于此；
- man2: 系统调用部分，描述所有的系统调用(请求内核执行操作)；
- man3: 函数库和子例程部分，描述不直接调用内核服务的程序库例程；
- man4: 特定文件部分，描述系统中特定文件，相关驱动程序和网络支持。通常，这包含/dev中找到的设备以及网络协议支持的内核接口；
- man5: 文件格式部分，许多数据文件的格式记录在此；
- man6: 游戏，演示和一般小程序；
- man7: 各种难以分类的手册页；
- man8: 系统管理员用于操作和维护系统的程序记录在这。

<br>
The following dirs or symboli-link to dirs must be in `/usr/share/man/<locale>`, unless they are empty

目录 | 描述 | 备注
- | - | -
man1 | 用户程序 | Optional
man2 | 系统调用 | Optional
man3 | 函数库调用 | Optional
man4 | 特定文件 | Optional
man5 | 文件格式 | Optional
man6 | 游戏 | Optional
man7 | 混杂的手册页 | Optional
man8 | 系统管理 | Optional

<br/>
必须在`/usr/share/man`结构中作出规定，以支持用不同语言编写的手册页。这些规定必须考虑到手册页的存储和参考，相关因素包括语言和字符编码集。

栗子：

Language | Country | CharacterSet | Dir
- | - | - | -
English | - | ASCII | /usr/share/man/en
English | United Kingdom | ISO 8859-15 | /usr/share/man/en_GB
English | United States | ASCII | /usr/share/man/en_US


<br>
### /usr/share/misc

`/usr/share/misc` :与架构无关的数据


<br/>
### /usr/share/sgml

`/usr/share/sgml` :SGML数据


<br>
### /usr/share/xml

`/usr/share/xml` :XML数据



<br/>
## /usr/src

`/usr/src` :源代码
Source Code可能放置在此目录的子目录中，仅供参考。




<br>
<br/>
# /var Hierarchy

`/var` 包含可变数据文件，包括假脱机目录和文件，系统管理和登录数据，以及临时文件。

如果`/var`不能成为一个单独的分区，最好将`/var`移出`/`分区并移入`/usr`分区。（为了减小根分区大小或当根分区空间不足时）
也可将`/var`链接到`/usr/var`。

<br/>
The following dirs or symbolic-link to dirs are required in `/var`.

目录 | 描述
- | -
`/var/cache` | 应用程序缓存数据
`/var/lib` | 可变状态信息
`/var/local` | /usr/local的可变数据
`/var/lock` | 锁文件
`/var/log` | 日志文件
`/var/opt` | /opt的可变数据
`/var/run` | 与运行进程相关的数据
`/var/spool` | 应用程序队列数据
`/var/tmp` | 为系统重启保留的临时文件

<br>
The following dirs or symbolic-link to dir must be in /var,if the corresponding subsystem is installed.

目录 | 描述 | 备注
- | - | -
`/var/account` | 进程账户日志 | 可选
`/var/crash` | 系统奔溃转储 | 可选
`/var/games` | 可变游戏数据 | 可选
`/var/mail` | 用户邮箱文件 | 可选
`/var/yp` | 网络信息服务数据库文件



<br/>
## /var/account

`/var/account` :该目录保存当前活动的进程记账日志和复合进程数据。


<br/>
## /var/cache

`/var/cache` :保存应用程序缓存的数据。应用程序必须能够重新生成或回复数据。
与`/var/spool`不同，删除了缓存文件不会丢失数据。数据必须在应用程序调用和系统重启间保持有效。
缓存目录的数据格式没有其他要求。

对于缓存数据单独存在的目录，系统管理员可从`/var`下其他目录设备不同的磁盘和备份策略。

目录 | 描述 | 备注
- | - | -
`/var/cache/fonts` | 本地生成的字体 | 可选
`/var/cache/man` | 本地格式化的手册页 | 可选
`/var/cache/www` | www代理或缓存数据 | 可选
`/var/cache/<package>` | 特定包缓存数据 | 可选


<br/>
## /var/lib

`/var/lib` :可变状态信息。目录保存于应用程序或系统有关的状态信息。
状态信息(state infofmation)，是程序在运行时修改的数据，属于一个特定的主机。

应用程序必须为其数据使用`/var/lib/<subdir>`，有一个必须的子目录`/var/lib/misc`用于不需要子目录的状态文件。


<br/>
## /var/lock

`/var/lock` :锁文件，锁文件应该存储在此目录中。
锁文件锁定多个应用程序共享的设备和其他资源。

这种锁文件内容的格式必须是HDB UUCP锁文件格式。
HDB格式是将进程标识符(PID)存储为ASCII十进制数，并带有换行符。


<br>
## /var/log

`/var/log` :日志文件和目录，大多数日志必须写入此目录或适当子目录。

<br>
The following file or symbolic-link to file must be in /var/log.

文件 | 描述
- | -
lastlog | 每个用户上次登录信息的记录
message | syslogd的系统信息
wtmp | 所有登录和注销的记录


<br>
## /var/mail

邮件缓存区必须通过`/var/mail`访问，邮件缓冲区文件必须采用<username>的形式。


<br/>
## /var/run

`/var/run` :运行时变化数据，此目录包含系统信息数据，描述系统启动以来的情况。
此目录下的文件必须在引导过程开始时被清除。
进程标识符(PID)文件放置于此目录或下的子目录里面。


<br/>
## /var/spool

`/var/spool` :应用程序队列数据。
此目录包含正在等待某种稍后处理的数据，/var/spool中的数据表示工作将在将来执行(通过程序，用户或管理员)，数据通常会在工作处理后被删除。

<br>
The following dirs or symbolic-link to dirs must be in /var/spool,if the corresponding subsystem is installed.

目录 | 描述 | 备注
- | - | -
lpd | 打印机队列目录 | 可选
mqueue | 发送邮件队列 | 可选
news | 新闻假脱机目录 | 可选
rwho | rwhod文件 | 可选
uucp | uucp的假脱机目录 | 可选


<br>
## /var/tmp

`/var/tmp` :在系统重启之间保存的临时文件。存储在`/var/tmp`的数据比`/tmp`中的数据更持久。




<br>
<br/>
# OS Specific Annex

本节是针对仅适用于特定OS的其他建议和要求。


## Linux

Linux操作系统的附件


### / :根目录

在Linux系统上，如果内核位于`/`，建议使用Linux内核源代码包中使用的名称**vmlinux或vmlinuz**。

我的`CentOS7`中，内核文件默认是`/boot/vmlinuz-$kernel-version.$arch`


<br>
### /bin :基本用户命令二进制文件(供多有用户使用)


<br/>
### /dev :设备和特殊文件

1. `/dev/null` :
	写入该设备的所有数据都被丢弃。从这个设备读取将返回一个EOF条件。
2. `/dev/zero` :
	该设备是归零数据的来源，写入该设备的所有数据被丢弃。从这个设备读取将返回包含zero的请求的字节数。
3. `/dev/tty` :
	该设备类似于进程控制终端。一旦这个设备被打开，所有读写操作就好像实际的控制终端以及被打开一样。


<br>
### /etc :主机的特定系统配置

Linux系统要将附件文件放置到`/etc`中。


<br/>
### /lib64 和 /lib32 :64/32位库(依赖于体系结构)

64位体系结构PPC64,AMD64,x86_64必须将64位库放置于`/lib64`中，将32位库放置于`/lib`中；
64位体系结构IA64必须将64位库放置于`/lib`中。


<br>
### /proc :内核和进程信息虚拟文件系统

PROC文件系统是用于处理进程和系统信息的标准Linux方法，而不是/dev/kmem和其它类似方法。
强烈建议使用PROC文件系统获取 存储，进程，内存，内核等信息。


<br/>
### /sbin :基本系统二进制文件

Linux系统将这些附加文件放置于`/sbin`中：

**第二扩展文件系统命令（可选）：**

```
badblocks
dumpe2fs
e2fsck
mke2fs
mklost+found
tune2fs
```

**boot-loader 映射安装程序（可选）：**
`lilo`

**静态二进制文件：**

```
ldconfig
sln(static ln)
ssync(static sync)
```

> 出现问题时，sln（静态ln）和ssync（静态同步）非常有用；
> idconfig程序可以作为升级知道的手段；
> sln的主要用途，修复不良协调升级后/lib中不正确的符号链接动态库。

对于/sbin, idconfig二进制文件是可选的。因为站点可能会在启动时选择运行idconfig而不是仅在升级共享库时。
以下是一些常见问题：
- 我刚刚删除了/lib/<file>；
- 我无法找到库的名称，因为`ls`是动态链接。我使用的shell没有内置`ls`，我也不知道使用`echo *`作为替换；
- 我有一个静态ln，但我不知道怎么称呼这个链接。


**杂项：**

```
#ctrl+alt+del
ctrlaltdel

#keyboard rate
kbdrate
```

为了应对某些键盘出现如此高的重复速率一致无法使用,`kbdrate`可以安装在某些系统上的`/sbin`中；

由于`ctrl+alt+del`组合键在内核中的默认操作是**硬重启**，因此通常建议在将根文件系统挂在到读写模式之前禁用该行为。这就可能需要`ctrlaltdel`程序，它可以安装在系统的`/sbin`中。


<br>
## /usr/include :C程序包含的头文件

如果安装了C或C++编译器，则只有非 基于glibc的系统才需要这些链接符号。

```
/usr/include/asm  -> /usr/src/linux/include/asm-<arch>
/usr/include/linux  -> /usr/src/linux/include/linux
```


<br>
## /usr/src :源代码

对于基于glibc的系统，此目录没有具体指导。

对于glibc之前基于linux libc修订版的系统： `/usr/src/linux`是唯一放置Linux内核源代码的位置。


<br>
## /usr/spool/cron :cron和jobs

此目录包含了cron和程序的可变数据。






































